# Uma Suave Introdução ao JavaScript Funcional: Parte 1

* **Artigo Original**: [A GENTLE INTRODUCTION TO FUNCTIONAL JAVASCRIPT: PART 1](http://jrsinclair.com/articles/2016/gentle-introduction-to-functional-javascript-intro/)
* **Tradução**: [Eric Douglas](https://github.com/ericdouglas)

> *Escrito por James Sinclair em 29 de Janeiro de 2016* 

Essa é a parte um de uma série de quatro artigos introduzindo a programação funcional no JavaScript. Nesse artigo, vamos ver os blocos fundamentais que fazem o JavaScript uma linguagem funcional, e vamos examinar como isso pode ser útil.

- Parte 1: Blocos fundamentais e motivação (esse artigo)
- [Parte 2: Trabalhando com Arrays e Listas](010-uma-suave-introducao-ao-javascript-parte-2.md)
- [Parte 3: Funções para fazer funções](011-uma-suave-introducao-ao-javascript-parte-3.md)
- Parte 4: Fazendo isso com estilo

## Por que Funcional?
O que é toda essa propaganda sobre JavaScript Funcional? E por que isso é chamado *funcional*? Não é como se houvesse alguém que parasse para escrever JavaScript *dis*funcional ou JavaScript que não funciona. Para que isso é bom? Por que você deveria se preocupar?

Para mim, aprender programação funcional é um pouco como [obter um Thermomix](https://www.youtube.com/watch?v=4yr_etbfZtQ&feature=youtu.be):

- É um preciso um investimento inicial;
- Você vai começar a dizer para seus amigos e família o quão incrível isso é;
- Eles começam a se perguntar se você se juntou a algum tipo de culto.

Mas isso realmente faz algumas tarefas de uma forma muito mais simples. Isso pode até automatizar certas coisas que de outra forma seriam muito tediosas e gastariam um bom tempo para serem feitas.

## Blocos Fundamentais
Vamos começar com algumas funcionalidades básicas do JavaScript que tornam a programação "funcional" possível, antes de irmos para porque isso é uma boa ideia. No JavaScript nós temos dois blocos fundamentais: *variáveis* e *funções*. Variáveis são como contêineres que podemos colocar coisas dentro. Você pode criar um assim:

```js
var myContainer = "Hey everybody! Come see how good I look!";
```

Isso cria um contêiner chamado `myContainer` e coloca uma *string* nele.

Agora a função, por outro lado, é uma forma de agrupar algumas instruções para que você possa usá-las novamente, ou apenas deixar as coisas organizadas, dessa forma você não vai ficar tentando pensar em tudo de uma vez. Pode-se criar uma função assim:

```js
function log(someVariable) {
    console.log(someVariable);
    return someVariable;
}
```

E você pode chamar uma função assim:

```js
log(myContainer);
// Hey everybody! Come see how good I look!
```

Mas, o fato é, se você já viu algum JavaScript anteriormente, então você vai saber que também podemos escrever e chamar nossa função dessa forma:

```js
var log = function(someVariable) {
    console.log(someVariable);
    return someVariable;
}

log(myContainer);
// Hey everybody! Come see how good I look!
```

Vamos olhar para isso cuidadosamente. Quando escrevemos a definição da função dessa forma, parece que criamos uma variável chamada `log` e grudamos uma função nela. E isso é exatamente o que fizemos. Nossa função `log` *é* uma variável; o que significa que podemos fazer as mesmas coisas que fazemos com outras variáveis.

Vamos testar isso. Poderíamos, talvez, passar uma função como um parâmetro para outra função?

```js
var classyMessage = function() {
    return "Stay classy San Diego!";
}

log(classyMessage);
// [Function]
```

Bem, não muito útil. Vamos testar de uma forma diferente.

```js
var doSomething = function(thing) {
    thing();
}

var sayBigDeal = function() {
    var message = "I’m kind of a big deal";
    log(message);
}

doSomething(sayBigDeal);
// I’m kind of a big deal
```

Isso pode não ser muito excitante para você, mas cientistas da computação ficam bastante animados. Essa habilidade de colocar funções dentro de variáveis é algumas vezes descrita dizendo que "funções são objetos de primeira classe no JavaScript". O que isso quer dizer é que funções (praticamente) não são tratadas de forma diferente de outros tipos de dados como objetos e *strings*. E essa pequena funcionalidade é surpreendentemente poderosa. Para entender porque, porém, precisamos de falar sobre código *DRY*.

## *Don't Repeat Yourself* (Não se repita) 
Programadores gostam de falar sobre o princípio DRY - Don't Repeat Yourself. A ideia é que se você precisa fazer a mesma série de tarefas várias vezes, junte-as em algum tipo de pacote reutilizável (como uma função). Dessa forma, se você precisar alterar algo nessa série de tarefas, você consegue fazer isso em apenas um lugar.

Vamos ver um exemplo. Digamos que nós queremos colocar 3 carrosséis em uma página usando uma biblioteca de carrossel (uma biblioteca imaginária para exemplificar):

```js
var el1 = document.getElementById('main-carousel');
var slider1 = new Carousel(el1, 3000);
slider1.init();

var el2 = document.getElementById('news-carousel');
var slider2 = new Carousel(el2, 5000);
slider2.init();

var el3 = document.getElementById('events-carousel');
var slider3 = new Carousel(el3, 7000);
slider3.init();
```

Esse código é um tanto repetitivo. Desejamos inicializar um carrossel para os elementos da página, cada um com um ID específico. Então, vamos descrever como inicializar um carrossel em uma função, e então chamar essa função para cada ID.

```js
function initialiseCarousel(id, frequency) {
    var el = document.getElementById(id);
    var slider = new Carousel(el, frequency);
    slider.init();
    return slider;
}

initialiseCarousel('main-carousel', 3000);
initialiseCarousel('news-carousel', 5000);
initialiseCarousel('events-carousel', 7000);
```

Esse código é mais claro e simples de manter. E temos um padrão a seguir: quando tivermos o mesmo conjunto de ações que queremos tomar em diferentes conjuntos de dados, podemos encapsular essas ações em uma função. Mas e se tivermos um padrão onde as ações alteram?

```js
var unicornEl = document.getElementById('unicorn');
unicornEl.className += ' magic';
spin(unicornEl);

var fairyEl = document.getElementById('fairy');
fairyEl.className += ' magic';
sparkle(fairyEl);

var kittenEl = document.getElementById('kitten');
kittenEl.className += ' magic';
rainbowTrail(kittenEl);
```

Esse código é um pouco mais complicado de se refatorar. Existe definitivamente um padrão repetido, mas estamos chamando uma função diferente para cada elemento. Podemos resolver parte do problema encapsulando a chamada `document.getElementById()` e adicionando a `className` dentro de uma função. Isso vai salvar um pouco de repetição:

```js
function addMagicClass(id) {
    var element = document.getElementById(id);
    element.className += ' magic';
    return element;
}

var unicornEl = addMagicClass('unicorn');
spin(unicornEl);

var fairyEl = addMagicClass('fairy');
sparkle(fairyEl);

var kittenEl = addMagicClass('kitten');
rainbow(kittenEl);
```

Mas podemos fazer isso ainda mais DRY, se lembrarmos que o JavaScript permite que passemos funções como parâmetros para outras funções:

```js
function addMagic(id, effect) {
    var element = document.getElementById(id);
    element.className += ' magic';
    effect(element);
}

addMagic('unicorn', spin);
addMagic('fairy', sparkle);
addMagic('kitten', rainbow);
```

Isso é muito mais conciso e fácil de manter. A habilidade de passar funções como variáveis abre muitas possibilidades. Na próxima parte vamos olhar como usar essa habilidade para trabalhar com *arrays* de uma forma mais agradável.
