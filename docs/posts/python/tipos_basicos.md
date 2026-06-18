---
title: Tipos Básicos
description: Introdução aos tipos básicos do Python.
authors:
  - leonardo_amaral
date:
  created: 2026-04-04
  updated: 2026-06-18
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

#### Imprecisão do tipo `float`

Números decimais no Python são aproximados na memória. Isso acontece porque o computador guarda o float em base 2 (binário), e alguns números que são simples na base 10 (como 0.1) não têm representação exata nesse sistema.

**Veja um exemplo:**

```py
print(0.1 + 0.2)  # Saída: 0.30000000000000004
print(0.1 + 0.2 == 0.3)  # Saída: False
```

O resultado não é exatamente `0.3`, então a comparação direta com `==` falha. Isso não é um bug do Python; é uma limitação de como todos os computadores armazenam números decimais.

#### Manipulando `float` de forma segura: `math.isclose()`

Para comparar floats de forma segura, use a função `math.isclose()`. Ela verifica se dois números estão próximos o suficiente, dentro de uma margem de tolerância:

```py
import math

print(math.isclose(0.1 + 0.2, 0.3))  # Saída: True
```

Você também pode ajustar a tolerância quando precisar de mais ou menos rigor:

```py
import math

# tolerância absoluta de 0.01
print(math.isclose(1.00, 1.009, abs_tol=0.01))  # Saída: True

# tolerância relativa de 1%
print(math.isclose(100.0, 101.0, rel_tol=0.01))  # Saída: True
```

| Parâmetro | Significado | Quando usar |
| --- | --- | --- |
| `abs_tol` | Diferença máxima aceitável, valor fixo | Números pequenos, perto de zero |
| `rel_tol` | Diferença máxima aceitável, proporcional ao valor | Números grandes, comparações relativas |

> ⚠️ **Atenção:** Nunca compare floats com `==` a menos que você tenha certeza absoluta de que ambos foram calculados exatamente da mesma forma. Prefira sempre `math.isclose()` para verificar igualdade.

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

### Booleanos (`bool`)

Representa **sim/não**, **ligado/desligado**, **verdadeiro/falso**:

```py
esta_chovendo = True
tem_ingresso = False
print(type(tem_ingresso)) # Saída: <class 'bool'>
```

> ⚠️ **Atenção:** Python é exigente com maiúsculas. `True` e `False` sempre começam com maiúscula. `true` ou `false` dão erro.

#### Truthy e falsy

Booleanos não existem só quando você escreve `True` ou `False`. O Python considera todo valor como verdadeiro ou falso em contextos que exigem um bool; como dentro de uma expressão `if` ou `while`.

| Contexto | Significado |
| --- | --- |
| Truthy | O Python trata como `True` |
| Falsy | O Python trata como `False` |

Veja os valores que o Python considera falsy:

| Valor | Tipo | Por que é falsy |
| --- | --- | --- |
| `False` | `bool` | É o próprio falso |
| `None` | `NoneType` | Ausência de valor |
| `0` | `int` | Zero inteiro |
| `0.0` | `float` | Zero decimal |
| `''` | `str` | Texto vazio |
| `[]` | `list` | Lista vazia |
| `{}` | `dict` | Dicionário vazio |
| `()` | `tuple` | Tupla vazia |
| `set()` | `set` | Conjunto vazio |

**Todo o restante é truthy**. Até mesmo o número `-1` ou o texto `'False'` (como texto) são considerados `True`:

```py
# Exemplos de valores truthy
print(bool(42))        # Saída: True
print(bool(-1))        # Saída: True
print(bool("False"))   # Saída: True — é um texto, não o booleano!
print(bool(" "))       # Saída: True — espaço em branco ainda é texto
print(bool([0]))       # Saída: True — lista com um elemento dentro

# Exemplos de valores falsy
print(bool(0))         # Saída: False
print(bool(""))        # Saída: False
print(bool([]))        # Saída: False
```
