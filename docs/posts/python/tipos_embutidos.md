---
title: Tipos de Embutidos
description: Introdução aos tipos de embutidos do Python.
authors:
  - leonardo_amaral
date:
  created: 2026-02-03
  updated: 2026-02-04
---

# Tipos de Embutidos

Esse artigo tem o objetivo de introduzir os principais tipos embutidos do Python. Conhecer os tipos Python e suas caracterísicas é importante para garantir a melhor abordagem na hora de arquitetar um projeto performático.

Conhecer as principais caracteríscas e comportamentos dos tipos embutidos dá previsbilidade sobre o comportamento do código e evita bugs inesperados. Por exemplo, dependendo da forma como se usa os números decimais, podem aparecer bugs característicos da imprecisão dos números decimais.

```py
numero1 = 0.1 + 0.1 + 0.1
numero2 = 0.3

# Verifica se os dois números têm o mesmo Valor
# depois salva o resultado na variável
resultado = numero1 == numero2

# Imprime no terminal o texto com o valor da variável
print(f"O resultado é", resultado) # Saída: O resultado é False
```

## Tipos Numéricos (int, float, complex, bool)

## Tipos Sequência (str, tuple, list, range, bytes)

## Tipos Conjunto (tuple, )

## Mapeamento

## Ausência de Valor (NoneType)

## Referência Bibliográficas

- PYTHON SOFTWARE FOUNDATION. Tipos embutidos. In: Python Documentation. Python Software Foundation, 2026. Disponível em: https://docs.python.org/pt-br/3.14/library/stdtypes.html. Acesso em: 3 abr. 2026.
