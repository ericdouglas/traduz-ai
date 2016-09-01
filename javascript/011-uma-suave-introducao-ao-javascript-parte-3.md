# Uma Suave Introdução ao JavaScript Funcional: Parte 3

* **Artigo Original**: [A GENTLE INTRODUCTION TO FUNCTIONAL JAVASCRIPT: PART 3](http://jrsinclair.com/articles/2016/gentle-introduction-to-functional-javascript-functions/)
* **Tradução**: [Eric Douglas](https://github.com/ericdouglas)

> *Escrito por James Sinclair em 6 de Fevereiro de 2016* 

Essa é a parte 3 de uma série de 4 artigos introduzindo a programação funcional no JavaScript. No último artigo vimos como podemos usar a programação funcional com listas e arrays. Neste artigo iremos examinar *high-order functions* (funções de ordem superior) - funções para fazerem funções.

- [Parte 1: Blocos fundamentais e motivação](009-uma-suave-introducao-ao-javascript-parte-1.md)
- [Parte 2: Trabalhando com Arrays e Listas](010-uma-suave-introducao-ao-javascript-parte-2.md)
- Parte 3: Funções para fazer funções (esse artigo)
- Parte 4: Fazendo isso com estilo

## Funções Para Fazerem Funções
No fim do último artigo, eu disse que ir a fundo no caminho funcional não é para todo mundo. Isso é porque além das funções que processam listas, as coisas começam a ficar um pouco estranhas. O que quero dizer é que começamos abstraindo coleções de instruções em funções. Então, começamos a abstrair loops `for` em `map` e `reduce`. O próximo nível de abstração é começar a refatorar padrões de *criação* de funções. Começamos a criar funções para fazer outras funções. Isso pode ser poderoso e elegante, mas começa a se parecer bem menos com o JavaScript que você está acostumado a escrever.

## more building blocks
