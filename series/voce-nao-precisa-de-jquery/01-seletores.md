# Selecionando elementos sem jQuery

* **Artigo original**: [Selecting Elements](http://blog.garstasio.com/you-dont-need-jquery/selectors/)
* **Tradução**: [Gabriel](https://github.com/BielRibeiro)

Quantas vezes você viu uma web app ou biblioteca que usa jQuery simplesmente para selecionar elementos? Quantas vezes você escreveu isto: ```$(#meuElemento')```? Ou isto: ```$('.meuElemento')```? Psst... Você não precisa de jQuery para selecionar elementos! Isto já é muito fácil simplesmente usando a DOM API.

1. [IDs](#ids)
2. [Classes CSS](#classes-css)
3. [Nome da Tag](#tag-name)
4. [Atributos](#atributos)
5. [Pseudo-classes](#pseudo-classes)
6. [Filhos](#filhos)
7. [Descendentes](#descendentes)
8. [Seletores de Exclusão](#seletores-exclusao)
9. [Seletores Múltiplos](#seletores-multiplos)
10. [Vê um padrão?](#padrao)
11. [Mas Eu Quero Mais](#quero-mais)
12. [Próximo](#proximo)

## <a name="ids"></a>Por ID

*jQuery*

```
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

Ambos os métodos retornam um único elemento. Perceba que [getElementById é mais eficiente do que usar querySelector](http://jsperf.com/getelementbyid-vs-queryselector/11).

Será que o jQuery proporciona alguma vantagem aqui? Eu não vejo nenhuma. Você vê?

## <a name="classes-css"></a>Por classe CSS

*jQuery*

```
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

O primeiro método retorna uma [HTMLCollection](https://developer.mozilla.org/pt-BR/docs/Web/API/HTMLCollection)  e é o [mais eficiente entre as duas escolhas](http://jsperf.com/getelementsbyclassname-vs-queryselectorall/25). O método querySelectorAll sempre retorna uma [NodeList](https://developer.mozilla.org/en-US/docs/Web/API/NodeList).

Novamente nada complicado. Por que se preocupar com jQuery?

## <a name="tag-name"></a>Por Nome da Tag

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

Como esperado, querySelectorAll (que retorna uma NodeList) é menos eficiente que o getElementsByTagName (que retorna uma HTMLCollection).

## <a name="atributos"></a>Por Atributo

Seleciona todos os elementos com o atributo "data-foo-bar" que contém o valor de "qualquerValor":

*jQuery*

```
$('[data-foo-bar="qualquerValor"]');
```

*DOM API*

```
// IE 8+
document.querySelectorAll('[data-foo-bar="qualquerValor"]');
```

Novamente, as sintaxes da DOM API e do jQuery se mostram bastante similares.

## <a name="pseudo-classes"></a>Por Pseudo-classe

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

Seleciona todos os filhos de um elemento em específico, assumindo que ele possui o ID "divPai". 

*jQuery*

```
$('#divPai').children();
```

*DOM API*

```
// IE 5.5+
// Observação: Este método incluirá comentários e nós de texto também.
document.getElementById('divPai').childNodes;
```

... ou ...

```
// IE 9+ (ignora comentários e nós de texto).
document.getElementById('divPai').children;
```

Mas e se quiséssemos selecionar somente alguns filhos em específico? Por exemplo, somente filhos com o atributo "ng-click"?

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

## <a name="seletores-exclusao"></a>Excluindo elementos da seleção

Seleciona todas ```<div>```'s, exceto aquelas com a classe "ignorar".

*jQuery*

```
$('DIV').not('.ignorar');
```

... ou ...

```
$('DIV:not(.ignorar)');
```

*DOM API*

```
// IE 9+
document.querySelectorAll('DIV:not(.ignorar)');
```

## <a name="seletores-multiplos"></a>Selecionando múltiplos elementos

Seleciona todos os elementos ```<div>```'s,  ```<a>```'s e ```<script>```'s.

*jQuery*

```
$(DIV, A, SCRIPT');
```

*DOM API*

```
// IE 8+
document.querySelectorAll('DIV, A, SCRIPT');
```

## <a name="padrao"></a>Consegue ver um padrão?

Se nosso objetivo for exclusivamente selecionar elementos, e não precisamos dar suporte a nada mais antigo que o IE8, podemos substituir o jQuery de forma bem simples com:

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

## <a name="quero-mais"></a>Mas Eu Quero Mais!

Para a maioria dos projetos, o suporte nativo da WEB API aos seletores é o suficiente. Mas e se vc for azarado o suficiente para ter que dar suporte ao IE7? Neste caso, você provavelmente precisará de uma pequena ajuda de uma dependência externa.

Claro, você poderia somente adicionar o jQuery ao projeto, Mas por quê usar uma biblioteca tão grande quando você, por enquanto, só precisa de suporte a seletores avançados?
Ao invés disso, tente usar uma micro-biblioteca que foca exclusivamente na seleção de elementos. Considere o [Sizzle](http://sizzlejs.com/), que por acaso é a biblioteca de seletores que o jQuery usa. [Selectivizr](http://selectivizr.com/) é outra biblioteca de seletores bastante pequena, que traz o suporte a seletores CSS3 a browsers antigos.

## <a name="proximo"></a>Próximo Post

[DOM Manipulation](http://blog.garstasio.com/you-dont-need-jquery/dom-manipulation/) (Em inglês)

[Selecionando elementos sem jQuery.](01-manipulacao-dom.md) (Em português)