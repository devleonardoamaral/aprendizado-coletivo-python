---
title: Operadores
description: Introdução aos operadores do Python.
authors:
  - leonardo_amaral
date:
  created: 2026-06-18
  updated: 2026-06-18
links:
  Python Docs - Operadores: https://docs.python.org/pt-br/3.14/library/operator.html
  Python Docs - Expressões: https://docs.python.org/pt-br/3.14/reference/expressions.html
---

## O que são operadores?

Operadores são símbolos que dizem como a linguagem deve realizar as operações matemáticas, lógicas e comparações.

<!-- more -->

## Operadores Aritméticos

São usados para realizar cálculos matemáticos. Existem dois tipos de operadores aritméticos: os unários, executam a operação com um operando; e os binários, que executam a operação com dois operandos.

### Tabela Operadores Aritméticos

| Operador | Operação | Uso | Exemplo | Explicação |
| --- | --- | --- | --- | --- |
| `+` | Soma | `+a` | `+(-1)` → `-1` | Operador unário, aplica a regra de sinais. |
| `-` | Subtração | `-a` | `-(-1)` → `1` | Operador unário, aplica a regra de sinais. |
| `+` | Soma | `a + b` | `1 + 3` → `4` | Soma o operando da esquerda `a` com o operando da direita `b`. |
| `-` | Subtração | `a - b` | `3 - 1` → `2` | Subtrai do operando da esquerda `a` o operando da direita `b`. |
| `*` | Multiplicação | `a * b` | `3 * 2` → `6` | Multiplica o operando da esquerda `a` com o operando da direita `b`. |
| `**` | Exponênciação | `a ** b` | `3 ** 2` → `9` | Eleva a base `a` à potência `b`. |
| `/` | Divisão | `a / b` | `3 / 2` → `1.5` | Divide o operando da esquerda `a` pelo operando da direita `b`, sempre resulta em um `float`. Levanta o erro `ZeroDivisionError` caso `b` seja `0` ou `0.0`. |
| `//` | Divisão Inteira | `a // b` | `3 // 2`→ `1` | Divide o operando à esquerda `a` pelo operando à direita `b` e arredonda para baixo. Se ambos os operandos forem `int`, o resultado é `int`; caso contrário, é `float`. Levanta o erro `ZeroDivisionError` caso `b` seja `0` ou `0.0`. |
| `%` | Módulo da Divisão | `a % b` | `3 % 2` → `1` | Divide o operando à esquerda `a` pelo operando à direita `b` e retorna o resto da divisão. Levanta o erro `ZeroDivisionError` caso `b` seja `0` ou `0.0`. |


## Operadores de Atribuição

São usados para realizar atribuição de valor entre os operandos. Por exemplo, atribuir um valor a uma variável.

### Tabela Operadores de Atribuição
| Operador | Operação | Uso | Equivale | Explicação |
| --- | --- | --- | --- | --- |
| `=` | Atribuição | `a = b` | — | Atribui no operando à esquerda `a` o valor do operando à direita `b`. |
| `+=` | Soma com atribuição | `a += b` | `a = a + b` | Realiza a soma do operando à esquerda `a` com o operando à direita `b` e atribui o resultado ao operando da esquerda `a`. |
| `-=` | Subtração com atribuição | `a -= b` | `a = a - b` | Realiza a subtração do operando à direita `b` do operando à esquerda `a` e atribui o resultado ao operando da esquerda `a`. |
| `*=` | Multiplicação com atribuição | `a *= b` | `a = a * b` | Realiza a multiplicação do operando à esquerda `a` pelo operando à direita `b` e atribui o resultado ao operando da esquerda `a`. |
| `/=` | Divisão com atribuição | `a /= b` | `a = a / b` | Realiza a divisão do operando à esquerda `a` pelo operando à direita `b` e atribui o resultado (float) ao operando da esquerda `a`. |
| `//=` | Divisão inteira com atribuição | `a //= b` | `a = a // b` | Realiza a divisão inteira do operando à esquerda `a` pelo operando à direita `b` e atribui o resultado ao operando da esquerda `a`. |
| `%=` | Módulo com atribuição | `a %= b` | `a = a % b` | Calcula o resto da divisão do operando à esquerda `a` pelo operando à direita `b` e atribui o resultado ao operando da esquerda `a`. |
| `**=` | Exponenciação com atribuição | `a **= b` | `a = a ** b` | Eleva o operando à esquerda `a` à potência do operando à direita `b` e atribui o resultado ao operando da esquerda `a`. |

### Operador Walrus/Morsa

O operador `:=` (walrus/morsa) permite atribuir um valor a uma variável e usá-lo no mesmo momento. Ele resolve um problema clássico: evitar que você precise calcular algo duas vezes ou criar uma linha extra só para guardar um valor.

**Veja um exemplo:**

O código lê todas as linhas um arquivo chamado `arquivo.txt` e as imprime no console.

```py
with open('arquivo.txt', 'r') as arquivo:
    linha = arquivo.readline()
    
    while linha:
        print(linha)
        linha = arquivo.readline() # Repetido!
```

