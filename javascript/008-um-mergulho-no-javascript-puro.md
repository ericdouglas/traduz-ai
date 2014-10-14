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

// This gives us simple dollar function and event binding
var $ = document.querySelectorAll.bind(document);
Element.prototype.on = Element.prototype.addEventListener;

// This is how you use it
$(".element")[0].on("touchstart", handleTouch, false);
```