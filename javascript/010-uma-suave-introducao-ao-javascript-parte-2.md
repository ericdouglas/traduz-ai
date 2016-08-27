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
`map` é muito útil, mas nós podemos fazer uma função ainda mais poderosa se pegarmos um array inteiro e retornar apenas um valor. Isso pode parecer inicialmente um pouco contraintuitivo - como uma função que retorna um valor ao invés de vários é mais *poderosa*? Para descobrir o porquê, temos que primeiro olhar como essa função trabalha.

Para ilustrar, vamos considerar dois problemas similares:

1. Dado um array de números, calcule a some; e
1. Dado um array de palavras, junte-as com um espaço entre cada palavra.

Isso pode parecer exemplos bobos, triviais - e eles são. Mas tenha paciência comigo, após vermos como a função `reduce` trabalha, nós vamos aplicá-la de formas mais interessantes.

A forma "procedural" de resolver esses problemas é, novamente, com loops `for`:

```js
// Given an array of numbers, calculate the sum
var numbers = [1, 3, 5, 7, 9];
var total = 0;
for (var i = 0; i < numbers.length; i = i + 1) {
    total = total + numbers[i];
}
// total is 25

// Given an array of words, join them together with a space between each word.
var words = ['sparkle', 'fairies', 'are', 'amazing'];
var sentence = '';
for (i = 0; i < words.length; i++) {
    sentence = sentence + ' ' + words[i];
}
// ' sparkle fairies are amazing'
```

Essas duas soluções tem muito em comum. Ambas usam um loop `for` para iterar sobre um array; ambas tem uma variável trabalhando ( `total` e `sentence`); e ambas definem o valor trabalhado com um valor inicial.

Vamos refatorar a parte interior de cada loop, e tornar isso em uma função:

```js
var add = function(a, b) {
    return a + b;
}

// Given an array of numbers, calculate the sum
var numbers = [1, 3, 5, 7, 9];
var total = 0;
for (i = 0; i < numbers.length; i = i + 1) {
    total = add(total, numbers[i]);
}
// total is 25

function joinWord(sentence, word) {
    return sentence + ' ' + word;
}

// Given an array of words, join them together with a space between each word.
var words = ['sparkle', 'fairies', 'are', 'amazing'];
var sentence = '';
for (i = 0; i < words.length; i++) {
    sentence = joinWord(sentence, words[i]);
}
// 'sparkle fairies are amazing'
```

Isso é muito mais conciso e o padrão ficou claro. Ambas funções interiores pegam a variável de trabalho como primeiro parâmetro e o elemento atual do array como segundo. Agora que conseguimos ver o padrão de forma mais clara, nós podemos mover os loops `for` para dentro da função:

```js
var reduce = function(callback, initialValue, array) {
    var working = initialValue;
    for (var i = 0; i < array.length; i = i + 1) {
        working = callback(working, array[i]);
    }
    return working;
};
```

Agora que temos nossa função `reduce`, vamos usá-la:

```js
var total = reduce(add, 0, numbers);
var sentence = reduce(joinWord, '', words);
```

Assim como `forEach` e `map`, `reduce` também é nativa no padrão JavaScript como um método do objeto Array. Podemos usá-la assim:

```js
var total = numbers.reduce(add, 0);
var sentence = words.reduce(joinWord, '');
```

Você pode ler mais sobre o método nativo [`reduce` na referência JavaScript do MDN.](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce)

## Juntando Tudo
Como mencionamos anteriormente, esses são exemplos triviais - as funções `add` e `joinWord` são muito simples - e esse é o ponto na verdade. Funções simples e pequenas são mais fáceis de pensar e testar. Mesmo quando pegamos duas funções pequenas e simples e as combinamos (como `add` e `reduce` por exemplo), o resultado ainda é mais fácil de se imaginar do que uma única função gigante e complicada. Mas, com isso dito, podemos fazer coisas mais interessantes do que adicionar números.

Vamos tentar fazer algo um pouco mais complicado. Começaremos com dados formatados de forma não convencional, e usar as funções `map` e `reduce` para transformar isso em uma lista HTML. Aqui estão nossos dados:

```js
var ponies = [
    [
        ['name', 'Fluttershy'],
        ['image', 'http://tinyurl.com/gpbnlf6'],
        ['description', 'Fluttershy is a female Pegasus pony and one of the main characters of My Little Pony Friendship is Magic.']
    ],
    [
        ['name', 'Applejack'],
        ['image', 'http://tinyurl.com/gkur8a6'],
        ['description', 'Applejack is a female Earth pony and one of the main characters of My Little Pony Friendship is Magic.']
    ],
    [
        ['name', 'Twilight Sparkle'],
        ['image', 'http://tinyurl.com/hj877vs'],
        ['description', 'Twilight Sparkle is the primary main character of My Little Pony Friendship is Magic.']
    ]
];
```


Os dados não estão muito arrumados. Seria muito mais claro se os arrays internos fossem objetos bem formatados. Previamente, nós usamos a função `reduce` para calcular simples valores como *strings* e números, mas ninguém disse que o valor retornado por `reduce` tem que ser simples. Nós podemos usar essa função com objetos, arrays ou até mesmo elementos DOM. Vamos criar uma função que recebe um desses arrays internos (como `['name', 'Fluttershy']`) e adiciona esse par chave/valor a um objeto.

```js
var addToObject = function(obj, arr) {
    obj[arr[0]] = arr[1];
    return obj;
};
```

Com essa função `addToObject` podemos converter cada "pequeno" array em um objeto:

```js
var ponyArrayToObject = function(ponyArray) {
    return reduce(addToObject, {}, ponyArray);
};
```

Se usarmos nossa função `map` poderemos converter todo o array em algo mais arrumado:

```js
var tidyPonies = map(ponyArrayToObject, ponies);
```

Agora temos um array de pequenos objetos. Com uma pequena ajuda do [pequeno *template engine* de Thomas Fuchs](http://mir.aculo.us/2011/03/09/little-helpers-a-tweet-sized-javascript-templating-engine/), podemos usar `reduce` novamente para converter isso em um fragmento HTML. A função de template pega uma string template e um objeto, e onde ela achar palavras envoltas com chaves (como `{name}` ou `{image}`), ela vai trocá-las com o valor correspondente no objeto. Por exemplo:

```js
var data = { name: "Fluttershy" };
t("Hello {name}!", data);
// "Hello Fluttershy!"

data = { who: "Fluttershy", time: Date.now() };
t("Hello {name}! It's {time} ms since epoch.", data);
// "Hello Fluttershy! It's 1454135887369 ms since epoch."
```

So, if we want to convert a pony object
