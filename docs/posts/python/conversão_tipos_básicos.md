---
title: Conversão de Tipos Básicos
description: Conversão de tipos em Python.
authors:
  - leonardo_amaral
date:
  created: 2026-06-15
  updated: 2026-06-15
---

# Conversão de Tipos Básicos

Para converter um tipo para outro, chamamos o construtor do tipo e passamos um valor conversível como argumento.

<!-- more -->

### Texto (`str`)

Exemplos de conversão de valor para texto.

```py
var = str(1)
```

| Valor original | Tipo original            | Resultado   | Explicação            |
|----------------|--------------------------|-------------|-----------------------|
| `1`            | Número inteiro (`int`)   | `'1'`       | O número vira texto.  |
| `3.14`         | Número decimal (`float`) | `'3.14'`    | O número vira texto.  |
| `True`         | Verdadeiro (`bool`)      | `'True'`    | A palavra vira texto. |
| `None`         | Vazio (`None`)           | `'None'`    |	A palavra vira texto. |

### Número inteiro (`int`)

Exemplos de conversão de valor para número inteiro.

```py
var = int(3.1415)
```

| Valor original  | Tipo original            | Resultado | Explicação                      |
|-----------------|--------------------------|-----------|---------------------------------|
| `3.1415`        | Número decimal (`float`) | `3`       | As casas decimais são perdidas. |
| `True`          | Verdadeiro (`bool`)      | `1`       | True converte em 1.             |
| `False`         | Falso (`bool`)           | `0`       | False converte em 0.            |

### Número decimal (`float`)

Exemplos de conversão de valor para número decimal.

```py
var = float(3)
```

| Valor original | Tipo original           | Resultado | Explicação                               |
|----------------|-------------------------|-----------|------------------------------------------|
| `3`            | Número inteiro (`int`)  | `3.0`     | Adiciona casas decimais.                 |
| `True`         | Verdadeiro (`bool`)     | `1.0`     | True converte em 1, com casas decimais.  |
| `False`        | Falso (`bool`)          | `0.0`     | False converte em 0, com casas decimais. |

### Verdadeiro ou falso (`bool`)

Exemplos de conversão de valor para verdadeiro ou falso.

```py
var = bool(-100)
```

| Valor original  | Tipo original            | Resultado | Explicação                                              |
|-----------------|--------------------------|-----------|---------------------------------------------------------|
| `0`             | Número inteiro (`int`)   | `False`   | Zero é falso.                                           |
| `1`             | Número inteiro (`int`)   | `True`    | Qualquer número diferente de zero é verdadeiro.         |
| `-100`          | Número inteiro (`int`)   | `True`    | Qualquer número diferente de zero é verdadeiro.         |
| `0.0`           | Número decimal (`float`) | `False`   | Zero decimal é falso.                                   |
| `None`          | Vazio (`None`)           | `False`   | Ausência de valor é falso.                              |
| `'Exemplo'`     | Texto (`str`)            | `True`    | Qualquer texto com pelo menos 1 caractere é verdadeiro. |
| `''`            | Texto vazio (`str`)      | `False`   | Ausência de valor é falso.                              |
