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
Agora, nossa função `forEach` é útil, mas de é de alguma forma limitada. Se a função *callback* que passamos retornar um valor, `forEach` apenas ignora-o. Com um pequeno ajuste, podemos alterar nossa função `forEach` e ela vai nos dar qualquer valor que a função callback retornar. Vamos ter então um novo array com o valor correspondente a cada valor no array original.

Vamos ver um exemplo. Se temos um arrays com IDs, e queremos pegar o elemento correspondente a cada um deles. Em uma solução da forma procedural usaríamos um loop `for`:

```js
var ids = ['unicorn', 'fairy', 'kitten'];
var elements = [];
for (var i = 0; i < ids.length; i = i + 1) {
    elements[i] = document.getElementById(ids[i]);
}
// elements now contains the elements we are after
```

Novamente, tivemos que dizer ao computador como criar uma variável *index* e incrementá-la - detalhes que realmente não devemos nos preocupar. Vamos refatorar o loop `for` assim como fizemos com `forEach` e colocá-lo numa função chamada `map`:

```js
var map = function(callback, array) {
    var newArray = [];
    for (var i = 0; i < array.length; i = i + 1) {
        newArray[i] = callback(array[i], i);
    }
    return newArray;
}
```

A função `map` pega funções pequenas e trivias, tornando-as em funções super-herói - ela multiplica a efetividade da função aplicando-a em um array inteiro apenas com uma chamada.

Assim como `forEach`, `map` é tão útil que implementações modernas do JavaScript a tem como um método nativo para objetos array. Você pode chamar o método nativo da seguinte forma:

```js
var ids = ['unicorn', 'fairy', 'kitten'];
var getElement = function(id) {
  return document.getElementById(id);
};
var elements = ids.map(getElement);
```

Você pode ler mais sobre o método nativo [`map` na referência JavaScript da MDN.](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map)

### Reduce
