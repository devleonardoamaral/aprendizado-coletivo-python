---
title: Conversão de Tipos
description: Conversão de tipos em Python.
authors:
  - leonardo_amaral
date:
  created: 2026-06-15
  updated: 2026-06-16
---

## O que é conversão de tipos?

Às vezes você precisa transformar um dado de um tipo para outro. Por exemplo:

- O usuário digitou `'25'` (texto), mas você precisa fazer conta (número).
- Você quer montar uma mensagem com um número, mas o Python não junta texto e número diretamente.

Para isso, usamos os **construtores dos tipos**: `int()`, `float()`, `str()`, `bool()`.

<!-- more -->

## Texto (`str`)

Exemplos de conversão de valor para texto.

```py
var = str(1)
```

| Valor original | Tipo original            | Resultado   | Explicação            |
|----------------|--------------------------|-------------|-----------------------|
| `1`            | Número inteiro (`int`)   | `'1'`       | O número vira texto.  |
| `3.14`         | Número decimal (`float`) | `'3.14'`    | O número vira texto.  |
| `True`         | Verdadeiro (`bool`)      | `'True'`    | A palavra vira texto. |
| `None`         | Vazio (`NoneType`)       | `'None'`    |	A palavra vira texto. |

## Número inteiro (`int`)

Exemplos de conversão de valor para número inteiro.

```py
var = int(3.1415)
```

| Valor original  | Tipo original            | Resultado | Explicação                      |
|-----------------|--------------------------|-----------|---------------------------------|
| `3.1415`        | Número decimal (`float`) | `3`       | As casas decimais são perdidas. |
| `True`          | Verdadeiro (`bool`)      | `1`       | True converte em 1.             |
| `False`         | Falso (`bool`)           | `0`       | False converte em 0.            |

## Número decimal (`float`)

Exemplos de conversão de valor para número decimal.

```py
var = float(3)
```

| Valor original | Tipo original           | Resultado | Explicação                               |
|----------------|-------------------------|-----------|------------------------------------------|
| `3`            | Número inteiro (`int`)  | `3.0`     | Adiciona casas decimais.                 |
| `True`         | Verdadeiro (`bool`)     | `1.0`     | True converte em 1, com casas decimais.  |
| `False`        | Falso (`bool`)          | `0.0`     | False converte em 0, com casas decimais. |

## Verdadeiro ou falso (`bool`)

Exemplos de conversão de valor para verdadeiro ou falso.

```py
var = bool(-100)
```

| Valor original  | Tipo original            | Resultado | Por quê?                                                |
|-----------------|--------------------------|-----------|---------------------------------------------------------|
| `1`, `-100`     | Número inteiro (`int`)   | `True`    | Qualquer número diferente de zero é verdadeiro.         |
| `0`             | Número inteiro (`int`)   | `False`   | Zero é falso.                                           |
| `0.0`           | Número decimal (`float`) | `False`   | Zero decimal é falso.                                   |
| `'Exemplo'`     | Texto (`str`)            | `True`    | Texto com pelo menos 1 caractere é verdadeiro.          |
| `''`            | Texto vazio (`str`)      | `False`   | Texto vazio é falso.                                    |
| `None`          | Vazio (`NoneType`)       | `False`   | Ausência de valor é falso.                              |

## Conversões inválidas

### Texto não numérico para inteiro

Nem toda conversão é possível. Python avisa com um erro:

```py
# Erro: texto não-numérico para int
int("Olá")  # ValueError: invalid literal for int() with base 10: 'Olá'
```

Nesse caso é necessário verificar se o texto é um número antes de realizar a conversão. O tipo texto (`str`) do Python fornece um método nativo `isnumeric()` para fazer essa verificação:

```py
texto = "Olá"
# 'Olá' não é um texto numérico, então retorna False (falso)
print(texto.isnumeric()) # Saída: False

texto = "100"
# '100' é um texto numérico, então retorna True (verdadeiro)
print(texto.isnumeric()) # Saída: True

texto = "3.14"            # o ponto não é considerado numérico
print(texto.isnumeric())  # Saída: False
```

Para verificar se o texto é um número de ponto flutuante é preciso outra abordagem, com **try/except** por exemplo (veremos isso em outro artigo).

### Texto numérico com ponto e vírgula para ponto flutuante

Ponto flutuante em Python só aceita o caractere ponto (`.`) como separador de casas decimais. Utilizar vírgula (`,`) como separador resultará em erro:

```py
float("3,14")  # ValueError: could not convert string to float: '3,14'
```

Para resolver esse problema, é necessário substituir a vírgula pelo caractere de ponto:

```py
numero = "3,14".replace(',', '.') # vira "3.14"
numero = float(numero)
print(numero)       # Saída: 3.14
print(type(numero))  # Saída: <class 'float'>
```