**Problema:** a leitura das linhas é feita em dois lugares diferentes no código.

O operador morsa simplifica isso ao atribuir o conteúdo da linha à variável ao mesmo tempo em que verifica se a linha está vazia, deixando o código mais legível.

```py
with open('arquivo.txt', 'r') as arquivo:
    while linha := arquivo.readline(): # Uma linha só!
        print(linha)
```

## Operadores Relacionais

Permitem verificar uma relação entre os dois operandos, funcionam com qualquer tipo de dado. O resultado de uma operação relacional é sempre do tipo `bool`: `True` (verdadeiro) ou `False` (falso).

### Tabela Operadores Relacionais

| Operador | Operação | Uso | Explicação |
| --- | --- | --- | --- |
| `==` | Igualdade | `a == b` | Verifica se o valor à esquerda `a` equivale ao valor à direita `b`. |
| `!=` | Diferença | `a != b` | Verifica se o valor à esquerda `a` é diferente do valor à direita `b`. |
| `>` | Maior | `a > b` | Verifica se o valor à esquerda `a` é maior que o valor da direita `b`. |
| `<` | Menor | `a < b` | Verifica se o valor à esquerda `a` é menor que o valor da direita `b`. |
| `>=` | Maior ou Igual | `a >= b` | Verifica se o valor à esquerda `a` é maior ou igual ao valor da direita `b`. |
| `<=` | Menor ou Igual | `a <= b` | Verifica se o valor à esquerda `a` é menor ou igual ao valor da direita `b`. |

## Operadores de Identidade

São operadores usados para verificar se duas variáveis referenciam o mesmo objeto na memória.

### Tabela Operadores de Identidade

| Operador | Uso | Explicação |
| --- | --- | --- |
| `is` | `a is b` | Verifica se o operando à esquerda `a` tem o mesmo endereço de memória que o operando à direita `b`. |
| `is not` | `a is not b` | Verifica se o operando à esquerda `a` não tem mesmo endereço de memória que o operando à direita `b`. |

**Veja um exemplo:**

Temos duas listas Python, as duas com os mesmos valores armazenados:

```py
lista1 = ['a', 'b', 'b']
lista2 = ['a', 'b', 'b']

print(lista1 == lista2) # Saída: True
print(lista1 is lista2) # Saída: False
```

**O que aconteceu?**

Quando verificamos as listas utilizando o operador relacional de igualdade `==`, o Python verifica os **valores** das listas; quando utilizamos os operadores de **identidade**, o Python verifica se o endereço de memória que as duas listas possuem são o mesmo.

Se quiser obter os endereços de memória, o Python possui uma função built-in `id()`:

```py
id_lista1 = id(lista1)
id_lista2 = id(lista2)
print(id_lista1) # Saída: 139712958820672
print(id_lista2) # Saída: 139712958823360
```

> ⚠️ **Atenção:** Os endereços de memória sempre vão mudar aleatoriamente para cada execução do programa, pois a memória aloca os valores aleatoriamente.

## Operadores de Associação

São operadores utilizados para verificar se uma sequência possui um valor.

### Tabela Operadores de Associação

| Operador | Uso | Exemplo | Explicação |
| --- | --- | --- | --- |
| `in` | `a in b` | `(1 in [3, 2, 1])` → `True` | Retorna `True` caso o valor seja encontrado na sequência. |
| `not in` | `a not in b` | `(1 not in [4, 3, 2])` → `True` | Retorna `False` caso o valor seja encontrado na sequência. |

## Operadores Lógicos

São usados para operações de portas lógicas. Os operadores lógicos trabalham operandos do tipo booleano (`bool`) ou valores que podem se comportar como tal, por exemplo os números inteiros `0` (falso) e `1` (verdadeiro). O resultado retornado de uma operação lógica é o valor da última porta acionada.

### Tabela Operadores Lógicos

| Operador | Operação | Uso | Explicação |
| --- | --- | --- | --- |
| `and` | AND lógico (E) | `a and b` | Retorna imediatamente o valor falso, sem executar o resto da operação, ou o último valor verdadeiro caso todos verdadeiros. |
| `or` | OR lógico (OU) | `a or b` | Retorna imediatamente o primeiro valor verdadeiro, sem executar o resto da operação, ou o último valor caso todos falsos. |
| `not` | NOT lógico (NÃO) | `not a` | Retorna um tipo `bool` que corresponde ao booleano inverso do operando. |

**Veja um exemplo:**

```py
resultado = True and True
print(resultado) # Saída: True

resultado = True and False
print(resultado) # Saída: False

resultado = 100 and 0 and 1 # True and False and True
print(resultado) # Saída: 0

resultado = 0 or not 0 or 100 # False or True or False
print(resultado) # Saída: True
```

## Operadores Bitwise (bit-a-bit)

São operadores utilizados para manipular as cadeias de bits de números inteiros (`int`). A operação percorre os valores bit-a-bit, realizando a operação designada, resultando em uma nova cadeia de bits. O resultado é sempre um número do tipo inteiro (`int`).

### Tabela Operadores Bitwise

