---
title: Estratégias de implantação de deploy
description: Introdução aos tipos de deploy e seus tradeoffs.
authors:
  - fernando_santini
date:
  created: 2026-04-07
  updated: 2026-04-07
---

# Estratégias de implantação de deploy

A estratégia de implantação é o método que uma equipe usa para levar um novo código para o ambiente de produção orquestrada por Devops e devs; Ela determina como o tráfego é alternado entre versões, quanto risco cada versão representa e a rapidez com que a equipe pode reverter (rollback) quando algo falha. A escolha não é acadêmica: uma incompatibilidade entre estratégia e sistema pode significar indisponibilidade (downtime), lançamentos falhos ou horas de recuperação manual.

**Implantar** significa colocar o código em um ambiente. 
**Liberar** (release) significa expor esse código aos usuários. 

Algumas estratégias tratam esses dois eventos como um só. Uma atualização contínua (rolling update) substitui as instâncias, e os usuários acessam a nova versão à medida que cada uma entra no ar. Outras estratégias os separam deliberadamente. Feature flags (sinalizadores de funcionalidade) permitem que uma equipe implante na terça-feira e libere na quinta-feira, após validação interna. Essa separação molda todas as seções seguintes.

### Quais são os diferentes tipos de estratégias de implantação?

Cada estratégia faz uma troca entre duas coisas: quanto risco ela impõe à produção e quanto esforço é necessário para configurá-la. Uma implantação do tipo "big bang" é extremamente simples, mas derruba todo o sistema. Uma implantação canário (canary) mal toca no tráfego de produção, mas precisa de divisão de tráfego, coleta de métricas e análise automatizada para funcionar. Nenhum dos eixos é melhor. A combinação certa depende do que está sendo implantado e do que quebra se algo der errado.

---

## Implantação canário (canary deployment)

Uma implantação canário envia uma pequena porcentagem do tráfego de produção — tipicamente de 1 a 5% — para a nova versão, enquanto o restante continua acessando a versão atual. Se as métricas do canário estiverem saudáveis, o tráfego aumenta gradualmente. Se as métricas piorarem, todo o tráfego retorna para a versão atual.

**Como funciona:** Uma camada de gerenciamento de tráfego (um controlador de ingresso, service mesh ou Argo Rollouts) roteia uma porcentagem definida de requisições para a nova versão. Sistemas de monitoramento coletam métricas tanto do canário quanto da linha de base (baseline). Após uma janela de observação definida, o tráfego aumenta ou sofre rollback.

**Custos e complexidade:** O canário custa menos que o blue-green porque só precisa de instâncias suficientes para lidar com sua parcela de tráfego. O custo de complexidade é maior: exige infraestrutura de divisão de tráfego, comparação de métricas em tempo real e análise automatizada. Sem isso, é apenas uma implantação contínua com passos extras. *Kayenta* (construído pelo Google e Netflix) e *Argo Rollouts* automatizam a decisão de promover ou reverter com base na análise estatística das métricas do canário.

---

## Implantação big bang (big bang deployment)

Uma implantação big bang interrompe todas as instâncias em execução da versão atual antes de iniciar as instâncias da nova versão. Não há divisão de tráfego, nem coexistência de código antigo e novo, nem lançamento gradual. No Kubernetes, isso é `strategy.type: recreate`.

**Como funciona:** O controlador de implantação encerra todos os pods que executam a versão antiga e, em seguida, inicia os pods com a nova versão. O tráfego só retorna depois que os novos pods passam em suas verificações de prontidão (readiness checks).

**A troca é direta:** simplicidade em troca de indisponibilidade. Não há exigência de compatibilidade reversa, mas os usuários ficam bloqueados durante todo o processo, que pode variar de segundos a minutos dependendo do tempo de inicialização da aplicação.

