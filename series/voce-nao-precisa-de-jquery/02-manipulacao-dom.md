# Manipulação do DOM

* **Artigo original**: [DOM Manipulation](http://blog.garstasio.com/you-dont-need-jquery/dom-manipulation/)
* **Tradução**: [Eric Cristhiano](https://github.com/ericcristhiano)

Anteriormente, [aprendemos como selecionar elementos sem depender do jQuery](01-seletores.md), mas o que aprendemos sobre mudar elementos? Sobre criar novos elementos? Que tal mudar os elementos para outro lugar na página? Você pode ficar feliz em saber que tudo isso, e mais, também é possível sem utilizar o jQuery. A web API nos fornece todas as ferramentas que precisamos, mais uma vez.
Você verá que algumas manipulações no DOM são triviais sem o auxílio do jQuery ou qualquer outra biblioteca. No entanto, outras podem ser um pouco mais complicadas. Este é um momento de destacar (novamente) que não estou tentando derrubar o jQuery, nem vou afirmar que é inútil ou completamente desnecessário. A intenção desses posts é para informa-lo como trabalhar com o navegador sem jQuery, se você preferir. Você pode concluir que, em muitos casos, uma biblioteca grande, como o jQuery, na maior parte não vai ser utilizada, e pode ser dispensada.

1. [Criando Elementos](#criando-elementos)
2. [Inserindo Elementos antes & depois](#inserindo-elementos-antes-e-depois)
3. [Inserindo elementos como filhos](#inserindo-elementos-como-filhos)
4. [Movendo elementos](#movendo-elementos)
5. [Removendo elementos](#removendo-elementos)
6. [Adicionando e removendo classes CSS](#adicionando-e-removendo-classes-css)
7. [Adicionando/removendo/alterando atributos](#adicionando-removendo-alterando-atributos)
8. [Adicionando e alterando conteúdo](#adicionando-e-alterando-conteudo)
9. [Adicionando/atualizando estilo dos elementos](#adicionando-atualizando-estilo-dos-elementos)
10. [Micro-bibliotecas para ajuda](#micro-bibliotecas-para-ajuda)
11. [Próximo](#proximo)

## <a name="criando-elementos"></a>Criando Elementos

*jQuery*

```
$('<div></div>');
```

*DOM API*

```
// IE 5.5+
document.createElement('div');
```

Wow, isso foi muito fácil. O jQuery nos poupou algumas teclas, mas isso dificilmente vale o tamanho da biblioteca.

## <a name="inserindo-elementos-antes-e-depois"></a>Inserindo elementos antes e depois

Vamos criar um elemento e inseri-lo após o outro elemento específico.
Então, vamos começar com:

```
<div id="1"></div>
<div id="2"></div>
<div id="3"></div>
```

E nós gostaríamos de criar um novo elemento com um ID '1.1' e inseri-lo entre as duas primeiras divs, dando-nos isto:

```
<div id="1"></div>
<div id="1.1"></div>
<div id="2"></div>
<div id="3"></div>
```

*jQuery*

```
$('#1').after('<div id="1.1"></div>');
```

*DOM API*

```
// IE 4+
document.getElementById('1')
    .insertAdjacentHTML('afterend', '<div id="1.1"></div>');
```

Há! Tome ISSO jQuery! Muito fácil em todos os navegadores apenas contanto com as ferramentas incorporadas no browser.

Ok, mas e se quisermos inserir um novo elemento ANTES da primeira div, dando-nos isso:

```
<div id="0.9"></div>
<div id="1"></div>
<div id="2"></div>
<div id="3"></div>
```

*jQuery*

```
$('#1').before('<div id="0.9"></div>');
```

*DOM API*

```
// IE 4+
document.getElementById('1')
    .insertAdjacentHTML('beforebegin', '<div id="0.9"></div>');
```

Praticamente igual ao último, com a exceção de uma chamada de método diferente para jQuery e um diferente parâmetro para a abordagem JavaScript puro.

## <a name="inserindo-elementos-como-filhos"></a>Inserindo elementos como filhos

Vamos dizer que temos isso:

```
<div id="parent">
    <div id="oldChild"></div>
</div>
```

...e queremos criar um novo elemento e torna-lo o primeiro filho de parent, como em:

```
<div id="parent">
    <div id="newChild"></div>
    <div id="oldChild"></div>
</div>
```

*jQuery*

```
$('#parent').prepend('<div id="newChild"></div>');
```

*DOM API*

```
// IE 4+
document.getElementById('parent')
    .insertAdjacentHTML('afterbegin', '<div id="newChild"></div>');
```

...ou criar um novo elemento e torna-lo o último filho de #parent

```
<div id="parent">
    <div id="oldChild"></div>
    <div id="newChild"></div>
</div>
```

*jQuery*

```
$('#parent').append('<div id="newChild"></div>');
```

*DOM API*

```
// IE 4+
document.getElementById('parent')
    .insertAdjacentHTML('beforeend', '<div id="newChild"></div>');
```

Tudo isso parece muito com a seção anterior que tratou com a inserção de novos elementos. Novamente, é muito simples fazer tudo isso, cross-browser, sem qualquer ajuda do jQuery (ou qualquer outra biblioteca).

## <a name="movendo-elementos"></a>Movendo elementos

Considere a seguinte marcação:

```
<div id="parent">
    <div id="c1"></div>
    <div id="c2"></div>
    <div id="c3"></div>
</div>
<div id="orphan"></div>
```

E se nós quisermos mudar o #orphan para o último filho do #parent? Isso nos daria isso:

```
<div id="parent">
    <div id="c1"></div>
    <div id="c2"></div>
    <div id="c3"></div>
    <div id="orphan"></div>
</div>
```

*jQuery*

```
$('#parent').append($('#orphan'));
```

*DOM API*

```
// IE 5.5+
document.getElementById('parent')
    .appendChild(document.getElementById('orphan'));
```

Bem simples, sem jQuery. Mas e se quiséssemos fazer #orphan o primeiro filho de #parent, dando-nos isso:

```
<div id="parent">
    <div id="orphan"></div>
    <div id="c1"></div>
    <div id="c2"></div>
    <div id="c3"></div>
</div>
```

*jQuery*

```
$('#parent').prepend($('#orphan'));
```

*DOM API*

```
// IE 5.5+
document.getElementById('parent')
    .insertBefore(document.getElementById('orphan'), document.getElementById('c1'));
```

Podemos ainda completar isso em uma linha, mas é um pouco menos intuitivo e verboso sem jQuery, ainda, não tão mau.

## <a name="removendo-elementos"></a>Removendo elementos

Como podemos remover um elemento do DOM? Vamos dizer que nós sabemos que um elemento com o ID “foobar” existe. Vamos destruí-lo.

*jQuery*

```
$('#foobar').remove();
```

*DOM API*

```
// IE 5.5+
document.getElementById('foobar').parentNode
    .removeChild(document.getElementById('foobar'));
```

A abordagem DOM API é certamente um pouco mais extensa e feia, mas funciona! Note que não temos que saber do elemento pai, o que é bom.

## <a name="adicionando-e-removendo-classes-css"></a>Adicionando & Removendo Classes CSS

Temos um simples elemento:

```
<div id="foo"></div>
```

...vamos adicionar uma classe CSS chamada “bold” neste elemento, dando-nos:
```
<div id="foo" class="bold"></div>
```

*jQuery*

```
$('#foo').addClass('bold');
```

*DOM API*

```
// IE 5.5+
// All modern browsers, with the exception of IE9
document.getElementById('foo').classList.add('bold');

// All browsers
document.getElementById('foo').className += 'bold';
```

Vamos remover essa classe agora:

*jQuery*

```
$('#foo').removeClass('bold');
```

*DOM API*

```
// All modern browsers, with the exception of IE9
document.getElementById('foo').classList.remove('bold');


// All browsers
document.getElementById('foo').className = 
    document.getElementById('foo').className.replace(/^bold$/, '');
```

Para o IE 10 e superior, é praticamente o mesmo entre jQuery e API Web.

## <a name="adicionando-removendo-alterando-atributos"></a>Adicionando/Removendo/Alterando Atributos

Vamos iniciar com um simples elemento, como este:

```
<div id="foo"></div>
```

Agora, vamos dizer quer ```<div>``` na verdade, funciona como um botão. Nós deveríamos definir o atributo ```role``` para tornar este elemento mais acessível.

*jQuery*

```
$('#foo').attr('role', 'button');
```

*DOM API*

```
// IE 5.5+
document.getElementById('foo').setAttribute('role', 'button');
```

Em ambos casos, um novo atribute pode ser criado, ou um atributo existente pode ser atualizado utilizando o mesmo código.
E se o comportamento de nossa ```<div>``` muda, e não tem mais a função de um botão. De fato, é apenas uma ```<div>``` agora com algum texto ou marcação. Vamos remover aquela ```role```...

*jQuery*

```
$('#foo').removeAttr('role');
```

*DOM API*

```
// IE 5.5+
document.getElementById('foo').removeAttribute('role');
```

## <a name="adicionando-e-alterando-conteudo"></a>Adicionando & Alterando conteúdo

Agora, temos a seguinte marcação:

```
<div id="foo">Hi there!</div>
```

...mas, nós queremos atualizar o texto para “Goodbye!”.

*jQuery*

```
$('#foo').text('Goodbye!');
```

Note que você também pode facilmente recuperar o texto atual do elemento chamando ```text``` sem nenhum parâmetro.

*DOM API*

```
// IE 5.5+
document.getElementById('foo').innerHTML = 'Goodbye!';

// IE 5.5+ but NOT Firefox
document.getElementById('foo').innerText = 'GoodBye!';

// IE 9+
document.getElementById('foo').textContent = 'Goodbye!';
```

Ambas propriedades acima retornarão HTML/texto atual.
A vantagem de usar o ```innerText``` ou ```textContent``` é que qualquer HTML é escapado, o que é uma grande característica seu o conteúdo é fornecido pelo usuário e você quer apenas incluir o texto como conteúdo para o elemento selecionado.

## <a name="adicionando-atualizando-estilo-dos-elementos"></a>Adicionando/Atualizando Estilo dos Elementos

Geralmente, adicionar códigos CSS “inline” ou com javascript é um ["código ruim"](https://en.wikipedia.org/wiki/Code_smell), mas pode ser necessário em casos únicos. Para esses casos, vou lhe mostrar como isso pode ser feito com jQuery e DOM API.

Dado um simples elemento com algum texto:

```
<span id="note">Attention!</span>
```

...gostaríamos de fazê-lo destacar-se um pouco mais, tornando-o negrito.

*jQuery*

```
$('#note').css('fontWeight', 'bold');
```

Note que você também pode facilmente recuperar o texto atual do elemento chamando “text” sem nenhum parâmetro.

*DOM API*

```
// IE 5.5+
document.getElementById('note').style.fontWeight = 'bold';
```

Na verdade, eu prefiro a abordagem DOM API nesse caso. Parece muito mais intuitivo que o método ```css``` do jQuery.

## <a name="micro-bibliotecas-para-ajuda"></a>Micro-bibliotecas para Ajuda

Então, quer saber? Você realmente não precisa de jQuery para manipulação de DOM cross-browser. Percebo que o jQuery pode fazer códigos mais complexos um pouco mais fáceis, mas se você está realmente interessado com tarefas mais complexas de manipulação de DOM, então considere utilizar em uma biblioteca menor que foca principalmente nisso. Algumas escolhas são o [jBone](https://github.com/kupriyanenko/jbone) e [dom.js](https://github.com/dkraczkowski/dom.js). Não há nada de errado em criar sua biblioteca também. Manipulação do DOM não é tão difícil como você pode imaginar.


## <a name="proximo"></a>Próximo Post

[Ajax Requests](http://blog.garstasio.com/you-dont-need-jquery/ajax/) (Em inglês)