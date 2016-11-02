# Behaviour Driven Development com JavaScript

* **Artigo original**: [Behaviour Driven Development with JavaScript](http://gajus.com/blog/1/behaviour-driven-development-with-javascript)
* **Tradução**: [Eric Douglas](https://github.com/ericdouglas)

Este artigo é resultado de uma extensa pesquisa sobre BDD no JavaScript. Eu extraí o núcleo principal e a terminologia, e forneci exemplos práticos para ilustrar os benefícios do BDD.

## O que é BDD?
No capítulo 8 do livro *"Behaviour Driven Development with JavaScript"* por Marco Emrich, o autor cita várias definições de BDD. Eu destaco as seguintes:

> [BDD] amplia o TDD escrevendo casos de teste em uma linguagem natural que não programadores podem ler.
>
> **- M. Manca**

> BDD é uma metodologia ágil da segunda geração, de fora para dentro (*outside-in*), que envolve múltiplas partes interessadas, de várias escalas e com alta automatização. O BDD descreve um ciclo de interações com saídas bem definidas, resultando na entrega de software que funciona, software testado e software que realmente importa.
>
> **- Dan North**

Desenvolvedores puxam as funcionalidades dos *stakeholders* (partes interessadas) e as desenvolvem de fora para dentro. As funcionalidades são puxadas na orderm de seu valor de negócio. A funcionalidade mais importante é implementada primeiro e ela é implementada partindo das partes superficiais até suas partes mais específicas. Nenhum tempo é gasto construindo código desnecessário.

A evolução de um programador que está aprendendo TDD é um bom guia para entender o BDD:

1. A maioria dos desenvolvedores começam escrevendo testes unitários para código existe.
1. Depois de ter alguma prática eles aprendem sobre as vantagens de escrever o teste primeiro. Eles podem ter aprendido sobre boas práticas de testes como AAA, *[stubs](https://en.wikipedia.org/wiki/Test_stub)* (programas que simulam o comportamento de outros módulos) e *factories*.
1. Eles percebem que TDD pode ser usado como uma ferramenta de projeto para descobrir a API do código em produção.
1. Eventualmente eles percebem que TDD não é sobre testes. TDD é sobre definir comportamento e escrever especificações que servem como documentações vivas. Eles também descobrem *mocks* e desenvolvimento *outside-in*.

## Princípios do BDD
## *Baby steps* - passos de bebê
