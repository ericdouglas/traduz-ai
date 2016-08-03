# Introdução ao React para pessoas que sabem apenas o suficiente de jQuery para sobreviver
* **Artigo original**: [React.js Introduction For People Who Know Just Enough jQuery To Get By](http://reactfordesigners.com/labs/reactjs-introduction-for-people-who-know-just-enough-jquery-to-get-by/)
* **Tradução**: [Eric Douglas](https://github.com/ericdouglas)

![JavaScript para millennials - Eu ouvi que react era bom](https://pbs.twimg.com/media/CE0Jp9_WMAI6eZB.png)
> JavaScript para [Millennials](https://pt.wikipedia.org/wiki/Gera%C3%A7%C3%A3o_Y) - Eu ouvi que react era bom

Eu também ouvi que React era bom e recentemente passei algum tempo brincando com ele. Agora que estou bastante confortável com React, eu decidi escrever um tutorial sobre esse assunto.

## Público alvo: Pessoas que sabem apenas o suficiente sobre jQuery para sobreviver
Antes de começar, eu gostaria de esclarecer quem é meu público alvo.

Zed Shaw, o autor da série ["Aprenda a programar da forma difícil"](http://learncodethehardway.org/), recentemente escreveu um ótimo artigo chamado ["Programadores Iniciados vs Iniciantes"](https://zedshaw.com/2015/06/16/early-vs-beginning-coders/). Em seu artigo, Zeg critica educadores de programação que dizem que seus materiais são para "iniciantes", mas que na verdade são totalmente incompreensíveis para a maioria dos "totalmente" iniciantes.

Eu não quero errar da mesma forma aqui. Das pessoas que nunca testaram React, algumas estão confortáveis com frameworks JavaScript como Backbone, Ember ou Angular. Algumas sabem JavaScript muito bem. Algumas sabem apenas o suficiente de jQuery para sobreviver. Um tutorial que é efetivo para um grupo pode não ser otimizado para os outros grupos.

Nesse tutorial, eu estou visando o terceiro grupo mencionado: *pessoas que sabem o suficiente de jQuery para sobreviver*. Exemplos de pessoas que se encaixam nessa categoria:

- Designers que sabem fazer o básico de HTML/CSS/jQuery.
- Desenvolvedores WordPress que sabem como usar plugins jQuery.
- Desenvolvedores iniciantes que já completaram tutoriais básicos sobre HTML/CSS/JS.
- Desenvolvedores backend que dependem do Bootstrap e jQuery para criar seu frontend.
- Qualquer um que faz mais "copia e cola" do que arquitetar seu código quando se trata de JavaScript.

**Se você está confortável com JavaScript ou qualquer framework frontend como Backbone/Ember/Angular, esse tutorial NÃO é para você**, e você vai ficar muito frustrado com meu estilo de escrita. Existem diversos excelentes tutoriais para você aprender, incluindo o [tutorial oficial do React](http://facebook.github.io/react/docs/tutorial.html).

**Se você já sabe React**, você também vai ficar chateado comigo porque vou falar principalmente sobre *states* (estados) ao invés de imutabilidade ou componentização. De qualquer forma, eu acho que ensinar sobre estados primeiro é a melhor forma de desenvolvedores jQuery verem porque React é superior.

Vamos começar!

## Tempo Estimado: 1 ~ 2 horas
Se você ir realmente rápido (e copiar e colar os exemplos de código ao invés de digitar), este tutorial deve levar em torno de 1 hora. Se você for devagar, ele deve um pouco mais de 2 horas.

### Dúvidas

Se você tiver alguma dúvida, você pode:

- Comentar na caixa de comentários no fim [desta página](http://reactfordesigners.com/labs/reactjs-introduction-for-people-who-know-just-enough-jquery-to-get-by/).
- Mande-me um e-mail em shu@chibicode.com.
- Mande um tweet em @chibicode.
- Crie uma issue [nesse repositório](https://github.com/reactfordesigners/reactfordesigners.github.io/issues).

## Visão geral: Vamos Construir uma "Tweet Box"
Muitos tutorias de React começam explicando como o React funciona ou porque o React é incrível. Meu tutorial não.

Ao invés, vamos já começar criando uma simples interface de usuário (UI), alternando entre implementações jQuery e implementações React, explicando as diferenças durante o caminho. Acredito que você vai pensar mais dessa forma do que apenas digitando os exemplos.

A UI (interface de usuário) que vamos construir vai se parecer a caixa de tweet que você encontra no Twitter. Ela não vai ser exatamente como a caixa de tweet real, mas vai ser muito similar. Espero que você ache isso um exemplo prático.

## Passo 1: Introdução ao JSBin (5 - 10 minutos)
Vamos usar o [JSBin](http://jsbin.com/), um editor online de HTML/CSS/JS que suporta códigos jQuery e React. Você deve estar familiarizado com serviços similares como [CodePen](http://codepen.io/) e [JSFiddle](https://jsfiddle.net/) - eles são todos muito parecidos, então apenas decidi usar o JSBin.

Aqui você encontra um exemplo do JSBin: [link](http://jsbin.com/temaduv/1/edit).

**Tente modificar o HTML na parte esquerda** - mude o texto do botão. Você verá a mudança na direita. É assim que o JSBin funciona.

### Crie uma conta no JSBin
A menos que já tenha uma conta no JSBin, **vá até [o site](http://jsbin.com/)** e crie uma conta. Click em ***Login* ou *Register*** no menu para criar uma conta.

Depois de criar sua conta, você pode **clonar** *JSBins* para sua conta, da mesma forma que você clona repositórios GitHub públicos.

Vamos tentar. Abra [este link](http://jsbin.com/josemi/1/edit?html,output).

Você pode selecionar *"Add library"* no menu para importar bibliotecas CSS/JS.

![Adicionando bibliotecas no JSBin](http://reactfordesigners.com/images/labs/add-library.png)

Tente fazer o seguinte:

- Clicar em *"Add library"* e adicione a última versão do Bootstrap.
- Adicione as classes `btn btn-primary` em `<button>`.

A saída (aba *Output* no JSBin) deve estar um pouco mais bonita, [dessa forma](http://jsbin.com/cotehe/1/edit?html,output).

### Criando uma Caixa de Tweet
Você já deve estar bem confortável com o JSBin agora. Certo, vamos criar nossa caixa de tweet. Ainda no mesmo JSBin anterior, **mude o HTML dentro de `<body>` para isso**:

```
<div class="well clearfix">
  <textarea class="form-control"></textarea><br/>
  <button class="btn btn-primary pull-right">Tweet</button>
</div>
```

Nós vamos usar classes Bootstrap como `form-control`, `well`, `clearfix`, etc. Elas são apenas para a parte visual, mas são irrelevantes para o tutorial. [Aqui está o resultado](http://jsbin.com/zevezic/1/edit?html,output).

É isso para esse passo! Nada mal hein?!

## Passo 2: Implementando a Primeira Funcionalidade - O Botão "Tweet" Deve Inicialmente Estar Desativado (5 minutos)
Agora é hora de um pouco de JS. Nós vamos implementar primeiro a seguinte funcionalidade:

**Funcionalidade 1**: o botão "Tweet" deve estar inicialmente desativado. Se você digitar algo na caixa de texto, o botão fica ativado.

Para fazer isso funcionar, **prossiga a partir [deste JSBin](http://jsbin.com/wewimu/2/edit?html,js,output), abra a aba JavaScript e adicione o código jQuery a seguir**. Você não precisa de adicionar a biblioteca jQuery porque o Bootstrap, que adicionamos no passo anterior, inclue o jQuery.

```js
// Desabilita o botão inicialmente
$("button").prop("disabled", true);

// Quando o valor do textarea  muda...
$("textarea").on("input", function() {
  // Se tiver pelo menos um caracter...
  if ($(this).val().length > 0) {
    // Ativa o botão.
    $("button").prop("disabled", false);
  } else {
    // Senão, desabilita o botão.
    $("button").prop("disabled", true);
  }
});
```

### Explicação
- Eu usei nomes de tag, `button` e `textarea`, como seletores - não existe necessidade de adicionar IDs/classes para esse exemplo trivial.
- Para habilitar/desabilitar o botão, use `$(...).prop(disabled, ...)`.
- Para ouvir as mudanças no `textarea`, use o evento `input`, que funciona em navegadores modernos.

Teste digitar algo na caixa de tweet e veja o botão o estado do botão mudar (habilitado/desabilitado).

NÂO PROSSIGA se esse exemplo foi confuso para você - você precisa um pouco de jQuery antes de ir para o React. Existem vários materiais excelentes como [Codecademy](http://www.codecademy.com/en/tracks/jquery), [Treehouse](http://teamtreehouse.com/library/jquery-basics), [Code School](https://www.codeschool.com/courses/try-jquery), e outros.

Agora que essa funcionalidade está completa, vamos tentar reimplementar a mesma coisa usando React. Isso vai levar alguns passos.

## Passo 3: A Caixa de Tweet Usando React.js (5 - 10 minutos)
Uma das primeiras coisas que você vai notar no React é que você vai escrever marcação no JS, não no HTML.

Deixe-me mostrar o que quero dizer. Aqui está o código React que exibe a mesma caixa de tweet.

### ATENÇÃO! Você não precisa digitar ainda - apenas leia o código.

**HTML**
```html
<!DOCTYPE html>
<html>
<head>
<script src="https://code.jquery.com/jquery.min.js"></script>
<link href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.6/css/bootstrap.min.css" rel="stylesheet" type="text/css" />
<script src="https://fb.me/react-15.1.0.js"></script>
<script src="https://fb.me/react-dom-15.1.0.js"></script>
    <meta charset="utf-8">
  <title>JS Bin</title>
</head>
<body>
  <div id="container"></div>
</body>
</html>
```

**JSX** 
```js
var TweetBox = React.createClass({
  render: function() {
    return (
      <div className="well clearfix">
        <textarea className="form-control"></textarea>
        <br/>
        <button className="btn btn-primary pull-right">Tweet</button>
      </div>
    );
  }
});

ReactDOM.render(
  <TweetBox />,
  document.getElementById("container")
);
```

Algumas observações:

- Dentro de `return (...)` temos código HTML, não JavaScript. Em React, você vai escrever em uma sintaxe especial chamada JSX, que lhe permite colocar código do tipo HTML dentro do JavaScript.
- Eu digo código "*tipo*" HTML porque não é idêntico ao HTML. Note que ele usa `className` ao invés de `class` - mas ele é muito parecido, então você vai aprender rápido.
- Seu navegador não entende JSX, então quando o React processa seu código JSX, ele automaticamente converte a parte HTML dentro do JSX para código JavaScript válido de forma que o navegador possa entendê-lo.
- O código HTML dentro de `return(...)` é praticamente idêntico ao código HTML do passo 1.
- Veja que no código HTML acima praticamente não temos marcações além de `<body><div id="container"></div></body>`. Isso é o que eu quero dizer quando falo que **em React, você vai escrever marcação no JavaScript (JSX), não no HTML**.

### Perguntas Feitas Frequentemente & Respostas
- **Pergunta:** O que `React.createClass` e `ReactDOM.render` fazem? Preciso entendê-las agora?
- **Resposta:** Não se preocupe com isso agora. Basicamente, `React.createClass` cria um pedaço de UI (interface do usuário) com um nome (neste caso, `TweetBox`). Ele posteriormente vai ser anexado ao DOM pelo `ReactDOM.render(<TweetBox />, document.getElementById("container"))` - isso significa que essa UI vai ser adicionada dentro da tag `<div id="container">`. Isso é tudo que você precisa saber por agora.

- **Pergunta:** Preciso de fazer algo especial para escrever JSX na minha máquina local?
- **Resposta:** Sim, mas isso está fora do escopo deste tutorial - sucintamente, você precisa importar algo chamado *JSX transformer* ([veja aqui como](http://facebook.github.io/react/downloads.html)). Esse passo não é necessário no JSBin. Tudo que você precisa para escrever JSX no JSBin é (1) adicionar a biblioteca React (a sem *addons*) no menu dropdown e (2) selecionar "JSX (React)" no menu dropdown na aba JS.

- **Pergunta:** Não é uma má prática escrever marcação (HTML) e comportamente (JS) no mesmo lugar?
- **Resposta:** Isso pode ser uma má prática para páginas web simples, mas não necessariamente para grandes aplicações web. Em grandes aplicações web, teremos centenas de partes de UI, cada uma contendo sua própria marcação e comportamento. O código vai ser mais gerenciável se essas marcações e comportamentos permanecerem juntos para cada pedaço de UI, ao contrário de deixar "toda marcação" junta e "todo os comportamentos" juntos. E o React foi projetado para o desenvolvimento de aplicações web grandes. De fato, o React foi criado e é usado pelo Facebook, uma das maiores aplicações web de todos os tempos.

A seguir vou te ensinar como escrever o código React acima passo a passo.

## Passo 4: Escrevendo Seu Primeiro Código React (5 - 10 minutos)
Eu criei um HTML inicial para você. Usando "Add library" (adiciona biblioteca), eu importei o Bootstrap (e removi os arquivos `bootstrap.js` e jQuery) e React (sem *addons*).

**Por favor, tente seguir junto comigo. Para começar, click "Save" para copiar [esse JSBin](http://jsbin.com/notawe/11/edit?html,output) para sua conta.**

**HTML** 
```html
<!DOCTYPE html>
<html>
<head>
<script src="https://fb.me/react-15.1.0.js"></script>
<script src="https://fb.me/react-dom-15.1.0.js"></script>
  <link href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.5/css/bootstrap.min.css" rel="stylesheet" type="text/css" />
  <meta charset="utf-8">
  <title>JS Bin</title>
</head>
<body>
  <div id="container"></div>
</body>
</html>
```

Depois de salvar seu JSBin, **abra a aba JavaScript e selecione "JSX (React)":** 

![Abrindo a aba JavaScript - JSX React](http://reactfordesigners.com/images/labs/select-jsx.png)

Agora você está pronto para escrever React. **Tente me acompanhar e digite o seguinte código JavaScript** em seu JSBin.

**JSX**
```js
var TweetBox = React.createClass({
  render: function() {
  }
});
```

Esse é o template para se criar uma parte da UI usando React (nesse caso, uma caixa de Tweet). Isso é tão essencial quanto `$(function() { ... })` no jQuery.

Para realmente construir a UI, precisamos preencher o método `render()`. Por agora, vamos deixá-lo simples com apenas uma tag `div`.

**JSX** 
```js
var TweetBox = React.createClass({
  render: function() {
    return (
      <div>
        Hello World!
      </div>
    );
  }
});
```

Como no exemplo acima, **coloque um par de parênteses `(...)` depois de `return`, e escreva a marcação dentro.** 

### Armadilhas do JSX 
Existe uma coisa que você precisa se lembrar com JSX - no método `render()`, é necessário existir exatamente **uma** tag externa dentro de `return (...)`.

Dessa forma, o código a seguir não vai funcionar porque não existe nenhuma tag externa:

**JSX** 
```js
return (
  Hello World!
);
```

O próximo código também não funciona pois existem duas tags `span` externas dentro de `return (...)`:

**JSX** 
```js
return (
  <span>
    Hello
  </span>
  <span>
    World
  </span>
);
```

Para o exemplo acima, a solução alternativa é criar uma tag extra que envolve as duas tags `span`. Eu apenas usei uma `div` aqui. Esse é um mal necessário quando se usa React.

**JSX** 
```js
return (
  <div>
    <span>
      Hello
    </span>
    <span>
      World
    </span>
  </div>
);
```

### Anexando a UI ao DOM
Agora precisamos "anexar" a UI ao DOM para vermos o `Hello World`. Para fazer isso, **adicione `ReactDOM.render()` abaixo do código que acabamos de escrever:** 

**JSX** 
```js
var TweetBox = React.createClass({
  ...
});

ReactDOM.render(
  <TweetBox />,
  document.getElementById("container")
);
```

> **Nota:** a elipse (`...`) no trecho de código indica que uma parte do código foi omitida para maior clareza. Em outras palavras, não toque nessa parte do código, deixe-a como está.

`ReactDOM.render` recebe dois argumentos. O primeiro argumento é o objeto UI, que é `<VariableName />`. O segundo argumento é o objeto DOM (neste caso, `document.getElementById("container")`). Colocados juntos, o código acima renderiza a UI `TweetBox` dentro da `<div id="container">`.

Agora, você deve ver `Hello World` aparecendo no seu JSBin. Parabéns, você escreveu sua primeira interface de usuário (UI) com React!

### Escreva o HTML Correto Para a Tweet Box
Agora, ao invés de `Hellow World`, nós vamos implementar o HTML para a caixa de tweet. Troque o código dentro de `render()` por este:

**JSX** 
```js
return (
  <div className="well clearfix">
    <textarea className="form-control"></textarea>
    <br/>
    <button className="btn btn-primary pull-right">Tweet</button>
  </div>
);
```

Existem duas coisas que você precisa estar atento:

- **Não use `class`**. Ao invés, use `className`. Isso é devido ao fato do JSX ser traduzido para JS, e `class` é uma palavra reservada na nova versão do JS.
- **Se você usar `<br>` ao invés de `<br/>`, isso não vai funcionar**. Certifique-se de colocar um `/` em tags "autor fechantes".

Todo o resto deve ser o mesmo com o exemplo jQuery anterior.

Se você digitou tudo corretamente, você vai conseguir ver a caixa de tweet no seu JSBin. **Se nada estiver aparecendo em *ouput*, verifique seu código cuidadosamente, certificando-se que não tem nenhum erro de digitação.** 

É isso para esse passo! [Aqui está o JSBin para essa parte](http://jsbin.com/vajica/10/edit?html,js,output).

## Passo 5 - Reimplementando a Primeira Funcionalidade - Botão Tweet Deve Estar Inicialmente Desabilitado - em React (5 - 10 minutos)
Vamos reimplementar com React a primeira funcionalidade que implementamos usando jQuery:

**Funcionalidade 1:** o botão "Tweet" deve estar inicialmente desabilitado. Quando tiver pelo menos um caracter no campo de texto, o botão "Tweet" deve ficar ativo.

[Aqui está](http://jsbin.com/wewimu/3/edit?html,js,output) o código jQuery que escrevemos:

**HTML** 
```html
<!DOCTYPE html>
<html>
<head>
<script src="https://code.jquery.com/jquery-2.1.1.min.js"></script>
  <link href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.5/css/bootstrap.min.css" rel="stylesheet" type="text/css" />
  <meta charset="utf-8">
  <title>JS Bin</title>
</head>
<body>
  <div class="well clearfix">
    <textarea class="form-control"></textarea><br>
    <button class="btn btn-primary pull-right">Tweet</button>
  </div>
</body>
</html>
```

**JS** 
```js
// Initially disable the button
$("button").prop("disabled", true);

// When the value of the text area changes...
$("textarea").on("input", function() {
  // If there's at least one character...
  if ($(this).val().length > 0) {
    // Enable the button.
    $("button").prop("disabled", false);
  } else {
    // Else, disable the button.
    $("button").prop("disabled", true);
  }
});
```

Vamos ver como fazer isso com React.

**Inicie com o JSBin to passo anterior.**

> **Dica:** já que você não vai tocar no HTML com React, **você pode fechar a aba HTML do JSBIN**, assim você obtém mais espaço na sua tela.

**Primeiro, vamos desativar o botão adicionando `disabled`**.

**JSX** 
```js
render: function() {
  return (
    ...
    <button className="..." disabled>Tweet</button>
    ...
  );
}
```

O botão agora deve estar desabilitado. Note que na nossa implementação jQuery nós escrevemos `$("button").prop("disabled", true);` para desabilitar o botão inicialmente, mas poderíamos ter modificado a tag `button` como feito acima.

Agora, precisamos abilitar o botão quando tiver pelo menos um caracter no campo de texto.

### Manipulando Eventos *Change* (de Alteração)
