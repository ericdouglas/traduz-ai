# Uma Suave Introdução ao JavaScript Funcional: Parte 1

* **Artigo Original**: [A GENTLE INTRODUCTION TO FUNCTIONAL JAVASCRIPT: PART 1](http://jrsinclair.com/articles/2016/gentle-introduction-to-functional-javascript-intro/)
* **Tradução**: [Eric Douglas](https://github.com/ericdouglas)

> *Escrito por James Sinclair em 29 de Janeiro de 2016* 

Essa é a parte um de uma série de quatro artigos introduzindo a programação funcional no JavaScript. Nesse artigo, vamos ver os blocos fundamentais que fazem o JavaScript uma linguagem funcional, e vamos examinar como isso pode ser útil.

- Parte 1: Blocos fundamentais e motivação (esse artigo)
- Parte 2: Trabalhando com Arrays e Listas
- Parte 3: Funções para fazer funções
- Parte 4: Fazendo isso com estilo

## Por que Funcional?
O que é toda essa propaganda sobre JavaScript Funcional? E por que isso é chamado *funcional*? É como se alguém sentasse para escrever JavaScript *dis*funcional ou JavaScript não funcional. Para que isso é bom? Por que você deveria se preocupar?

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

Isso pode não ser muito excitante para você, mas cientistas da computação ficam bastante animados. This ability to put functions into
