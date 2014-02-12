# Simples Guia Passo-a-Passo Para Desenvolvedores Front-End Iniciarem Com Node.js, Express, Jade e MongoDB

### Configure uma Aplicação *Full Stack JavaScript* e a tenha funcionando em 30 minutos. Faça-a conversar com seu banco de dados em outros 30.

#### Artigo Traduzido. Veja o original [aqui](http://cwbuecheler.com/web/tutorials/2013/node-express-mongo/).

> [Você pode encontrar/forkar este tutorial e todo o projeto de exemplo no Github.](http://cwbuecheler.com/web/tutorials/2013/node-express-mongo/)

## Introdução

Existem aproximadamente cem milhões de tutorials na web para obter um "Hello World!" com Node.js. Isto é ótimo! Isto é especialmente bom se seu objetivo é comprimentar o mundo e depois abandonar sua carreira na web e passar o resto da sua vida como um jóquei, ou qualquer outra coisa. Isto realmente não descreve muitos de nós, então vamos olhar por mais tutoriais.

Em minha experiência, o "próximo nível" de tutoriais que achamos estão 30 níveis acima destes. Vamos de um "Hello World" para uma construção de um sistema inteiro de blog com comentários. Isto também é ótimo, mas muitas vezes estes tutoriais assumem que o leitor tem um sólido conhecimento básico, e então eles soltam um monte de funções sobre você de uma só vez. Eu aprendo melhor fazendo um monte de etapas intermediárias, menores, e não acho que sou o único.

Eu não sou o único, certo?

Bom, boa notícia a todos! Eu li e fiz muitos tutoriais, até que as coisas finalmente funcionaram. Tenho um projeto web rodando que usa Node.js, o framework Express, o pré-processador de HTML chamado Jade e o MongoDB para os dados. Sou capaz de ler e escrever a partid do banco de dados. A partir disso, o céu é o limite.

Aqui está o acordo: Vou mostrar à você como pegar todas essas coisas e configurá-las. Vou assumir que você é um desenvolvedor front-end que conhece o suficiente de HTML5/CSS3/JavaScript para que eu não tenha que explicá-los.

Seu aplicativo vai ficar bonito, vai se conectar com um DB (banco de dados), ele vai obter alguns resultados, e vai fazer coisas com estes resultados. E por diversão também vamos fazê-lo salvar dados no DB. Através de tudo isso, eu irei explicar o que o código faz, e como escrevê-lo, ao invés de somente fornecer a você funções massantes para olhar. Vamos partir de nada instalado, para uma aplicação que manipula banco de dados em uma linguagem que você compreende totalmente, e a fundação necessária para construir funcionalidades adicionais para sua app. E vamos fazer isso aproximadamente em 60 minutos entre instalação e codificação. Isso é impressionante? Sugiro que sim.

Vamos lá!

## PARTE 1 - 15 MINUTOS DE INSTALAÇÃO

Se você está realmente começando do zero, então ter tudo rodando leva um pouco de tempo. Nada disso é difícil. Eu rodo o Windows 8 na minha máquina, então isso pode ser um pouco diferente no Mac, Ubuntu ou qualquer outro sistema *nix, mas é basicamente a mesma coisa em todos os casos.

### PASSO 1 - INSTALANDO NODE.JS

Isso é realmente fácil. Vá para o site do [Node.js](http://nodejs.org/) e clique no grande botão verde *Install*. Ele irá detectar seu sistema operacional (SO) e dar a você o instalador apropriado (se por algum motivo isso não ocorrer, clique no botão de download e pegue o que você precisa). Rode o instalador. É isso, você instalou o Node.js e o, igualmente importante, NPM - Node Package Manager - que permite que você adicione todo tipo de complemento ao Node de forma rápida e fácil.

* **Abra o prompt de comando**
* **`cd` até o diretório em que você deseja manter suas aplicações de teste**

### PASSO 2 - INSTALANDO O EXPRESS

Agora que temos o Node rodando, nós precisamos do resto das coisas para criar, realmente, um website que funcione. Para fazer isso, vamos instalar o Express, que é um framework que pega o Node a partir de uma aplicação simples e o transforma em algo que se comporta mais como um servidor web que todos nós usamos (e talvez um pouco mais que isso). Nós precisamos iniciar com o Express, pois iremos utilizar seu *scaffolding* (estrutura) para obter todo o resto que queremos (mais sobre isso em um instante). Então, digite isso:

```sh

$ npm install -g express

```

Isso instala algumas funcionalidades do núcleo do Express junto com a instalação do Node, tornando-o disponível globalmente, então podemos usá-lo em qualquer lugar que quisermos. Você vai ver um monte de texto em seu prompt de comando, vários http 304 e GETs. Tudo bem. O Express está agora instalado e disponível.

### PASSO 3 - CRIANDO UM PROJETO EXPRESS

Vamos usar Express e Jade, mas não o pré-processador CSS Stylus (que as pessoas geralmente usam nesta configuração). Temos que usar o Jade ou outro motor de templates para ter acesso aos dados baseados em Node/Express. Jade não é difícil de se aprender se você já conhece HTML. Apenas lembre-se que você realmente tem que ter atenção a indentação, ou coisas vão sair muito erradas.

De qualquer forma, continue no seu diretório onde está armazenando sua aplicação node e digite isso:

```sh

$ express --sessions nodetest1

```

Aperte enter e veja o que acontece. Irá aparecer algo como isso:

```sh

eo_op:~/estudos/nodejs $ express --sessions nodetest1
create : nodetest1
create : nodetest1/package.json
create : nodetest1/app.js
create : nodetest1/routes
create : nodetest1/routes/index.js
create : nodetest1/routes/user.js
create : nodetest1/views
create : nodetest1/views/layout.jade
create : nodetest1/views/index.jade
create : nodetest1/public/images
create : nodetest1/public/javascripts
create : nodetest1/public
create : nodetest1/public/stylesheets
create : nodetest1/public/stylesheets/style.css

install dependencies:
$ cd nodetest1 && npm install

run the app:
$ node app

```

### PASSO 4 - EDITANDO AS DEPENDÊNCIAS

Tudo bem, agora que temos uma estrutura básica, mas ainda não terminamos. Você vai notar que a rotina de instalação do Express criou um arquivo chamado package.json em seu diretório `nodetest1`. Abra este arquivo, ele vai parecer com isso:

```json

{
  "name": "application-name",
  "version": "0.0.1",
  "private": true,
  "scripts": {
    "start": "node app.js"
  },
  "dependencies": {
    "express": "3.4.8",
    "jade": "*"
  }
}

```

Este é um arquivo básico JSON que descreve seu aplicativo e suas dependências. Nós precisamos adicionar algumas coisas a ele. Especificamente, o MongoDB e Monk. Vamos fazer nosso objeto `dependencies` se parecer com isso:

```json

"dependencies": {
    "express": "3.4.4",
    "jade": "*",
    "mongodb": "*",
    "monk": "*"
}

```

### PASSO 5 - INSTALAR AS DEPENDÊNCIAS

Agora definimos nossas dependências e estamos prontos para começar. Note que o astericos diz ao NPM "pegue a última versão" quando você roda a instalação, que estamos prestes a fazer.

Volte para seu prompt de comando, `cd` para o diretório *nodetest1* e digite isso:

```sh

$ npm install

```

Será impresso uma tonelada de coisas. Isto por causa que está sendo lido nosso arquivo JSON que acabamos de editar e a instalação de todas as coisas listadas no objeto *dependencies* (sim, incluindo o Express - nós instalamos o material de alto nível usando a flag `-g`, mas ainda temos que instalar algum código que será necessário para este projeto em particular). Uma vez que o NPM percorreu seu caminho, você terá um diretório `node_modules` que contém todas as suas dependências para este tutorial.

Agora você tem uma aplicação em pleno funcionamento e esperando para ser rodada. Vamos testá-la! **Vá para o diretório nodetest1** e digite:

```sh

$ node app.js

```

Aperte enter. Você vai obter isso:

```sh

Express server listening on port 3000

```

Incrível! Abra seu navegador e digite `http://locahost:3000`. Agora você verá a página de boas vindas do Express.

![boas vindas express](http://cwbuecheler.com/web/tutorials/2013/node-express-mongo/browsershot1.png)

Você tem agora seu próprio servidor web com Node.js, com a *engine* Express e o pré-processador Jade instalados. Não é tão difícil, né?

## PARTE 2 - OK. LEGAL. VAMOS FAZER O "HELLO WORLD!"

Abra seu editor de texto ou IDE favorita. Eu gosto muito do [Sublime Text](http://www.sublimetext.com/). Vá para o diretório `nodetest1` e abra o arquivo `app.js`. Esse é como o coração da sua app. Não há muitas surpresas lá. Aqui temos uma parte do que você irá ver lá:

```js

var express = require('express');
var routes = require('./routes');
var user = require('./routes/user');
var http = require('http');
var path = require('path');

```

Isso cria muitas variáveis básicas do JavaScript e as liga a certos pacotes, dependências, funcionalidades do Node e rotas. Rotas são como uma espécie de combinação de modelos e controladores nesta configuração - elas direcionam o tráfico e também contém alguma lógica de programação (você pode estabelecer uma arquitetura MVC mais tradicional com o Express se você quiser. Isso está fora do escopo deste artigo). Voltando ao momento onde nós configuramos este projeto, o Express criou todas essas coisas para nós. Vamos ignorar totalmente a rota *user* por agora e trabalhar somente na rota de nível superior (controlado por `nodetest1/routes/index.js`).

`nodetest1/app.js`
```js

var app = express();

```

Isto é importante, pois configura o Express e atribui nossa variável `app` a ele. A próxima seção usa esta variável para configurar um monte de coisas do Express.

`nodetest1/app.js`
```js

// todos ambientes
app.set( 'port', process.env.PORT || 3000 );
app.set( 'views', path.join( __dirname, 'views' ) );
app.set( 'view engine', 'jade' );
app.use( express.favicon() );
app.use( express.logger( 'dev' ) );
app.use( express.bodyParser() );
app.use( express.methodOverride() );
app.use( app.router );
app.use( express.static( path.join( __dirname, 'public' ) ) );

```

Isso configura a porta, que diz ao app onde encotrar as views, que engine usar para renderisar estas views (Jade), e chama alguns métodos para deixar as coisas funcionando. Note também que a linha final está dizendo ao Express para servir objetos estáticos no diretório *public*. Por exemplo, as imagens no diretório `../nodetest1/public/images`. Mas elas são acessadas pela url `http://localhost:3000/images`.

**NOTA:** Você vai precisar mudar esta linha:

`app.js`
```js

app.use( express.bodyParser() );

```

para:

```js

app.use( express.urlencoded() );

```

Em razão de evitar alguns avisos em seu console Node quando você rodar a aplicação. Isto é devido a algumas mudanças futuras bi Express e seus plugins. Se você não fizer esta mudança, sua aplicação vai continuar rodando, mas você irá ver texto sobre futuras *desaprovações* (deprecations) toda vez que você rodar isso.

`app.js`
```js

// development only
if ( 'development' == app.get( 'env' ) ) {
	app.use( express.errorHandler() );
}

```

Isso permite que você faça alguma checagem de erro durante o desenvolvimento. É importante, mas pela proposta deste tutorial não vamos fazer nada com isso.

`app.js`
```js

app.get( '/', routes.index );
app.get( '/users', user.list );

```

Isso diz a app quais rotas usar quando uma URI particular é solicitada. Note que a variável "user" está declarada acima, e é mapeada para /routes/user.js - nós vamos chamar a função de lista definida neste arquivo. Ou estaríamos se estivéssemos acessando a página de usuários, mas estamos ignorando-a, lembra?

`app.js`
```js

http.createServer( app ).listen( app.get( 'port' ), function () {
	console.log( 'Express server listening on port ' + app.get( 'port' ) );
} );

```

Por último, mas não menos importante, isso cria nosso servidor http e o lança. Bons tempos!

Agora então, vamos fazer algumas coisas. Não vamos fazer apenas um "Hello, World!" na nossa página index. Ao invés disso, vamos usar essa oportunidade para aprender um pouco mais sobre rotas e ver como o Jade trabalha para colocar as páginas em conjunto. Primeiro, vamos adicionar uma linha para manipular uma nova URI. Em baixo da seção `app.get()` no arquivo app.js, adicione esta linha:

```js

app.get('/helloworld', routes.helloworld);

```

Aperte `ctrl c` para encerrar o app.js em sua linha de comando, e então reinicie o processo e vá até `http://localhost:3000/helloworld`. Você deve obter um interessante erro do node e uma quebra na linha de comando. Isto porque nós não modificamos nossa rota para manipular esta requisição. Vamos fazer isso! Em seu editor de texto, abra sua pasta *routes*, encontre `index.js` e abra-o. Ele vai se parecer com isso:

`index.js`
```js

/*
 * GET home page.
 */

exports.index = function(req, res){
  res.render('index', { title: 'Express' });
};

```

Muito escasso, certo? Vamos adicionar uma nova página. Minha abordagem preferida é adicionar um novo arquivo de rota para o diretório de nível superior, mas nós não criamos um diretório *helloworld* completo nas views, então vamos apenas usar a rota index. No fim do arquivo, adicione este código:

```js

exports.helloworld = function ( req, res ) {
	res.render('helloworld', { title: 'Hello, World!' });
};

```

Isso é tudo que temos que fazer para rotear esta URI, mas nós não temos nenhuma para o `res.render` renderizar. É ai que o Jade entra. Abra sua pasta `views`, e então abra o arquivo `index.jade`. Antes de fazer qualquer coisa, **salve este arquivo como `helloworld.jade`**.

Agora dê uma olhada no código:

`helloworld.jade`
```jade

extends layout

block content
	h1= title
	p Welcome to #{title}

```

Isso é muito simples. Ele usa `extends` o arquivo `layoud.jade` como um template, e então dentro do bloco `content` definido no arquivo layout, ele altera o `header` e o `p` (parágrafo). Note o uso da variável `title` que configuramos acima, em nossa rota index.js. Isso significa que não temos que mudar sempre o texto para mostrar coisas diferentes na página home. Mas vamos mudar um pouco de qualquer forma para:

```jade

p Hello, World! Welcome to #{title}

```

Salve o arquivo, vá para o terminal e encerre sua aplicação `ctrl c`. Agora digite:

```sh

node app.js

```

É importante mencionar: mudanças nos templates Jade não necessitam do reinício do servidor, mas basicamente toda vez que você mudar um arquivo `.js`, como `app.js` ou um arquivo de rota, você vai precisar reiniciar para ver as mudanças.

Agora, com o servidor reiniciado, navegue até `http://localhost:3000/helloworld` e divirta-se com o texto completamente estúpido mostrado:

![helloworld](http://cwbuecheler.com/web/tutorials/2013/node-express-mongo/browsershot2.png)

Ok! Agora que temos nossa rota nos levando para nossa view. Vamos fazer alguma modelagem. Eu vou dar um momento para você, caso precise reparar seu cabelo ou maquiagem.

> Nota do tradutor: /\ AUSHDUHASUDHUASHDUAHSUDHAUSHDUHASUDHUASHDUHAASDUHASUHDUHASUdhuashDUHASUDH

## PARTE 3 - CRIANDO NOSSO DB E LENDO ALGO DELE