# Uma Suave Introdução ao JavaScript Funcional: Parte 2

* **Artigo Original**: [A GENTLE INTRODUCTION TO FUNCTIONAL JAVASCRIPT: PART 2](http://jrsinclair.com/articles/2016/gentle-introduction-to-functional-javascript-arrays/)
* **Tradução**: [Eric Douglas](https://github.com/ericdouglas)

> *Escrito por James Sinclair em 30 de Janeiro de 2016* 

Essa é a parte 2 de uma série de 4 artigos introduzindo a programação funcional no JavaScript. No artigo anterior, nós vimos como as funções podem ser usadas para fazer abstrações de código de forma mais fácil. Nesse artigo vamos aplicar essas técnicas em listas.

- [Parte 1: Blocos fundamentais e motivação](009-uma-suave-introducao-ao-javascript-parte-1.md)
- Parte 2: Trabalhando com Arrays e Listas (esse artigo)
- Parte 3: Funções para fazer funções
- Parte 4: Fazendo isso com estilo

## Trabalhando com Arrays e Listas
Relembre que no artigo anterior, falamos sobre código DRY. Vimos que funções são úteis para juntar grupos de ações que podem se repetir. Mas e se estivermos repetindo a mesma função várias vezes? Por exemplo:

```js
function addColour(colour) {
    var rainbowEl = document.getElementById('rainbow');
    var div = document.createElement('div');
    div.style.paddingTop = '10px';
    div.style.backgroundColour = colour;
    rainbowEl.appendChild(div);
}

addColour('red');
addColour('orange');
addColour('yellow');
addColour('green');
addColour('blue');
addColour('purple');
```

A função `addColor` é chamada várias vezes. Estamos nos repetindo - algo que queremos evitar. Uma forma de evitar isso é movendo a lista de cores para um array, e chamar `addColor` em um loop `for`:

```js
var colours = [
    'red', 'orange', 'yellow',
    'green', 'blue', 'purple'
];

for (var i = 0; i < colours.length; i = i + 1) {
    addColour(colours[i]);
}
```

Esse código é perfeitamente aceitável. Ele finaliza o trabalho e é menos repetitivo que a versão anterior. Mas não é particularmente expressivo. Temos que dar ao computador instruções muito específicas sobre criar uma variável de indicação (*index*) e incrementá-la, verificando se é hora de parar. E se pudéssemos encapsular todo esse loop `for` em uma função?

### For-Each (Para Cada)
Uma vez que o JavaScript nos permite passar uma função como parâmetro para outra função, escrever uma função `forEach` é relativamente simples:

```js
function forEach(callback, array) {
    for (var i = 0; i < array.length; i = i + 1) {
        callback(array[i], i);
    }
}
```

Essa função recebe outra função, `callback`, como um parâmetro e a chama em cada item do array.

Agora, com nosso exemplo, nós queremos rodar a função `addColor` em cada item no array. Usando nossa função `forEach` podemos expressar essa intenção em apenas uma linha:

```js
forEach(addColour, colours);
```

Chamar uma função em cada item de um array é uma ferramenta tão útil que implementações modernas do JavaScript incluem isso como uma função nativa nos arrays. Então ao invés de usarmos nossa própria função `forEach`, podemos usar a função nativa dessa forma:

```js
var colours = [
    'red', 'orange', 'yellow',
    'green', 'blue', 'purple'
];

colours.forEach(addColour);
```

Você pode achar mais informações sobre o método nativo [`forEach` na referência JavaScript da MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach)

### Map