| Operador | Operação | Uso | Exemplo | Explicação |
| --- | --- | --- | --- | --- |
| `&` | AND bitwise | `a & b` | `0b1011 & 0b1110` → `0b1010` | Desliga o bit caso ambos sejam diferentes (`0` e `1`), mantém ligado ou desligado caso ambos forem iguais (`1` e `1` ou `0` e `0`). |
| `|` | OR bitwise | `a | b` | `0b1001 | 0b1100` → `0b1101` | Liga o bit caso algum seja `1` e mantém desligado caso ambos sejam `0`. |
| `^` | XOR bitwise | `a ^ b` | `0b1001 ^ 0b0101` → `0b1100` | Liga o bit caso ambos sejam diferentes (`0` e `1`) e desliga caso sejam iguais. |
| `~` | NOT bitwise | `~a` | `~0b0101` → `-6` | Inverte todos os bits do número. Equivalente a `-(a+1)` para inteiros. |
| `<<` | Deslocamento à esquerda | `a << b` | `0b1 << 2` → `0b100` | Desloca `b` bits à esquerda do operando `a`, preenchendo com `0` à direita.* |
| `>>` | Deslocamento à direita | `a >> b` | `0b101 >> 2` → `0b1` | Desloca `b` bits à direita do operando `a`, preenchendo com `0` à esquerda se positivo ou `1` se negativo e descartando bits à direita. |

### Tabela Operadores Bitwise de Atribuição
| Operador | Operação | Uso | Equivale | Explicação |
| --- | --- | --- | --- | --- |
| `&=` | AND bit a bit com atribuição | `a &= b` | `a = a & b` | Realiza o AND bit a bit entre o operando à esquerda `a` e o operando à direita `b` e atribui o resultado ao operando da esquerda `a`. |
| `|=` | OR bit a bit com atribuição | `a |= b` | `a = a | b` | Realiza o OR bit a bit entre o operando à esquerda `a` e o operando à direita `b` e atribui o resultado ao operando da esquerda `a`. |
| `^=` | XOR bit a bit com atribuição | `a ^= b` | `a = a ^ b` | Realiza o XOR bit a bit entre o operando à esquerda `a` e o operando à direita `b` e atribui o resultado ao operando da esquerda `a`. |
| `>>=` | Deslocamento à direita com atribuição | `a >>= b` | `a = a >> b` | Desloca os bits do operando à esquerda `a` para a direita pelo número de posições indicado pelo operando à direita `b` e atribui o resultado ao operando da esquerda `a`. |
| `<<=` | Deslocamento à esquerda com atribuição | `a <<= b` | `a = a << b` | Desloca os bits do operando à esquerda `a` para a esquerda pelo número de posições indicado pelo operando à direita `b` e atribui o resultado ao operando da esquerda `a`. |

> ***** Devido à forma como o Python trabalha com números inteiros, os bits à esquerda não são descartados. Python usa inteiros de precisão arbitrária, então nunca há overflow. 

## Ordem de precedência

A ordem de precedência dos operadores define a ordem em que cada operação é realizada quando são misturadas mais de uma operação na mesma instrução. Compreender a ordem de precedência é importante para evitar erros na hora de definir operações.

#### Regras da ordem de precedência

Quando operadores têm a mesma prioridade, o Python resolve da esquerda para a direita.

#### Exceções à regra

  - `**` (exponenciação): resolve da direita para a esquerda
  - `if-else` (expressão condicional): resolve da direita para a esquerda

### Tabela Ordem de Precedência 

Itens no topo da tabela tem mais prioridade que os itens da base:

| Operador | Descrição |
| --- | --- |
| `()`, `{}`, `[]` | Binding ou expressão entre parênteses, display de lista, display de dicionário, display de conjunto |
| `x(args...)`, `x[index]`, `x[index:index]`, `x.attribute` | Chamadas de funções, referenciação de atributos, subscrição (e fatiamento) |
| `await x` | Expressões await |
| `**` | Exponênciações |
| `+x`, `-x`, `~x` | Operadores Unários |
| `*`, `/`, `//`, `%`, `@` | Multiplicações e Divisões |
| `+`, `-` | Somas e Subtrações |
| `<<`, `>>` | Deslocamentos Bitwise |
| `&` | Bitwise AND |
| `^` | Bitwise XOR |
| `|` | Bitwise OR |
| `in`, `not in`, `is`, `is not`, `<`, `<=`, `>`, `>=`, `!=`, `==` | Comparações, Operadores de Identidade, Operadores de Associação |
| `not x` | NOT Booleano |
| `and` | AND Booleano |
| `or` | OR Booleano |
| `if`-`else` | Expressões Condicionais
| `lambda` | Expressões lambda
| `:=` | Expressões de atribuição |

**Veja exemplos:**

```py
# Sem parênteses: divisão antes da soma
resultado = 10 + 20 / 2
print(resultado)  # Saída: 20.0 (20/2=10, depois 10+10)

# Com parênteses: soma antes da divisão
resultado = (10 + 20) / 2
print(resultado)  # Saída: 15.0 (10+20=30, depois 30/2)
```