**Use big bang quando a indisponibilidade for aceitável:** ambientes de desenvolvimento e staging, janelas de manutenção programada ou situações onde uma mudança crítica (breaking change) torna impossível executar duas versões simultaneamente.

---

## Implantação contínua (rolling deployment)

Uma implantação contínua atualiza as instâncias em lotes enquanto a aplicação permanece no ar. No Kubernetes, essa é a estratégia padrão (`strategy.type: RollingUpdate`), controlada por `maxUnavailable` e `maxSurge`.

**Como funciona:** O controlador de implantação escala um novo ReplicaSet em lotes enquanto reduz o antigo, verificando as sondas de prontidão (readiness probes) a cada passo.

**Vantagens e custos:** As implantações contínuas são eficientes em termos de recursos porque não exigem um ambiente duplicado. O custo é a coexistência de versões: versões antigas e novas servem tráfego simultaneamente, portanto a aplicação deve ser compatível com versões anteriores (backward-compatible). A reversão (rollback) também não é instantânea. Reverter significa fazer o rollout na direção oposta.

**Use implantações contínuas para mudanças compatíveis com versões anteriores onde o custo de infraestrutura importa.** Para a maioria das equipes que não possuem infraestrutura de divisão de tráfego, é um ponto de partida razoável.

---

## Implantação blue-green (blue-green deployment)

Uma implantação blue-green mantém dois ambientes de produção idênticos. Um (azul/blue) serve todo o tráfego. O outro (verde/green) fica ocioso. Para implantar, a equipe envia a nova versão para o verde, valida e então alterna o tráfego. Se algo der errado, o tráfego volta para o azul em menos de um minuto.

**Como funciona:** A nova versão é implantada no ambiente ocioso. A equipe executa testes de smoke ou testes de integração contra ele. Após a validação, ocorre a alternância de tráfego: uma mudança na regra do balanceador de carga, uma atualização do seletor de serviço no Kubernetes ou uma mudança de peso no DNS. Se algo falhar após a alternância, o mesmo mecanismo direciona o tráfego de volta para o azul.

**Custos e desafios:** O custo mais visível é o dobro da capacidade computacional. O custo menos óbvio é o banco de dados. Ambos os ambientes compartilham o mesmo banco de dados, portanto as mudanças de esquema devem funcionar com ambas as versões da aplicação simultaneamente. É aqui que a maioria das falhas blue-green acontece, e onde o padrão "expand-migrate-contract" (expandir-migrar-contratar) é importante.

- Na fase **expandir**, adicione novas colunas ou tabelas junto com as existentes, sem remover nada.
- Na fase **migrar**, faça o backfill dos dados existentes para as novas estruturas.
- Na fase **contratar**, depois que todo o tráfego estiver rodando no novo código, remova as colunas antigas.
Cada fase é uma implantação separada, e a restrição durante todo o processo é a compatibilidade N-1: a nova versão da aplicação deve funcionar com o esquema anterior, e vice-versa.

**Use blue-green quando a velocidade de reversão for um requisito obrigatório e a equipe puder arcar com o custo de infraestrutura.**

---

## Implantação para teste A/B (A/B testing deployment)

Uma implantação para teste A/B direciona o tráfego para diferentes versões da aplicação com base em segmentos de usuários para medir métricas de negócio, como taxa de conversão, receita por sessão ou engajamento. Diferente do canário (divisão aleatória por segurança), o teste A/B visa grupos específicos de usuários para responder a uma pergunta de negócio.

**Como funciona:** A equipe define uma regra de segmentação que inclui usuários de uma região específica, usuários em dispositivos móveis ou uma porcentagem atribuída por uma função de hash consistente. Ambas as versões rodam simultaneamente enquanto um sistema de análise coleta as métricas alvo. Quando o experimento atinge significância estatística, a versão vencedora é lançada para todos os usuários.

**Observação importante:** Teste A/B é uma técnica de experimentação, não uma estratégia de mitigação de risco. Ele assume que ambas as versões são estáveis e prontas para produção. O perfil de risco é maior do que no canário porque não há reversão automática acionada por taxas de erro ou latência.

