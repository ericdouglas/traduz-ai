# Um Mergulho no JavaScript Puro - Viljami S.

* **Artigo Original**: [A Dive Into Plain JavaScript](http://blog.adtile.me/2014/01/16/a-dive-into-plain-javascript/)
* **Tradução**: [Lucas F. Costa](https://github.com/lucasfcosta)

Ainda que eu tenha trabalhado por mais de uma década construindo os mais diversos websites, fazem apenas 3 anos que comecei a aprender mais sobre como trabalhar com JavaScript puro ao invés de sempre usar jQuery como ponto de partida. O fato de que atualmente estou aprendendo uma dúzia de coisas novas todo dia fez com que o trabalho no projeto Adtile's JavaScript SDK parecesse mais com a construção de um projeto de código aberto do que um "trabalho de verdade" e eu tenho que dizer: gostei muito disso.

Hoje eu vou compartilhar algumas das coisas básicas que aprendi durante os últimos anos, as quais espero que também te ajudem a mergulhar no mundo do JavaScript puro, tornando mais fácil a decisão de usar ou não jQuery no seu próximo projeto. 

## Melhora Progressiva

Enquanto libraries como [jQuery](http://jquery.com/) te ajudam a esquecer a maior parte das inconsistências entre navegadores, você vai ficar realmente familiarizado com eles assim que começar a usar JavaScript puro para tudo. Para evitar escrever um código cheio de "hacks" para o navegador e código que apenas resolve problemas de incompatibilidade entre navegadores, eu recomendo construir uma experiência melhorada progressivamente, usando uma detecção de funcionalidades para atingir somente os navegadores mais modernos. Isso não significa que navegadores como o IE7 não verão nada, significa apenas que eles obtêm uma experiência mais básica, sem melhoras no JavaScript.

## Aqui Está Como Vamos Fazer Isso

Nós temos um fragmento JavaScript separado chamado "feature.js" que contém todos os testes de funcionalidades. A verdadeira lista de testes é muito mais longa, nós vamos voltar a nos preocupar com isso um pouco mais tarde. Para cortar alguns dos navegadores mais antigos, nós usamos esses dois testes:

```js
var feature = {
	addEventListener : !!window.addEventListener,
	querySelectorAll : !!document.querySelectorAll,
};
```

Depois, na parte principal da aplicação, nós iremos detectar se essas funcionalidades são suportadas usando a simples declaração "`if`" abaixo. Caso elas não sejam suportadas o navegador não irá executar nada deste código:

```js
if (feature.addEventListener && feature.querySelectorAll) {
  this.init();
}
```

Esses dois testes confirmam que nós temos uma maneira nativa de usar seletores CSS em JavaScript (`querySelectorAll`), uma maneira de adicionar e remover elementos facilmente (`addEventListener`) e também confirma que os padrões suportados pelo navegador são melhores do que os presentes no IE8. Leia mais sobre essa técnica chamada "Cortando a mostarda" no [Blog da BBC](http://responsivenews.co.uk/post/18948466399/cutting-the-mustard).

##Suporte ao navegador

Aqui está uma lista "rústica" dos navegadores que suportam as funcionalides que estamos testando e consequentemente irão executar nosso JavaScript:

* IE9+
* Firefox 3.5+
* Opera 9+
* Safari 4+
* Chrome 1+
* iPhone e iPad iOS1+
* Telefones e Tablets Android 2.1+
* Blackberry OS6+
* Windows 7.5+
* Firefox Mobile
* Opera Mobile

##O Básico, a Maneira JavaScript Puro

Vamos começar a ver como as funcionalidades mais básicas e frequentemente usadas funcionam em JavaScript puro comparadas ao jQuery. Para cada exemplo irei prover ambas as maneiras: com jQuery e usando JavaScript puro.

###Document Ready

Com jQuery, muitos de vocês provavelmente estão acostumados a escrever o `document.ready` desta maneira:

```js
$(document).ready(function() {
  // Code
});
```

Mas você sabia que você pode simplesmente colocar o seu JavaScript no final da página e ele vai fazer basicamente a mesma coisa? JavaScript também possui um *event listener* para o evento de quando o conteúdo DOM é carregado, sendo assim você também pode usar o código abaixo em vez do `document.ready` do jQuery:

```js
document.addEventListener("DOMContentLoaded", function() {
  // Code
}, false);
```

##API de Seletores

A API de seletores nativos do JavaScript é muito boa. Ela funciona com seletores CSS e é muito similar à que o jQuery provê. Se você está acostumado à escrever isso em jQuery:

```js
var element = $("div");
```

Você agora pode substituir isso por:

```js
var element = document.querySelector("div");
```

Ou, para selecionar todos os `div`'s dentro de um determinado container:

```js
var elements = document.querySelectorAll(".container div");
```

Você também pode buscar por um elemento específico para encontrar seu respectivo filho:

```js
var navigation = document.querySelector("nav");
var links = navigation.querySelectorAll("a");
```

Muito objetivo, fácil de entender e não requer, necessariamente, muito mais escrita, não é mesmo? Para ir um pouco além nós podemos até construir uma pequena *library* em JavaScript para que possamos fazer buscas no DOM de maneira mais simplificada. Aqui está uma ideia que Andrew Lunny teve:

```js

// Isso nos dá uma simples função representada pelo símbolo de dólar e uma ligação com um evento na linha seguinte
var $ = document.querySelectorAll.bind(document);
Element.prototype.on = Element.prototype.addEventListener;

// Assim é como você usa isso
$(".element")[0].on("touchstart", handleTouch, false);
```

##Navegando pelo DOM

Navegar pelo DOM com JavaScript puro é um pouco mais difícil do que com jQuery. Mas não é difícil demais. Aqui estão alguns exemplos simples:

```js
// Obtendo o nó pai
var parent = document.querySelector("div").parentNode;

// Obtendo o nó de texto
var next = document.querySelector("div").nextSibling;

// Obtendo o nó anterior
var next = document.querySelector("div").previousSibling;

// Obtendo o primeiro elemento filho
var child = document.querySelector("div").children[0];

// Obtendo o último filho
var last = document.querySelector("div").lastElementChild;
```

##Adicionando e Removendo Classes

Com jQuery, adicionar, remover e checar se um elemento possui certas classes é muito simples. É um pouco mais complexo se usarmos JavaScript puro. Dar a um elemento uma classe chamada "foo" e substituir todas as classes atuais fica algo como:

```js
// Selecione um elemento
var element = document.querySelector(".alguma-classe");

// Dê a classe "foo" ao elemento
element.className = "foo";
```

Adicionando uma classe sem substituir as classes atuais:

```js
element.className += " foo";
```

Removendo a classe "no-js" de um elemento html e substituindo-a por "js":

```js
<html class="no-js">
<head>
  <script>
    document.documentElement.className = "js";
  </script>
……
```

Foi bem objetivo, não é mesmo? Vamos para o próximo passo, que é remover apenas certas classes. Isso é um pouco mais complexo. Eu estive usando essa pequena "função ajudante" em um fragmento separado chamado *util.js*. Ela leva 2 parâmetros: elemento e a classe que você quer remover:

```js
// removeClass, leva dois parâmetros: elemento(el) e nome da classe (cls)
function removeClass(el, cls) {
  var reg = new RegExp("(\\s|^)" + cls + "(\\s|$)");
  el.className = el.className.replace(reg, " ").replace(/(^\s*)|(\s*$)/g,"");
}
```

Depois, no fragmento principal da aplicação eu usei-a desta maneira:

```js
removeClass(element, "foo");
```

Se você também quiser checar um elemento quanto a sua pertinência a alguma classe, da mesma maneira que o `hasClass` do jQuery funciona, você pode adicionar isso no seu fragmento `utils`:

```js
// hasClass, aceita dois parâmentros: elemento(el) e nome da classe(cls)
function hasClass(el, cls) {
  return el.className && new RegExp("(\\s|^)" + cls + "(\\s|$)").test(el.className);
}
```

...e usar dessa maneira:

```js
// Checa se um elemento possui a classe "foo"
if (hasClass(element, "foo")) {

  // E mostra uma mensagem caso ele possua
  alert("O elemento tem essa classe!");
}
```

##Introduzindo a API classList do HTML5

Se você precisa apenas dar suporte aos navegadores modernos como IE10+, Chrome, Firefox, Opera e Safari, você pode começar usando a funcionalidade `classList` do HTML5, a qual faz com que adicionar ou retirar classes se torne muito mais fácil.

Isso é algo que eu acabei fazendo com a nossa última documentação de desenvolvedores. Como a funcionalidade que eu estava desenvolvendo era mais relacionada à uma melhora na interface de usuário e não era algo realmente essencial e que quebraria a experiência se não estivesse presente, decidi implementar.

Você pode detectar se o navegador suporta essa funcionalidade usando essa simples declaração "`if`":

```js
if ("classList" in document.documentElement) {
  // classList é suportado dentro desse bloco, agora faça algo com ele
}
```

Adicionando, removendo e alternando classes com classList:

```js
// Adicionando uma classe
element.classList.add("bar");

// Removendo uma classe
element.classList.remove("foo");

// Checando se contém uma classe
element.classList.contains("foo");

// Alternando uma classe
element.classList.toggle("active");
```

Um outro benefício de usar `classList` é que sua performance vai ser muito melhor do que usar a propriedade `className` pura. Se você tivesse um elemento como esse:

```js
<div id="test" class="one two three"></div>
```

O qual você quisesse manipular:

```js
var element = document.querySelector("#test");
addClass(element, "two");
removeClass(element, "four");
```

Para cada um desses métodos, a propriedade `className` seria lida do elemento e depois reescrita, fazendo um com que o navegador tenha que redesenhar a página. Ainda assim, esse não é o caso se quisermos usar os métodos relevantes da funcionalidade `classList`:

```var element = document.querySelector("#test");
element.classList.add("two");
element.classList.remove("four");
```

Com `classList` a propriedade base `className` é apenas alterada quando necessário. Dado que nós estamos adicionando uma classe que já é presente e removendo uma classe que não é, a propriedade `className` permanece intocada, isso significa que nós acabamos de evitar dois rdeesenhos!

##Detectores de Evento (*Event Listeners*)

Adicionar e remover detectores de evento de elementos em JavaScript puro é quase tão simples quanto em jQuery. As coisas ficam um pouco mais complexas quando você tem que adicionar múltiplos detectores de evento, mas eu vou explicar isso logo logo! O exemplo mais simples, o qual irá apenas fazer com que uma mensagem de alerta apareça quando um elemento é clicado, é o seguinte:

```js
element.addEventListener("click", function() {
  alert("Você clicou");
}, false);
```

Para conseguir essa mesma funcionalidade em todos os elementos de uma dada página nós vamos ter que fazer uma repetição através de todos os elementos, um por um, e dar a todos eles detectores de eventos:

```js
// Seleciona todos os links
var links = document.querySelectorAll("a");

// Para cada elemento link
[].forEach.call(links, function(el) {

  // Adiciona um detector de evento
  el.addEventListener("click", function(event) {
    event.preventDefault();
    alert("You clicked");
  }, false);

});
```

Uma das funcionalidades relacionadas a detectores de eventos mais legais do JavaScript é o fato de que "`addEventListener`" pode levar um objeto como segundo argumento, o qual vai automaticamente procurar por um método chamado "`handleEvent`" e chamá-lo. [Ryan Seddon](https://twitter.com/ryanseddon) já falou sobre essa técnica [neste artigo](http://www.thecssninja.com/javascript/handleevent), então eu vou apenas dar um exemplo rápido e depois você pode ler mais sobre isso no blog dele:

```js
var object = {
  init: function() {
    button.addEventListener("click", this, false);
    button.addEventListener("touchstart", this, false);
  },
  handleEvent: function(e) {
    switch(e.type) {
      case "click":
        this.action();
        break;
      case "touchstart":
        this.action();
        break;
    }
  },
  action: function() {
    alert("Clicado ou tocado!");
  }
};

// Init
object.init();
```

##Manipulando o DOM

Manipular o DOM com JavaScript puro pode soar como uma ideia horrível à primeira vista, mas não é muito mais complexo do que usar jQuery. Abaixo nós temos um exemplo que seleciona um elemento do DOM, cloná-o, manipula o estilo do clone com JavaScript e depois substitui o elemento original pelo elemento que foi manipulado.

```js
// Select an element
var element = document.querySelector(".class");

// Clone it
var clone = element.cloneNode(true);

// Do some manipulation off the DOM
clone.style.background = "#000";

// Replaces the original element with the new cloned one
element.parentNode.replaceChild(clone, element);
```

Se você não quer substituir nada no DOM, mas em vez disso apenas anexar o novo `div` dentro da *tag* `<body>`, você pode fazer algo como:

```js
document.body.appendChild(clone);
```

Se você quiser ler ainda mais sobre outros métodos relacionados ao DOM, eu sugiro que você se encaminhe para as [Tabelas Essenciais do DOM](http://quirksmode.org/dom/core/) do Peter-Paul Koch.

##Mergulhando Ainda Mais Fundo

Eu irei compartilhar mais duas técnicas avançadas aqui, as quais eu descobri recentemente. Ambas são funcionalidades que precisamos enquanto estávamos construindo o [Addtile](http://www.adtile.me/), então talvez você possa considerá-las úteis também.

##Determinando a Largura Máxima de Imagens Responsivas em JS

Esse é, pessoalmente, um dos meus favoritos e é muito útil se você alguma vez já precisou manipular imagens fluídas com JavaScript. Como os navegadores costumam retornar tamanhos redimensionados de imagens por padrão, nós tivemos que desenvolver alguma outra solução. Por sorte, navegadores modernos agora possuem uma maneira de fazer isso:

```js
var maxWidth = img.naturalWidth;
```

Isso irá nos dar o valor do atributo `max-width: 100%` da imagem em pixels e é suportado no IE9, Chrome, Firefox, Safari e Opera. Nós também podemos levar isso mais além e adicionar suporte para navegadores antigos carregando a imagem em um objeto na memória:

```js
// Pega o atributo max-width:100%; da imagem em pixels
function getMaxWidth(img) {
  var maxWidth;

  // Checa se naturalWidth é suportado
  if (img.naturalWidth !== undefined) {
    maxWidth = img.naturalWidth;

  // Não suportado, use a solução de carregar na memória como plano B
  } else {
    var image = new Image();
    image.src = img.src;
    maxWidth = image.width;
  }

  // Retorne o max-width
  return maxWidth;
}
```

Você deve notar que as imagens devem ser totalmente carregadas antes de checar a largura. Isso é o que nós estamos usando para ter certeza que as imagens possuem dimensões:

```js
function hasDimensions(img) {
  return !!((img.complete && typeof img.naturalWidth !== "undefined") || img.width);
}
```

##Determinando se um Elemento Está no Campo de Visão (Viewport)

Você pode obter a posição de qualquer elemento na página usando o método [getBoundingClientRect](https://developer.mozilla.org/en-US/docs/Web/API/Element.getBoundingClientRect). Abaixo está uma simples função mostrando o quão simples e poderoso isso pode ser. Essa função leva um parâmetro, que é o elemento o qual você quer checar. Ela irá retornar `true` quando o elemento estiver visível:

```js
// Determina se um elemento está no campo de visão
function isInViewport(element) {
  var rect = element.getBoundingClientRect();
  var html = document.documentElement;
  return (
    rect.top >= 0 &&
    rect.left >= 0 &&
    rect.bottom <= (window.innerHeight || html.clientHeight) &&
    rect.right <= (window.innerWidth || html.clientWidth)
  );
}
```

A função acima pode ser usada adicionando um detector de evento "`scroll`" (rolagem) à janela e depois chamando `isInViewport()`

##Conclusão

Se você deve ou não usar jQuery no seu próximo projeto depende muito do que você estiver construindo. Se é algo que requer grandes quantidades de código *front-end*, você provavelmente deve usá-lo. No entanto, se você estiver construindo um *plugin* ou uma *library* JavaScript, você pode querer considerar a possibilidade de usar apenas JavaScript puro. Usar JavaScript puro significa menos requisições e menos dados para carregar. Também significa que você não está forçando outros desenvolvedores a adicionarem jQuery ao projeto deles só por causa de sua dependência.