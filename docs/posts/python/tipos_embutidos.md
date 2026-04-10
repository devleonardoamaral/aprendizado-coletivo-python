---
title: Tipos de Embutidos
description: Introdução aos tipos de embutidos do Python.
authors:
  - leonardo_amaral
date:
  created: 2026-02-03
  updated: 2026-02-09
---

# Tipos de Embutidos

> ⚠️ Este artigo ainda é um rascunho e pode estar faltando conteúdo a ser abordado.

## Introdução

Esse artigo tem o objetivo de introduzir os principais tipos embutidos do Python. Conhecer suas características e comportamentos permite um melhor aproveitamento dos recursos que a linguagem tem a oferecer e a previsibilidade sobre o comportamento do código evita bugs inesperados.

Diferente de outras linguagens de programação, Python não possui tipos primitivos da forma tradicional. Em Python, tudo é um objeto, incluindo os tipos mais básicos como `int`, `float`, `bool` e `str`. Por exemplo, o `int` em Python é um objeto completo com métodos (`int.to_bytes()`, `int.bit_length()`, etc), ao invés de valores brutos sem metadados. Esta característica dos tipos Python permite uma maior abstração ao lidar com os dados.

## Tipos Numéricos

Python fornece três tipos numéricos built-in — funcionalidades built-in são recursos que não precisam de importação para serem utilizadas na linguagem e possuem nomes reservados: inteiros (`int`), números de ponto flutuante (`float`) e números complexos (`complex`). É importante destacar que os valores booleanos em Python são um subtipo (herança) built-in dos números inteiros, sendo o `True` equivalente a `1` e `False` equivalente a `0`.

### Construtores

Os construtores `int()`, `float()` e `complex()` podem ser utilizados para criar números de um tipo específico. 

```py
num_inteiro = int(1)
num_flutuante = float(1)
num_complexo = complex(1, 2)
```

Também é possível produzir estes números através de uma sintaxe literal.

```py
num_inteiro = 1
num_flutuante = 1.0
num_flutuante = 1e-2
num_complexo = 1.0+2.0j
num_complexo = 1+2j
```

### Precisão dos Números Flutuantes

Números de ponto flutuante possuem precisão limitada de acordo com a arquitetura do sistema onde o Python está sendo executado. Essa precisão é equivalente ao double da linguagem C, na qual o interpretador padrão do Python, o CPython, é desenvolvido. 

Para curiosidade, Python utiliza a especificação **IEEE 754 double precision (64 bits)** para representar números de ponto flutuante. Essa especificação divide o número em três partes: **1 bit para o sinal**, que indica se o número é positivo ou negativo; **11 bits para o expoente**, que representa a magnitude do número; e **52 bits para a fração (também chamada de mantissa armazenada)**, que contém os dígitos significativos. Como todos os números normalizados em binário começam com 1., esse primeiro bit é implícito — não é armazenado, mas assumido pelo hardware durante os cálculos. Isso resulta em 53 bits de precisão efetiva, equivalente a aproximadamente 15-16 dígitos decimais significativos. No entanto, apenas os primeiros 15 dígitos são confiáveis, o 16º dígito pode estar incorreto devido a arredondamento, especialmente após operações aritméticas.

Essa especificação permite que diversas linguagens de programação possam representar os números de ponto flutuate de forma precisa. Mas com limitações, já que o hardware, a memória RAM, não consegue armazenar infinitas casas decimais ou números significativos infinitos de um número de ponsto flutuante. 

É possível obter a precisão suportada pelo seu sistema executando este código:

```py
import sys
print(sys.float_info)
```

A saída será algo como:

```py
sys.float_info(max=1.7976931348623157e+308, max_exp=1024, max_10_exp=308, min=2.2250738585072014e-308, min_exp=-1021, min_10_exp=-307, dig=15, mant_dig=53, epsilon=2.220446049250313e-16, radix=2, rounds=1)
```

O limite de precisão confiável de dígitos significativos do seu sistema será o valor exibido em `dig=15`. No caso do sistema qual foi executado o exemplo, que é um sistema 64bits, o máximo de números significativos de um número de ponto flutuante será de 15 dígitos. A contagem de significativos inclui todos os dígitos entre o primeiro e último não-zero, incluindo zeros intermediários. 

Exemplos de números significativos:

| Número         | Significativos | Quantidade          |
| -------------- | -------------- | ------------------- |
| 1230,1230      | 1230123        | 7 significativos    |
| 0,0012301230   | 1230123        | 7 significativos    |
| 1230123,00     | 1230123        | 7 significativos    |

### Problemas ao lidar com Números de Ponto Flutuante

Como vimos anteriormente, os números de ponto flutuante têm **precisão limitada**; e por conta disso, podem ter algumas características que podem ser problemáticas em operações aritiméticas e expressões condicionais. O exemplo abaixo ilustra bem uma situação comum em um código feito por um desenvolvedor que não conhece muito bem os números de ponto flutuante:

```py
numero1 = 0.1 + 0.1 + 0.1
numero2 = 0.3

if numero1 == numero2:
    print("Os números são iguais.")
else:
    print("Os números não são iguais.")
```

Esse código cairá no resultado do `else`, pois o resultado da expressão aritimética da variável `numero1` não resulta em `0.3`, mas sim em `0.30000000000000004`. E quando comparamos se esse número é igual a `0.3`, resultará em `False`. Para lidar com esse tipo de problema com números de ponto flutuante o Python nos fornece a função `math.isclose()`. Essa função infere automaticamente se a diferença entre os dois números de ponto flutuante é irrelevante. O mesmo exemplo, mas agora corrigindo o erro anterior, ficaria assim:

```py
import math

numero1 = 0.1 + 0.1 + 0.1
numero2 = 0.3

if math.isclose(numero1, numero2):
    print("Os números são iguais.")
else:
    print("Os números não são iguais.")
```

Um outro erro comum ao trabalhar com números flutuantes é exceder o limite de precisão durante uma operação aritimética, perdendo parte do dado no processo:

```py
limite = 2**53 # Equivale a: 9007199254740992.0
print(f"{limite + 1.1:f}") # Saída: 9007199254740994.000000
```

Isso ocorre porque a mantissa de 53 bits equivale a aproximadamente 15.95 dígitos decimais. O 16º dígito pode estar correto em alguns casos, mas não é confiável. Neste exemplo, quando foram somados 1.1 ao limite, resultou em um número de 17 números significativos, que não pôde ser representado corretamente na memória, tendo seu valor corrompido até mesmo no 16º dígito.  

## Referências Bibliográficas

- PYTHON SOFTWARE FOUNDATION. Tipos embutidos. In: Python Documentation. Python Software Foundation, 2026. Disponível em: [https://docs.python.org/pt-br/3.14/library/stdtypes.html](https://docs.python.org/pt-br/3.14/library/stdtypes.html). Acesso em: 9 abr. 2026.
- PYTHON SOFTWARE FOUNDATION. Aritmética de ponto flutuante: problemas e limitações. In: Python Documentation. Python Software Foundation, 2026. Disponível em: [https://docs.python.org/pt-br/3.14/tutorial/floatingpoint.html](https://docs.python.org/pt-br/3.14/tutorial/floatingpoint.html). Acesso em: 9 abr. 2026.
