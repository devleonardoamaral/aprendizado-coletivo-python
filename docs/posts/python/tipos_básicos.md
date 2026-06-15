---
title: Tipos de Básicos
description: Introdução aos tipos de básicos do Python.
authors:
  - leonardo_amaral
date:
  created: 2026-04-04
  updated: 2026-06-15
---

# Tipos Básicos

Os tipos básicos são as formas principais que o Python usa para guardar e trabalhar com informações. Eles são importantes porque o Python precisa saber que tipo de dado está lidando para decidir o que fazer com ele.

<!-- more -->

Veja um exemplo prático:

- Se você somar **números**, o Python faz uma conta matemática.
- Se você somar **textos**, o Python apenas os junta, um atrás do outro.

Isso acontece porque o Python trata números e textos de formas diferentes — mesmo que o texto contenha um número.

No código abaixo, temos duas variáveis. Cada uma guarda um texto que contém apenas um número:

```py
var1 = '1'
var2 = '2'
result = var1 + var2
print(result) # Saída: '12'
```

**O que aconteceu?**

Como `'1'` e `'2'` estão entre aspas, o Python entende que são textos, não números. Por isso, o sinal de `+` não faz uma soma matemática — ele apenas junta os dois textos, resultando em `'12'`.

Se quiséssemos fazer a conta de verdade (1 + 2 = 3), precisaríamos usar números sem aspas:

```py
var1 = 1
var2 = 2
result = var1 + var2
print(result) # Saída: 3
```

Em Python, você não precisa dizer de antemão que tipo de dado uma variável vai guardar. O Python descobre sozinho, olhando apenas o valor que você colocou nela.

- Se você guarda um **texto entre aspas**, a variável vira do tipo texto (`str`).
- Se você guarda um **número inteiro**, vira do tipo número inteiro (`int`).
- Se você guarda um **número com casas decimais** (com ponto), vira do tipo decimal (`float`).
- Se você guarda um **valor de verdadeiro ou falso**, vira do tipo lógico (`bool`).

Veja no exemplo abaixo. Usamos a mesma variável `var` várias vezes, mas com valores diferentes. A cada vez, o Python muda o tipo dela automaticamente:

```py
var = "Texto de exemplo"
print(type(var)) # Saída: <class 'str'>

var = 12
print(type(var)) # Saída: <class 'int'>

var = 0.12
print(type(var)) # Saída: <class 'float'>

var = True
print(type(var)) # Saída: <class 'bool'>
```

Os principais e tipos básicos do Python são:

| Tipo    | Nome comum          | Para que serve                                            |
----------|---------------------|-----------------------------------------------------------|
| `int`   | Número inteiro      | Guarda números sem casas decimais, como `10`, `-5`, `0`.  |
| `float` | Número decimal      | Guarda números com casas decimais, como `3.14`, `-0.5`.   |
| `str`   | Texto               | Guarda textos, como `"Olá"`, `'Python'`.                  |
| `bool`  | Verdadeiro ou Falso | Guarda apenas `True` (verdadeiro) ou `False` (falso).     |
| `None`  | Vazio               | Representa ausência de valor.                             |

Como vimos antes, somar dois textos junta os textos e somar dois números faz a conta matemática. Por isso, às vezes é necessário [converter um tipo para outro](conversão_tipos_básicos.md) para que o Python realize a operação correta.
