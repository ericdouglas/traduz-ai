# Selecionando elementos sem jQuery

* **Artigo original**: [Selecting Elements](http://blog.garstasio.com/you-dont-need-jquery/selectors/)
* **Tradução**: [Gabriel](https://github.com/BielRibeiro)

How many times have you seen a web app or library that uses jQuery simply to perform trivial element selection? How many times have you written this: $(#meuElemento')? Or this: $('.meuElemento')? Psst... you don't need jQuery to select elements! It's pretty easy with the plain 'ole DOM API.

Quantas vezes você viu uma web app ou biblioteca que usa jQuery simplesmente para fazer seleções de elementos? Quantas vezes você escreveu isto: ```$(#meuElemento')```? Ou isto: ```$('.meuElemento')```? Psst... Você não precisa de jQuery para selecionar elementos! Isto já é muito fácil simplesmente usando a DOM API.

1. [IDs](#ids)
2. [Classes CSS](#classes-css)
3. [Nome da Tag](#tag-name)
4. [Atributos](#atributos)
5. [Pseudo-classes](#pseudo-classes)
6. [Filhos](#filhos)
7. [Descendentes](#descendentes)
8. [Seletores de Exclusão](#seletores-exclusao)
9. [Seletores Múltiplos](#seletores-multiplos)
10.[Ve um padrão?](#padrao)
11.[Filling in the Gaps](#)
12.[Next in this Series](#)

## <a name="ids"></a>Por ID

*jQuery*

```
// returns a jQuery obj w/ 0-1 elements
// retorna um objeto jQuery com 0-1 elementos
$('#meuElemento');
```

*DOM API*

```
// IE 5.5+
document.getElementById('meuElemento');
```

... ou ...

```
// IE 8+
document.querySelector('#meuElemento');
```

Both methods return a single Element. Note that getElementById is more efficient than using querySelector.

Ambos os métodos retornam um único elemento. Perceba que [getElementById é mais eficiente do que usar querySelector](http://jsperf.com/getelementbyid-vs-queryselector/11).

Does jQuery's syntax provide any advantage here? I don't see one. Do you?
Será que o jQuery proporciona alguma vantagem aqui? Eu não vejo nenhuma. Você vê?

## <a name="classes-css"></a>Por classe CSS

*jQuery*

```
// returns a jQuery obj w/ all matching elements
// retorna um objeto jQuery com todos elementos correspondentes
$('.meuElemento');
```

*DOM API*

```
// IE 9+
document.getElementsByClassName('meuElemento');
```

... ou ...

```
// IE 8+
document.querySelectorAll('.meuElemento');
```

The first method returns an HTMLCollection https://developer.mozilla.org/en-US/docs/Web/API/HTMLCollection and is the most efficient of the two choices http://jsperf.com/getelementsbyclassname-vs-queryselectorall/25. querySelectorAll always returns a NodeList https://developer.mozilla.org/en-US/docs/Web/API/NodeList.

O primeiro método retorna uma [HTMLCollection](https://developer.mozilla.org/pt-BR/docs/Web/API/HTMLCollection)  e é o [mais eficiente entre as duas escolhas   ](http://jsperf.com/getelementsbyclassname-vs-queryselectorall/25). O método querySelectorAll sempre retorna uma [NodeList](https://developer.mozilla.org/en-US/docs/Web/API/NodeList).

Again, really simple stuff here. Why bother with jQuery?
Novamente nada complicado. Por que se preocupar com jQuery?

## <a name="tag-name"></a>Por Nome da Tag

For example, to select all ```<div>``` elements on a page:
Para selecionar todas as ```<div>``` de uma página, por exemplo:

*jQuery*

```
$('div');
```

*DOM API*

```
// IE 5.5+
document.getElementsByTagName('div');
```

... ou ...

```
// IE 8+
document.querySelectorAll('div');
```

As expected, querySelectorAll (which returns a NodeList) is less efficient than getElementsByTagName (which returns an HTMLCollection).

Como esperado, querySelectorAll (que retorna uma NodeList) é menos eficiente que o getElementsByTagName (que retorna uma HTMLCollection).

## <a name="atributos"></a>Por Atributo

Select all elements with a "data-foo-bar" attribute that contains a value of "algumValor":

Seleciona todos os elementos com o atributo "data-foo-bar" que contém o valor de "algumValor":

*jQuery*

```
$('[data-foo-bar="algumValor"]');
```

*DOM API*

```
// IE 8+
document.querySelectorAll('[data-foo-bar="algumValor"]');
```

Again, the DOM API and jQuery syntax is very similar.
Novamente, as sintaxes da DOM API e do jQuery se mostram bastante similares.

## <a name="pseudo-classes"></a>Por Pseudo-classe

Select all fields in a specific form that are currently invalid. Let's assume our form has an ID of "meuForm".

Seleciona de um form em específico, todos os campos que estão atualmente inválidos. Vamos assumir que o nosso form tem o ID "meuForm".

*jQuery*

```
$('#meuForm :invalid');
```

*DOM API*

```
// IE 10+
document.querySelectorAll('#meuForm :invalid');
```

## <a name="filhos"></a>Filhos

Select all children of a specific element. Let's assume our specific has an ID of "divPai".

Seleciona todos os filhos de um elemento em específico, assumindo que ele possui o ID "divPai". 

*jQuery*

```
$('#divPai').children();
```

*DOM API*

```
// IE 5.5+
// NOTE: This will include comment and text nodes as well.
// Observação: Este método incluirá comentários e nós de texto também.
document.getElementById('divPai').childNodes;
```

... ou ...

```
// IE 9+ (ignores comment & text nodes).
// IE 9+ (ignora comentários e nós de texto).
document.getElementById('divPai').children;
```

But what if we want to only find specific children? Say, only children with an attribute of "ng-click"?

Mas e se quiséssemos algum filho específico? Por exemplo, somente filhos com o atributo "ng-click"?

*jQuery*

```
$('#divPai').children('[ng-click]');
```

... ou ...

```
$('#divPai > [ng-click]');
```

*DOM API*

```
// IE 8+
document.querySelector('#divPai > [ng-click]');
```

## <a name="descendentes"></a>Descendentes

Find all anchor elements under #divPai.
Seleciona todos os elementos ```<a>``` dentro da tag com ID #divPai.

*jQuery*

```
$('#divPai A');
```

*DOM API*

```
// IE 8+
document.querySelectorAll('#divPai A');
```

## <a name=""></a>Excluding Elements

Select all ```<div>``` elements, except those with a CSS class of "ignore".

*jQuery*

```
$('DIV').not('.ignore');
```

... ou ...

```
$('DIV:not(.ignore)');
```

*DOM API*

```
// IE 9+
document.querySelectorAll('DIV:not(.ignore)');
```

## <a name=""></a>Multiple Selectors

Select all ```<div>```, ```<a>``` and ```<script>``` elements.

*jQuery*

```
$(DIV, A, SCRIPT');
```

*DOM API*

```
// IE 8+
document.querySelectorAll('DIV, A, SCRIPT');
```

## <a name=""></a>See a Pattern?

If we focus exclusively on selector support, and don't need to handle anything older than IE8, seems like we could get pretty far simply by replacing jQuery with this:

```
window.$ = function(selector) {
    var selectorType = 'querySelectorAll';

    if (selector.indexOf('#') === 0) {
        selectorType = 'getElementById';
        selector = selector.substr(1, selector.length);
    }

    return document[selectorType](selector);
};
```

## <a name=""></a>But I Want More!

For the vast majority of projects, the selectors support baked into the Web API is sufficient. But what if you are unfortunate enough to require IE7 support? In that case, you'll probably need a little help from some third party code.

Sure, you could just pull in jQuery, but why use such a large codebase when you only need advanced selector support for now? Instead, try a micro-library that focuses exclusively on element selection. Consider Sizzle http://sizzlejs.com/, which happens to be the selector library that jQuery uses. Selectivizr http://selectivizr.com/ is another very small selector library that brings CSS3 selector support to very old browsers.


## <a name=""></a>Próximo Post

[DOM Manipulation](http://blog.garstasio.com/you-dont-need-jquery/dom-manipulation/) (Em inglês)