**Use teste A/B quando o objetivo for otimização, não segurança.** Requer uma camada de segmentação, um pipeline de análise e volume de tráfego suficiente para atingir significância estatística.

---

## Implantação sombra (shadow deployment)

Uma implantação sombra espelha o tráfego real de produção para uma nova versão executada ao lado da versão atual. A versão atual lida com todas as respostas voltadas ao usuário. A versão sombra processa as mesmas requisições, mas suas respostas nunca são servidas aos usuários. Elas são registradas (log) e comparadas com a saída da produção como validação.

**Como funciona:** Uma camada de espelhamento de tráfego duplica as requisições recebidas e envia cópias para o ambiente sombra. As respostas são comparadas com as respostas da versão de produção, em tempo real ou em lote.

**Segurança e limitações:** Implantações sombra são a maneira mais segura de testar com tráfego real porque os usuários nunca são afetados. Isso as torna uma ótima opção para trocas de modelos de ML, reescritas de algoritmos e validação de migração de banco de dados. A limitação principal são operações com estado (stateful): se a versão sombra escreve em um banco de dados ou processa pagamentos, essas operações são executadas duas vezes. Espelhar um serviço com efeitos colaterais de escrita exige filtrar esses caminhos, o que reduz a fidelidade do teste.

---

## Implantação com feature flags (feature flag deployment)

Uma implantação com feature flags desacopla a implantação do código da sua liberação para os usuários. O novo código é enviado para produção atrás de uma flag que padrão é "desligada". A equipe liga a flag para usuários específicos, porcentagens ou segmentos quando estiver pronta. Se algo der errado, desligar a flag esconde a mudança instantaneamente.

**Como funciona:** Uma plataforma de feature flags (LaunchDarkly, Unleash, Flagsmith ou uma solução caseira) armazena as configurações das flags e as entrega à aplicação via um SDK. Quando um engenheiro alterna uma flag, a mudança se propaga em segundos. Sem nova implantação, sem reinicialização de pod, sem alteração no balanceador de carga.

**Benefícios e desvantagens:** Feature flags permitem desenvolvimento baseado em trunk (trunk-based development), dogfooding interno e reversão instantânea no nível da aplicação. Elas combinam naturalmente com qualquer outra estratégia desta lista. A troca é a "dívida de flags": flags que permanecem no código depois que a funcionalidade foi lançada criam bagunça e aumentam a superfície de teste. A solução é disciplina: defina uma data de expiração para cada flag e trate a remoção como parte da definição de "pronto" (definition of done) da funcionalidade.

---

## Implantação imutável (immutable deployment)

Uma implantação imutável substitui a infraestrutura por completo, em vez de modificá-la no lugar. Cada mudança produz um novo artefato (imagem de container, imagem de VM, AMI) implantado em instâncias novas. Nenhuma instância é corrigida (patched) ou reconfigurada após começar a executar.

**Como funciona:** O pipeline de CI constrói a aplicação, empacota-a em um artefato imutável e a envia para um registro (registry). Novas instâncias são criadas a partir desse artefato. Instâncias antigas são encerradas.

**Benefício principal:** Imutabilidade resolve o desvio de configuração (configuration drift) — a divergência gradual entre o que uma instância deveria estar executando e o que ela realmente executa. Containers impõem isso por design, mas o princípio se estende a AMIs construídas com Packer e configurações baseadas em Nix. A AWS inclui infraestrutura imutável em seu Well-Architected Framework. O custo é um processo de build mais rigoroso, onde cada mudança exige um ciclo completo de build-test-deploy.

**Imutabilidade não é uma escolha de roteamento de tráfego.** É uma decisão sobre como os artefatos são construídos, e combina com qualquer outra estratégia desta lista.

---

## Implantação baseada em GitOps (GitOps-based deployment)

