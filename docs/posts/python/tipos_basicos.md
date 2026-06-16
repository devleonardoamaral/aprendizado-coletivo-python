---
title: Tipos Básicos
description: Introdução aos tipos básicos do Python.
authors:
  - leonardo_amaral
date:
  created: 2026-04-04
  updated: 2026-06-16
---

## O que são tipos?

Os tipos são classificações que definem como os dados são guardados na memória. Eles são fundamentais para que o Python saiba como interpretar, manipular e validar cada informação.

Pense no tipo como a etiqueta de uma caixa. Se a etiqueta diz "vidro frágil", você sabe que precisa carregar com cuidado. Se diz "pesado", sabe que vai precisar de força. O tipo funciona assim: ele avisa ao Python como "manusear" cada dado.

Veja na prática:

| O que você escreve | O que o Python entende | Resultado |
|--------------------|------------------------|-----------|
| `1 + 2` | Dois números | `3` (soma) |
| `'1' + '2'` | Dois textos | `'12'` (junção) |

Mesmo símbolo (`+`), comportamentos diferentes — tudo por causa do **tipo**.

<!-- more -->

Entender tipos bem ajuda a escrever programas mais coerentes e evita erros comuns na manipulação de dados.

---

## Tipos básicos

O Python tem dezenas de tipos, mas vamos começar pelos **quatro fundamentais**:

| Tipo | Nome | Guarda... | Exemplo |
|------|------|-----------|---------|
| `int` | Número inteiro | Contagens, quantidades | `10`, `-5`, `0` |
| `float` | Número decimal | Medidas, porcentagens | `3.14`, `-0.5` |
| `str` | Texto | Nomes, mensagens | `'Olá'`, `"Python"` |
| `bool` | Booleano | Verdadeiro/falso | `True`, `False` |

Os tipos básicos são **simples**: você trata cada um como uma coisa só. O número `10` é só `10`. O texto `'Olá'` é só `'Olá'`. Isso muda nos **tipos compostos**; como a lista `[1, 'Olá', True]`, que guarda vários valores de uma vez.

### Números (`int` e `float`)

Python divide números em dois tipos:

| Tipo | Quando usar | Exemplo | Cuidado |
|------|-------------|---------|---------|
| `int` | Contagens inteiras | `10`, `-5` |  |
| `float` | Medidas com decimais, porcentagens | `3.14`, `-0.5` | Use **ponto**, não vírgula: certo `3.14`, errado `3,14` |

```py
# int
idade = 25
print(type(idade))  # Saída: <class 'int>

# float
altura = 1.75
print(type(altura))  # Saída: <class 'float'>
```

### Booleanos (`bool`)

Representa **sim/não**, **ligado/desligado**, **verdadeiro/falso**:

```py
esta_chovendo = True
tem_ingresso = False
print(type(tem_ingresso)) # Saída: <class 'bool'>
```

> ⚠️ **Atenção:** Python é exigente com maiúsculas. `True` e `False` sempre começam com maiúscula. `true` ou `false` dão erro.

### Texto (`str`)

Textos ficam entre **aspas simples** (`'`) ou **duplas** (`"`), as duas funcionam igual:

```py
nome = "Maria"
mensagem = 'Olá, tudo bem?'
print(type(mensagem)) # Saída: <class 'str'>
```

### Ausência de valor (`None`)

Representa **"ainda não sei"** ou **"nada aqui"**:

```py
# Útil quando você precisa criar a variável agora,
# mas só vai ter o valor depois
nome_usuario = None
print(nome_usuario)        # Saída: None
print(type(nome_usuario))  # Saída: <class 'NoneType'>

# Mais tarde no programa...
nome_usuario = "Ana"
print(type(nome_usuario))  # Saída: <class 'str'>
```

> ⚠️ **Atenção:** `None` não é zero (`0`), não é texto vazio (`''`). É literalmente a ausência de qualquer valor.

## Próximos passos

Como vimos nesse artigo, operações como a soma (`+`) podem ter comportamentos diferentes dependendo dos tipos envolvidos. Por isso, às vezes é necessário converter um valor de um tipo para outro. Leia [Conversão de Tipos](conversao_tipos.md).
