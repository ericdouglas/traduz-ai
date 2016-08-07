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
Primeiro, precisamos de esperar até o usuário adicionar algum texto. Na nossa implementação jQuery, escrevemos:

**JS**
```js
$("textarea").on("input", function() {
  ...
})
```

Com React, nós escrevemos o manipulador de evento como um **método**. Vamos chamá-lo de `handleChange`:

**JSX**
```js
React.createClass({
  handleChange: function(event) {
  },
  render: function() {
    ...
  }
});
```

Em seguida, nós invocamos esse *handler* (manipulador) quando algum texto for adicionado. Para fazer isso, **modifique a tag `textarea` em `render()` dessa forma:** 

**JSX** 
```js
<textarea className="form-control"
          onChange={this.handleChange}></textarea>
```

- Nós usamos o evento `input` para o jQuery, mas no React vamos usar `onChange` - você vai aprender como os eventos diferem no JSX do React na documentação do React então não se preocupe muito com isso agora.
- **Mais importante**, nós usamos a sintaxe `{...}` para incluir qualquer código JavaScript dentro do JSX na parte da sintaxe HTML. Nesse caso, nós queremos passar o manipulador `handleChange`, e adicionamos o prefixo `this.` pois é um método deste objeto da UI.
- Se você está acostumado com jQuery, isso pode parecer uma má prática, mas não se preocupe. Novamente, um aplicações grandes, o código vai ser mais gerenciável se essas marcações e comportamentos forem deixados juntos para cada parte da UI.

Para termos certeza que o manipulador está sendo chamado, **vamos adicionar um `console.log` dentro do `handleChange`**:

**JSX** 
```js
handleChange: function(event) {
  console.log(event.target.value);
},
```

O objeto `event` contém `target`, que é a `textarea`. Nós usamos `.value` para imprimir o valor atual da `textarea`.

**Em seu JSBin, abra a aba `console` para verificar a saída. Depois digite algo na caixa de tweet.** 