Uma implantação GitOps usa um repositório Git como a única fonte de verdade (single source of truth) para o que deve estar em execução em cada ambiente. Um agente executando dentro do cluster (ArgoCD, Flux) compara continuamente o estado vivo com o estado declarado no Git e reconcilia qualquer desvio.

**Como funciona:** A equipe armazena manifests do Kubernetes, charts Helm ou overlays Kustomize em um repositório Git. Quando um engenheiro faz merge de um pull request, o agente detecta o novo commit e o aplica. Se alguém editar manualmente um recurso no cluster, o agente o reverte para corresponder ao Git.

**Princípios e escopo:** O projeto OpenGitOps define quatro princípios: o estado desejado é declarativo, versionado e imutável no Git, puxado automaticamente por agentes e continuamente reconciliado. O modelo pull-based significa que as credenciais do cluster nunca saem do cluster.

**A troca é o escopo:** ArgoCD e Flux são construídos especificamente para Kubernetes. Equipes que usam VMs, serverless ou ambientes híbridos podem adotar os princípios, mas não terão a mesma reconciliação pronta para uso.

---

### Como escolher a estratégia de implantação certa

Comparar estratégias de implantação nos dá uma visão geral de como lidar com implantações. Mas também precisamos considerar fatores específicos do seu projeto e seus requisitos. Vamos examinar cinco fatores cruciais que ajudarão a alinhar seu projeto à estratégia certa.

1. **Tolerância a indisponibilidade (downtime).** O sistema pode ficar offline durante uma implantação? Se sim, mesmo que brevemente durante uma janela de manutenção, big bang ainda é uma opção. Se não, elimine big bang. Se o sistema não tolerar nem mesmo uma breve coexistência de versões durante um rollout, as opções restantes são blue-green, canário ou feature flags.

2. **Orçamento de infraestrutura.** A equipe pode executar um ambiente de produção duplicado completo? Se sim, blue-green funciona. Se não, canário é mais adequado porque só precisa de instâncias suficientes para lidar com sua parcela de tráfego. Rolling é o mais barato de todos, pois reutiliza a capacidade existente. Orçamento elimina blue-green para muitas equipes.

3. **Maturidade da equipe e ferramentas.** A equipe tem infraestrutura de divisão de tráfego (service mesh, ingress com peso, ou Argo Rollouts)? Consegue executar comparação automatizada de métricas entre canário e linha de base? Se a resposta para qualquer uma for não, comece com implantações contínuas e feature flags. Evolua para canário quando as ferramentas existirem. Canário sem infraestrutura de monitoramento significa alguém observando dashboards manualmente para cada release — e isso não escala.

4. **Arquitetura.** Um monólito é implantado como uma unidade única, o que torna blue-green ou rolling as opções mais simples. Microsserviços se beneficiam de canário ou entrega progressiva (progressive delivery), onde cada serviço é lançado independentemente em seu próprio cronograma. Funções serverless são melhor controladas por feature flags, já que o provedor de nuvem gerencia a infraestrutura e não há instâncias entre as quais rotear.

5. **Requisitos de conformidade e auditoria.** Indústrias reguladas geralmente precisam de uma trilha de auditoria completa do que foi implantado, quando e por quem. GitOps fornece isso por padrão: cada mudança é um commit no Git com um autor e uma trilha de revisão. Alguns frameworks de conformidade exigem validação do ambiente completo antes de uma alternância de tráfego, o que favorece blue-green. Se a equipe opera sob SOC 2, HIPAA ou PCI-DSS, a estratégia de implantação precisa satisfazer o auditor, não apenas os engenheiros.

Na prática, a maioria das equipes não dependerá de uma única estratégia de implantação. Lembre-se de que essas estratégias não servem para todos. Escolha sabiamente com base em suas necessidades específicas e que suas implantações sejam tão tranquilas quanto o canto de um canário!