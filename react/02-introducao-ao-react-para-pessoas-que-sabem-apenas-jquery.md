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