![Abra a aba console no jsbin](http://reactfordesigners.com/images/labs/console-tab.png)

Você pode testar isso [nesse JSBin](http://jsbin.com/kohudu/8/edit?js,output).

É isso para esse passo! Nós vamos finalizar essa funcionalidade no próximo passo.

**NOTA: Feche a aba console no JSBin quando você terminar.** Não vamos mais precisar dela.

## Passo 6: Implementando *State* (Estado) (10 - 15 minutos)
Vou explicar agora uma das maiores diferenças entre código jQuery e código React.

Em jQuery, quando algum evento ocorre, você geralmente altera o DOM (como fizemos anteriormente):

![Estilo jQuery](http://reactfordesigners.com/images/labs/jquery-style-1.png)
> O manipulador de evento altera o DOM.

Com React, **você não altera o DOM diretamente**. Ao invés, em um *event handler* (manipulador de evento), você modifica algo chamado **"state"** (estado). E isso é feito chamando `this.setState`.

![Estilo Rect](http://reactfordesigners.com/images/labs/react-style-1.png)
> O manipulador de evento altera o mágico "state" (estado)

Então, toda vez que o estado é atualizado, **`render()` é chamada novamente**. E **dentro de `render()` você pode acessa o estado**.

![Estilo React](http://reactfordesigners.com/images/labs/react-style-2.png)
> O manipulador de evento altera o estado mágico, e toda vez que o estado mágico é alterado a função `render()` é chamada novamente, e dentro de render você pode atualizar o estado

Essa é a forma que você atualiza a UI em resposta a um evento. Sim isso é confuso, deixe-me explicar usando código.

### Escrevendo o Manipulador de Evento
**Inicie com o JSBin do passo anterior.** Primeiro, temos que **inicializar o objeto *state*** - sem isso nada vai funcionar.

Para fazer isso, **precisamos escrever um método especial chamado `getInitialState` e ele retorna um objeto JS** , que se torna o estado inicial.

O que vai nesse objeto? **Vamos criar chave simples chamada `text`** e deixar que ela armazene tudo que está dentro da caixa de tweet.

**JSX**
```js
var TweetBox = React.createClass({
  getInitialState: function() {
    return {
      text: ""
    };
  },
  handleChange: ...
  render: ...
});
```

**A seguir, vamos modificar o manipulador de evento** para configurar o estado do campo `text` para qualquer coisa que esteja atualmente na caixa de tweet. Para fazer isso, **nós usamos um método especial do React chamado `setState` e passamos o par chave-valor atualizado**.

**JSX**
```js
handleChange: function(event) {
  this.setState({ text: event.target.value });
},
```

Agora, vamos verificar se o estado está sendo configurado corretamente escrevendo algum código para debug me `render()`.

Para isso, **simplesmente adicione `this.state.next` próximo do fim de `render()`, e use a sintaxe `{...}`** para chamar o código JS dentro da parte HTML da sintaxe do JSX.

**JSX** 
```js
render: function() {
  return (
    <div ...>
      ...
      <button ...>Tweet</button>

      <br/>
      {this.state.text}
    </div>
  )
}
```

**Agora, tente digitar algo na caixa de tweet**. O mesmo texto deve aparecer abaixo do botão.

Você pode testar isso [neste JSBin](http://jsbin.com/yabiqo/10/edit).

Agora o diagrama anterior deve fazer mais sentido para você.

![Estilo React](http://reactfordesigners.com/images/labs/react-style-2.png)

### Removendo o Código de Debug
Uma vez confirmado que o estado está sendo configurado corretamente, **remova o código de debug que foi adicionado:** 

**JSX** 
```md
<br/>
{this.state.text}
```

### Habilitando/Desabilitando o Botão
Agora que podemos detectar as mudanças no texto, tudo que resta é habilitar/desabilitar o botão dependendo da inserção do texto.

Usando o estado, podemos usar essa lógica:

- Se `this.state.text.length ===0`, então o botão deve estar desabilitado.

Para fazer isso no React, **adicione o atributo `disabled`, e configure-o com o valor retornado por `this.state.text.length === 0`.** Uma vez que isso é código JS, você precisa de envolvê-lo com `{}`.

**JSX** 
```js
<button className="btn btn-primary pull-right"
        disabled={this.state.text.length === 0}>Tweet</button>
```

Se você escrever `disabled="true"` ou `disabled="false"` em HTML puro isso não vai funcionar - em HTML puro, você precisa de remover o atributo `disabled` para habilitar o botão. Mas React **não** é HTML puro - ele faz essa mágica por trás das panos:

- Se você fizer `disabled={true}` no JSX, isso é convertido para `<button ... disabled>` em HTML.
- Se você fizer `disabled={false}` no JSX, o atributo `disabled` é removido da tag `button` no HTML.

Isso funciona para outros atributos booleanos como `checked`. Não está oficialmente documentado por agora (no momento em que escrevo), mas deve ser incluído em breve.

O JSBin resultando [está aqui](http://jsbin.com/lutefu/11/edit).

### Reflexões
Novamente, mantenha essa diferença entre o jQuery e o React em mente antes de seguir para o próximo passo:

- Em jQuery, você escreve manipuladores de evento que modificam o **DOM**.
- Em React, você escreve manipuladores de evento que modificam o **estado**. E você vai escrever `render()` para refletir o estado atual.

## Passo 7: Contador de Caracteres Restantes em jQuery (5 minutos)
A próxima funcionalidade que vamos implementar é o contador de caracteres restantes.

![Contador de caracteres restantes](http://reactfordesigners.com/images/labs/tweet-box-character-count.png)

Aqui está a especificação:

- O contador de caracteres deve mostrar `140 - o número de caracteres digitados`.

**Vamos implementar isso primeiro em jQuery e depois em React.**

Vamos começar a partir da nossa implementação jQuery anterior. Coloque seu código React na espera. **A partir de agora, vou te dar o novo código para começar no início de cada capítulo**, pois estamos alternando entre jQuery e React. Isso significa que depois que você terminar um passo, você pode brincar com o código antes de ir para o próximo passo.

[JSBin com código jQuery do exemplo anterior](http://jsbin.com/wewimu/3/edit?html,js,output).

Primeiro, **adicione o contador de caracteres no HTML usando `span`**.

**HTML**
```html
<textarea ...></textarea><br>
<span>140</span>
<button ...>Tweet</button>
```

E **dentro do manipulador do `input` no JS, adicione esse código para atualizar o contador de caracteres**:

**JS**
```js
$("textarea").on("input", function() {
  $("span").text(140 - $(this).val().length);
  ...
});
```

É isso! **Tente digitar na caixa de tweet** e você vai ver o contador de caracteres atualizado. [Aqui está o JSBin](http://jsbin.com/durima/2/edit?html,js,output).

## Passo 8: Contador de Caracteres Restantes em React (5 minutos)
Como fazer isso em React? Você deve tentar fazer isso por sua conta. Comece com nossa [implementação anterior em React](http://jsbin.com/lutefu/11/edit).

**Dicas**:

- Não precisa mudar os métodos `getInitialState()` ou `handleChange()`
- Use `this.state.text.length` no `render()`.

### Resposta:

**Adicione isso depois do `<br/>` no `render()`**:

**JSX**
```js
<span>{140 - this.state.text.length}</span>
```

[Resultado no JSBin.](http://jsbin.com/lizoco/9/edit?html,js,output)

Muito fácil? Em dúvida por que o React é muito melhor que o jQuery? Bom, o próximo passo é mais complexo e é onde o React realmente brilha.

## Passo 9: O Botão "Add Photo" (Adiciona Foto) (5 minutos)
Como nossa próxima funcionalidade, vamos adicionar o botão *Add Photo* na interface do usuário. Ai é quando as coisas ficam complicadas.

![Botão Add photo](http://reactfordesigners.com/images/labs/tweet-box-add-photo.png)

Entretando, **nós não vamos deixar os usuários fazerem upload de fotos.** Ao invés, isso é o que vamos fazer.

Quando você faz upload de uma foto no Twitter, isso conta contra o número de caracteres que você pode usar. Na minha tentativa, essa ação diminuiu o número restante de caracteres de 140 para 117:

![Caracteres restantes ao adicionar uma foto](http://reactfordesigners.com/images/labs/tweet-box-add-photo-explanation.png)

Aqui está a especificação do que vamos fazer:

- Criar um botão "Add Photo"
- Clicar nesse botão alterna o estado ON/OFF (ligado/desligado). **Se estiver ON, o botão vai dizer `✓ Photo Added`**.
- Se o botão estiver ON, **o número de caracteres disponíveis vai diminuir em 23**.
- Também, se o botão estiver ON, **mesmo que não tenha nenhum texto digitado, o botão "tweet" deverá permanecer ativo**.

[Aqui está a demonstração no JSBin](http://jsbin.com/sanuqi/6/edit). **Tente clicar no botão "Add Photo"** e ver o que acontece ao contador de caracter e ao botão "tweet".

Vamos implementar isso. Primeiro vamos tentar com Jquery.

## Passo 10: O Botão "Add Photo", com jQuery (15 - 20 minutos)
