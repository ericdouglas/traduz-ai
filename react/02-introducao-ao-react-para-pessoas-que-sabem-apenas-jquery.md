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
Comece com nossa [implementação jQuery anterior](http://jsbin.com/durima/2/edit?html,js,output).

Vamos alterar o HTML e o JS. Antes, vamos anexar um manipulador ao `$("button")`, mas isso não vai funcionar se tivermos dois botões. **Dessa forma, vamos modificar o HTML assim:**

**HTML**
```html
...
<button class="js-tweet-button btn btn-primary pull-right" disabled>Tweet</button>
<button class="js-add-photo-button btn btn-default pull-right">Add Photo</button>
...
```

Aqui estão as alterações:

- **Adicionado o segundo botão chamado "Add Photo"**.
- **Adicionadas as classes `js-tweet-button` e `js-add-photo-button` para cada botão.** Elas estão prefixadas com `js-` pois são usadas apenas pelo JS e não pelo CSS.
- **Adicionado o atributo `disabled` ao botão Tweet**, então você não precisa fazer isso com JS.

Agora, reescreva todo o arquivo JS dessa forma:

**JS**
```js
$("textarea").on("input", function() {
  $("span").text(140 - $(this).val().length);

  if ($(this).val().length > 0) {
    $(".js-tweet-button").prop("disabled", false);
  } else {
    $(".js-tweet-button").prop("disabled", true);
  }
});
```

Aqui estão as mudanças:

- **(Importante) Removido `$("button").prop("disabled", true);` da primeira linha** pois adicionei o atributo `disabled` ao botão Tweet.
- **Trocado `$("button")` com `$(".js-tweet-button")`** dessa forma podemos distinguí-lo do `.js-add-photo-button`.


### Adicionando o Botão
A seguir, vamos implementar uma das funcionalidades:

- Clicando no botão "Add Photo" alterna o estado ON/OFF. **Se estiver ON, o botão vai dizer `✓ Photo Added`**.

Para fazer isso, **adicione esse código**:

**JS**
```js
$("textarea").on("input", function() {
  ...
});

$(".js-add-photo-button").on("click", function() {
  if ($(this).hasClass("is-on")) {
    $(this)
      .removeClass("is-on")
      .text("Add Photo");
  } else {
    $(this)
      .addClass("is-on")
      .text("✓ Photo Added");
  }
});
```

Usamos a classe `is-on` para rastrear o estado. **Teste para ver se isso funciona** clicando no botão "Add Photo" várias vezes e vendo o texto alternar.

### Diminuindo o Contador de Caracteres
Agora, vamos implementar essa funcionalidade:

- Se o botão "Add Photo" estiver ON, **o número de caracteres disponíveis vai dimiminuir em 23**.

Para fazer isso, **modifique o manipulador *click* que acabamos de adicionar da seguinte forma**:

**JS**:
```js
if ($(this).hasClass("is-on")) {
  $(this)
    .removeClass("is-on")
    .text("Add Photo");
  $("span").text(140 - $("textarea").val().length);
} else {
  $(this)
    .addClass("is-on")
    .text("✓ Photo Added");
  $("span").text(140 - 23 - $("textarea").val().length);
}
```

Mudamos o texto em `span` em cada click. Se o botão fica ON, então precisamos subtrair o o tamanho do texto, o que é `140 - 23`. Usamos `140 - 23` para sermos claros - normalmente usaríamos constantes.

**Verifique se tudo está funcionando** clicando no botão "Add Photo".

### Consertando o Manipulador Input (de entrada)
Entretanto a funcionalidade não está completa - **se você tiver o botão "Add Photo" ON e começar a digitar na área de texto, os caracteres remanescentes ficarão dessincronizados.**

Para consertar isso, **nós também precisamos de atualizar o manipulador de entrada (input) do `textarea`**:

**JS**
```js
$("textarea").on("input", function() {
  if ($(".js-add-photo-button").hasClass("is-on")) {
    $("span").text(140 - 23 - $(this).val().length);
  } else {
    $("span").text(140 - $(this).val().length);
  }

  if (...) {
    ...
});
```

**Verifique se a funcionalidade está funcionando** clicando no botão "Add Photo" e digitando algum texto.

### Eu sei que isso está levando tempo...
Mas continue! O código jQuery aqui deve estar confuso, não se preocupe!

### Implementando a Última Funcionalidade
A última funcionalidade que precisamos implementar é esta:

- Se o botão "Add Photo" está ON, **mesmo se não tiver nenhum texto digitado, o botão Tweet vai ficar ativo**.

Para fazer isso, **nós precisamos modificar o manipulador click do botão "Add Photo"**:

**JS**
```js
$(".js-add-photo-button").on("click", function() {
  if ($(this).hasClass("is-on")) {
    ...
    if ($("textarea").val().length === 0) {
      $(".js-tweet-button").prop("disabled", true);
    }
  } else {
    ...
    $(".js-tweet-button").prop("disabled", false);
  }
});
```

Aqui a explicação:

- Se o botão "Add Photo" passar de ON para OFF (condição `if`), nós precisamos verificar se não existe texto digitado e se não tiver, desabilitar o botão tweet.
- Se o botão "Add Photo" passer de OFF par ON (condição `else`), nós sempre ativaremos o botão tweet.

### Novamente, isso está quebrado
**Nós não terminamos ainda.** Os passos a seguir vão quebrar o código. **Tente repetí-los**:

- Ative o botão "Add Photo".
- Digite algum texto.
- Delete todo o texto.
- O botão "tweet" deveria permanecer ativo porque o botão "Add Photo" está ON, mas isso não é o que ocorre.

Isso significa que nosso manipulador de *input* (entrada) do `textarea` está com alguma lógica faltando. Para consertar isso, **nós precisamos adicionar outra condição na declaração `if` no manipulador de input**.

**JS**
```js
$("textarea").on("input", function() {
  ...
  if ($(this).val().length > 0 || $(".js-add-photo-button").hasClass("is-on")) {
    ...
  } else {
    ...
  }
});
```

Nós adicionamos o seguinte verificador para ver se o botão está ou não desativado:

- Quando o texto mudar, se o botão "Add Photo" estiver ON, não desabilite o botão tweet.

**Tente os passos acima novamente** e dessa vez o código não vai quebrar.

## Passo 11: Refletindo Sobre o Código jQuery - Por que Tão Confuso? (5 minutos)
[Aqui está](http://jsbin.com/xupice/2/edit?html,js,output) o resultado final do código HTML e JS do passo anterior.

**Dê uma olhada no código jQuery novamente.** Ele é muito confuso. Ele está bem confuso. Se você for deixar o código dessa forma, você provavelmente terá que colocar alguns comentários para lembrar o que você fez. Existem claros sinais de duplicação, mas você tem que pensar um pouco em como refatorar.

A questão é: **como isso ficou tão feio tão rápido?**

E a resposta tem a ver com o **"jeito jQuery"** de codificar que falamos sobre anteriormente. Relembre este diagrama:

![Estilo jQuery](http://reactfordesigners.com/images/labs/jquery-style-1.png)
> Manipulador de evento -> Muda o DOM

Isso é simples quando temos apenas 1 evento e 1 DOM. Entretanto, como nós acabamos de ver, **se vários manipuladores de evento estão modificando várias partes do DOM, o código fica feio.**

![Estilo jQuery](http://reactfordesigners.com/images/labs/jquery-style-2.png)
> Manipulador de evento de TextArea altera vários elementos do DOM - Manipulador de Evento do Botão "Add Photo" altera vários elementos do DOM

Imagine adicionar mais funcionalidades que influenciem no limite de caracteres e no estado do botão tweet. O diagrama acima teria ainda mais setas. E o código ficaria "ingerenciável".

Você pode, em teoria, mitigar isso refatorando o código em funções reutilizáveis. Mas você ainda terá que pensar muito toda vez que for adicionar algo novo.

> Alguém no Hacker News me enviou o [código jQuery refatorado](http://pastebin.com/wbGZZs7U). Muito limpo mas novamente, isso requer algum esforço mental.

Agora, vamos ver como fazer a mesma coisa com React. **Dica: vai ser muito mais simples.**

## Passo 12: O Botão "Add Photo" com React (10-20 minutos)
Comece com nossa [implementação prévia em React.](http://jsbin.com/lizoco/9/edit?html,js,output)

### Adicionando o Botão
Primeiro, vamos adicionar o botão "Add Photo". Modifique o JSX:

**JSX**
```js
<button ...>Tweet</button>
<button className="btn btn-default pull-right">Add Photo</button>
```

Agora, **vamos adicionar um manipulador de click** a este botão para que o texto mude de `Add Photo` para `✓ Photo Added`. Relembre o jeito React de escrever código:

![Estilo React](http://reactfordesigners.com/images/labs/react-style-2.png)
> O Manipulador de evento altera o mágico "state", e cada vez que o *state* é alterado, a função `render()` é chamada novamente.

Nós vamos:

1. **Criar uma variável *state*** que vai rastrear se o botão "Add Photo" está ON ou OFF.
1. **Usar o *state*** em `render()` para decidir se mostramos `Add Photo` ou `✓ Photo Added`.
1. **Modificar o *state*** no manipulador click.

Para (1), **vamos modificar `getInitialState`** e adicionar um par chave-valor no *state* para rastrear se a foto foi adicionada ou não:

**JSX**
```js
getInitialState: function() {
  return {
    text: "",
    photoAdded: false
  };
},
```

Para (2), **vamos modificar a marcação JSX** para o botão "Add Photo". Vamos ter o botão dizendo "Photo Added" se `this.state.photoAdded` for `true`. Podemos usar uma expressão ternária aqui.

**JSX**
```js
<button className="btn btn-default pull-right">
  {this.state.photoAdded ? "✓ Photo Added" : "Add Photo" }
</button>
```

Finalmente, para a tarefa (3), **vamos anexar o manipulador click no JSX** assim como fizemos com `textarea`:

**JSX**
```js
<button className="btn btn-default pull-right"
  onClick={this.togglePhoto}>
  {this.state.photoAdded ? "✓ Photo Added" : "Add Photo" }
</button>
```

E **adicionar o método manipulador que reverte `this.state.photoAdded`**:

**JSX**
```js
togglePhoto: function(event) {
  this.setState({ photoAdded: !this.state.photoAdded });
},
```

Agora, clicando em `Add Photo` deve fazer o texto alternar. **Teste você mesmo**.

### Diminuindo o Contador de Caracteres
Vamos agora implementar a nova funcionalidade:

- Se o botão "Add Photo" estiver ON, **o número de caracteres disponíveis deve ser diminuído em 23**.

Atualmente, o número de caracteres disponíveis é mostrado da seguinte maneira em `render()`:

**JSX**
```js
<span>{140 - this.state.text.length}</span>
```

Isso agora depende também de `this.state.photoAdded`, então precisamos de um `if` e `else` aqui.

Entretanto, **no JSX, você não pode escrever `if` ou `else` dentro de um `{...}`**. Você pode usar uma expressão ternária (`a ? b : c`) como fizemos anteriormente, mas isso seria muito longo nesse caso.

Normalmente a forma mais simples nessa situação é refatorar uma condicional em um método. Vamos tentar isso.

**Primeiro, modifique o código acima para usar um método, como este:**

**JSX**
```js
<span>{ this.remainingCharacters() }</span>
```

E defina o método dessa forma:

**JSX**
```js
remainingCharacters: function() {
  if (this.state.photoAdded) {
    return 140 - 23 - this.state.text.length;
  } else {
    return 140 - this.state.text.length;
  }
},
```

Agora, a contagem dos caracteres restantes deve ser atualizada corretamente quando o botão "Add Photo" alternar.

**Dúvida**: Em `render()`, por que `{ this.remainingCharacters() }` tem `()` mas `{ this.handleChange }` e `{ this.togglePhoto }` não?

Boa pergunta. Vamos dar uma olhada em `render()` novamente:

**JSX**
```js
render: function() {
  return (
    ...
      <textarea className="..."
                onChange={ this.handleChange }></textarea>
      ...
      <span>{ this.remainingCharacters() }</span>
      ...
      <button className="..."
        onClick={ this.togglePhoto }>
        ...
      </button>
    </div>
  );
```

**Resposta:**

- Nós escrevemos o método `remainingCharacters()` para **retornar um número**. Precisamos pegar esse número e colocá-lo entre `<span></span>`, então precisamos **chamar o método `remainingCharacters()`** usando `()`. Por isso temos um `()` em `remainingCharacters()`.
- Por outro lado, `handleChange` e `togglePhoto` são **manipuladores de evento**. Nós queremos que esses métodos sejam chamados apenas quando o usuário interagir com a interface (mudando o texto ou clicando o botão). Para fazer isso, em `render()`, nós precisamos de escrevê-los sem `()` e atribuí-los a atributos como `onChange` e `onClick`.

### Os Estados do Botão Tweet
Temos mais uma funcionalidade para implementar:

- Se o botão "Add Photo" estiver ON, **mesmo que nenhum texto tenha sido digitado, o botão Tweet permanecerá ativo.**

Isso é de fato muito fácil de fazer. Anteriormente, a opção `disabled` do botão tweet foi definida assim:

**JSX**
```js
<button ... disabled={this.state.text.length === 0}>...</button>
```

Em outras palavras, o botão tweet era desabilitado se o tamanho do texto fosse 0. **Agora, o botão tweet vai ser desabilitado se:**

- O tamanho do texto for 0 **e**:
- O botão "Add Photo" estiver OFF.

Então a lógica vai ficar assim:

**JSX**
```js
<button ... disabled={this.state.text.length === 0 && !this.state.photoAdded}>...</button>
```

Ou, você pode simplificar o código acima utilizando `remainingCharacters()`. Se existirem 140 caracteres restantes, isso significa que nenhum texto foi digitado e que o botão "Add Photo" está OFF, então o botão Tweet deve ficar desativado.

**JSX**
```js
<button ... disabled={this.remainingCharacters() === 140}>...</button>
```

É isso! Tente alternar o botão "Add Photo" e verificar se o botão Tweet fica ativado/desativado corretamente.

### Terminamos!
Isso foi fácil. [Aqui o JSBin com o resultado](http://jsbin.com/fitiha/10/edit?js,output).

## Passo 13: Reflexão Sobre o Código React - Por que tão simples? (5 minutos)
As mudanças para acomodar o botão "Add Photo" foram mínimas usando React. Sem necessidade de refatorações. Por que aconteceu isso?

Novamente, isso tem a ver com a forma de escrever código UI do React. Em React, manipuladores de evento modificam o "*state*" (estado), e sempre que o estado é alterado, o React automaticamente chamada `render()` para atualizar a UI.

![Estilo React](http://reactfordesigners.com/images/labs/react-style-2.png)

Nesse exemplo em particular, o diagrama agora vai parecer com isso:

![Estilo React para nossa aplicação](http://reactfordesigners.com/images/labs/react-style-3.png)
> O manipulador de evento textArea modifica o estado, assim como o manipulador de evento do botão "Add Photo". Quando o *state* é modificado, o React automaticamente chamada `render()`

O estado se torna algo intermediário que se situa entre manipuladores de evento e `render()`:

- Manipuladores de evento não precisam de se preocupar com a parte do DOM que será alterada. Eles apenas precisam de definir o *state* (estado).
- Similarmente, quando você escreve `render()`, tudo que você precisa de se preocupar é qual é o `state` atual.

### Comparando com jQuery
Você pode imaginar o que aconteceria se a UI recebesse mais funcionalidades. Sem o "*state*" intermediário, teríamos um tempo difícil gerenciando essa complexidade. Isso é o motivo porque você gostaria de usar React ao invés de jQuery para interfaces complexas.

![jQuery versus React](http://reactfordesigners.com/images/labs/jquery-style-vs-react-style.png)

Novamente, **é possível** escrever código jQuery limpo que não parece um macarrão. Mas você tem que definir a estutrura do código por si mesmo e pensar sobre como refatorar o código cada vez que você adicionar uma funcionalidade. O React fornece essa estrutura para você e reduz a sobrecarga cognitiva.

## Passo 14: A Funcionalidade Final - Destacando os Caracteres Excedentes (5 minutos)
