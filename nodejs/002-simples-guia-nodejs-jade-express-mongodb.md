# Simples Guia Passo-a-Passo Para Desenvolvedores Front-End Iniciarem Com Node.js, Express, Jade e MongoDB

* **Artigo Original**: [THE DEAD-SIMPLE STEP-BY-STEP GUIDE FOR FRONT-END DEVELOPERS TO GETTING UP AND RUNNING WITH NODE.JS, EXPRESS, JADE, AND MONGODB](http://cwbuecheler.com/web/tutorials/2013/node-express-mongo/)
* **Tradução**: [Eric Douglas](https://github.com/ericdouglas)
* **Atualização** [Alex Aleluia](https://github.com/alexaleluia12)

### Configure uma Aplicação *Full Stack JavaScript* e a tenha funcionando em 30 minutos. Faça-a conversar com seu banco de dados em outros 30.

> [Você pode encontrar/forkar este tutorial e todo o projeto de exemplo no Github.](http://cwbuecheler.com/web/tutorials/2013/node-express-mongo/)

> Esta é apenas uma atualização do [tutorial antigo](https://github.com/ericdouglas/traduz-ai/blob/master/nodejs/002-simples-guia-nodejs-jade-express-mongodb.md) para você não pesquisar cada declaração que o tutorial faz.

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

$ npm install express-generator -g

```

Isso instala algumas funcionalidades do núcleo do Express junto com a instalação do Node, tornando-o disponível globalmente, então podemos usá-lo em qualquer lugar que quisermos. Você vai ver um monte de texto em seu prompt de comando, vários http 304 e GETs. Tudo bem. O Express está agora instalado e disponível.

> Vou considerar que o seguite comando deve retorar uma versão igual ou superior
```sh
$ express --version
4.11.1
```

### PASSO 3 - CRIANDO UM PROJETO EXPRESS

Vamos usar Express e Jade, mas não o pré-processador CSS Stylus (que as pessoas geralmente usam nesta configuração). Temos que usar o Jade ou outro motor de templates para ter acesso aos dados baseados em Node/Express. Jade não é difícil de se aprender se você já conhece HTML. Apenas lembre-se que você realmente tem que ter atenção a indentação, ou coisas vão sair muito erradas.

De qualquer forma, continue no seu diretório onde está armazenando sua aplicação node e digite isso:

```sh

$ express nodetest1

```

Aperte enter e veja o que acontece. Irá aparecer algo como isso:

```sh

eo_op:~/estudos/nodejs $ express nodetest1
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
create : nodetest1/bin
create : nodetest1/bin/www

install dependencies:
$ cd nodetest1 
$ npm install

run the app:
$ DEBUG=nodetest1 ./bin/www

```

### PASSO 4 - EDITANDO AS DEPENDÊNCIAS

Tudo bem, agora que temos uma estrutura básica, mas ainda não terminamos. Você vai notar que a rotina de instalação do Express criou um arquivo chamado package.json em seu diretório `nodetest1`. Abra este arquivo, ele vai parecer com isso:

```json

{
  "name": "application-name",
  "version": "0.0.1",
  "private": true,
  "scripts": {
    "start": "node ./bin/www"
  },
  "dependencies": {
    "body-parser": "~1.10.2",
    "cookie-parser": "~1.3.3",
    "debug": "~2.1.1",
    "express": "~4.11.1",
    "jade": "~1.9.1",
    "morgan": "~1.5.1",
    "serve-favicon": "~2.2.0",
}

```

Este é um arquivo básico JSON que descreve seu aplicativo e suas dependências. Nós precisamos adicionar algumas coisas a ele. Especificamente, o MongoDB e Monk. Vamos fazer nosso objeto `dependencies` se parecer com isso:

```json

"dependencies": {
    "body-parser": "~1.10.2",
    "cookie-parser": "~1.3.3",
    "debug": "~2.1.1",
    "express": "~4.11.1",
    "jade": "~1.9.1",
    "morgan": "~1.5.1",
    "serve-favicon": "~2.2.0",
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

> ( MacOS ou Linux)
```sh

$ DEBUG=nodetest1 ./bin/www

```

> (Windows)
```sh

> set DEBUG=myapp & node .\bin\www

```

Aperte enter. E o cursor vai ficar piscando no canto do console.


Incrível! Abra seu navegador e digite `http://locahost:3000`. Agora você verá a página de boas vindas do Express.

![boas vindas express](http://cwbuecheler.com/web/tutorials/2013/node-express-mongo/browsershot1.png)

Você tem agora seu próprio servidor web com Node.js, com a *engine* Express e o pré-processador Jade instalados. Não é tão difícil, né?

## PARTE 2 - OK. LEGAL. VAMOS FAZER O "HELLO WORLD!"

Abra seu editor de texto ou IDE favorita. Eu gosto muito do [Sublime Text](http://www.sublimetext.com/). Vá para o diretório `nodetest1` e abra o arquivo `app.js`. Esse é como o coração da sua app. 

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

Em razão de evitar alguns avisos em seu console Node quando você rodar a aplicação. Isto é devido a algumas mudanças futuras do Express e seus plugins. Se você não fizer esta mudança, sua aplicação vai continuar rodando, mas você irá ver texto sobre futuras *desaprovações* (deprecations) toda vez que você rodar isso.

`app.js`
```js

// development error handler
// will print stacktrace
if (app.get('env') === 'development') {
    app.use(function(err, req, res, next) {
        res.status(err.status || 500);
        res.render('error', {
            message: err.message,
            error: err
        });
    });
}

```

Isso permite que você faça alguma checagem de erro durante o desenvolvimento. É importante, mas pela proposta deste tutorial não vamos fazer nada com isso.

`app.js`
```js

app.use( '/', routes );
app.use( '/users', user );

```

Isso diz a app quais rotas usar quando uma URI particular é solicitada. Note que a variável "user" está declarada acima, e é mapeada para `/routes/user.js`. 

Agora então, vamos fazer algumas coisas. Não vamos fazer apenas um "Hello, World!" na nossa página index. Ao invés disso, vamos usar essa oportunidade para aprender um pouco mais sobre rotas e ver como o Jade trabalha para colocar as páginas em conjunto. Primeiro, vamos adicionar uma linha para manipular uma nova URI. Em baixo da seção `router.get();` no arquivo `nodetest1/routes/index.js`, adicione esta linha:

```js

router.get('/helloworld', function(req, res, next){
  res.render('helloworld', {title: 'Hello Word'});
});

```

Isso é tudo que temos que fazer para rotear esta URI, mas nós não temos nenhuma página para o `res.render` renderizar. É ai que o Jade entra. Abra sua pasta `views`, e então abra o arquivo `index.jade`. Antes de fazer qualquer coisa, **salve este arquivo como `helloworld.jade`**.

Agora dê uma olhada no código:

`helloworld.jade`
```jade

extends layout

block content
	h1= title
	p Welcome to #{title}

```

Isso é muito simples. Ele usa o `extends` e faz o arquivo `layoud.jade` como um template, e então dentro do bloco `content` definido no arquivo layout, ele altera o `header` e o `p` (parágrafo). Note o uso da variável `title` que configuramos acima, em nossa rota index.js. Isso significa que não temos que mudar sempre o texto para mostrar coisas diferentes na página home. Mas vamos mudar um pouco de qualquer forma para:

```jade

p Hello, World! Welcome to #{title}

```

Salve o arquivo, vá para o terminal e encerre sua aplicação `ctrl c`. Agora digite (iniciar o servidor):

```sh

$ DEBUG=nodetest1 ./bin/www

```

É importante mencionar: mudanças nos templates Jade não necessitam do reinício do servidor, mas basicamente toda vez que você mudar um arquivo `.js`, como `app.js` ou um arquivo de rota, você vai precisar reiniciar para ver as mudanças.

Agora, com o servidor reiniciado, navegue até `http://localhost:3000/helloworld` e divirta-se com o texto completamente estúpido mostrado:

![helloworld](http://cwbuecheler.com/web/tutorials/2013/node-express-mongo/browsershot2.png)

Ok! Agora temos nossa rota nos levando para nossa view. Vamos fazer alguma modelagem. **Eu vou dar um momento para você, caso precise reparar seu cabelo ou maquiagem**.

> Nota do tradutor: (Y) AUSHDUHASUDHUASHDUAHSUDHAUSHDUHASUDHUASHD

## PARTE 3 - CRIANDO NOSSO DB E LENDO ALGO DELE

### PASSO 1 - INSTALAR MONGODB

Vamos deixar um pouco nosso editor de texto e ir para nosso terminal. Bem, primeiro vamos para nosso browser, no endereço http://mongodb.org/ e fazer o download do Mongo. Click no link de downloads no menu principal e pegue a versão de produção que se encaixa com seu sistema. Para o Windows 8 com um processador 64-bit, nós vamos usar o "64-bit *2008R2+". Isso irá lhe fornecer um arquivo `.zip`, que você deve descompactar para um diretório temporário. Então você pode criar um diretório no qual o Mongo vai permanecer pra sempre depois de armazenar o Mongo. Você pode usar `c:\mongo` ou `c:\program files\mongo` ou qualquer outra coisa louca que você quiser. Isso não importa na verdade - O Mongo é bem pequeno, e vamos armazenar nosso banco de dados no nosso diretório `nodetest1`.

De qualquer forma, copie os arquivos da pasta bin dentro do seu diretório temporário para onde você quer que o Mongo fique, e você está pronto. Você instalou o Mongo. Agora vamos fazer isso funcionar.

### PASSO 2 - RODANDO MONGOD e MONGO

No seu diretório nodetest1, cria um subdiretório chamado `data`. Então navegue até o diretório em que você colocou seus arquivos do MongoDB. Deste diretório, digite o seguinte:

```sh

mongod --dbpath c:\node\nodetest1\data

```

Você vai ver que o servidor Mongo inicia. Pode demorar um pouco se for a primeira vez, porque ele tem que fazer algumas pre-alocações de espaço e algumas tarefas de limpeza. Uma vez que isso disser "[initandlisten] waiting for connections on port 27017", tudo está feito. Não há nada mais para se fazer; o servidor está rodando. Agora você pode **abrir um segundo terminal**. Navegue novamente até o diretório de instalação do Mongo, e digite:

```sh

mongo

```

Você vai ver algo assim:

```sh

c:\mongo>mongo
MongoDB shell version: 2.4.5
connecting to: test

```

Adicionalmente, se você está prestando atenção em sua instância mongod, você vai ver que ele menciona que a conexão foi estabilizada. Tudo certo, você tem o MongoDB funcionando, e conectou a ele com o client. Nós vamos usar o client manualmente para trabalhar no nosso banco de dados, mas não é necessário para rodar o website. Somente o mongod é necessário para isso.

### PASSO 3 - CRIANDO UM BANCO DE DADOS

Não se preocupe com "connecting to: test"... este é apenas o db padrão decidido pelo MongoDB para ser usado se você não especificar um na linha de comando, o qual não fizemos porque não é importante por agora. Na verdade, ele nem mesmo cria o db "test", a menos que você adicione um registro. Seria totalmente correto trabalhar neste db por agora, mas vamos criar um próprio nós mesmos. No seu console Mongo, digite o seguinte:

```sh

use nodetest1

```

Agora estamos usando o db `nodetest1`. Igualmente ao `test`, nada existe ainda. Para criar o db, temos que adicionar algum dado. Vamos começar fazendo isso diretamente pelo Mongo Client.

### PASSO 4 - ADICIONANDO ALGUNS DADOS

O que mais gosto sobre o MongoDB é que ele usa JSON em sua estrutura, que significa que isso foi instantâneamente familiar para mim. Se você não está acostumando com JSON, você terá que fazer alguma leitura, pois isso está fora do escopo deste tutorial.

Vamos adicionar um registro para nossa coleção. Para a proposta deste tutorial, vamos apenas ter um simples db com nomes de usuários e endereços de e-mail. O formato de nossos dados vão ser dessa forma:

```json
{
  "_id" : 1234,
  "username" : "cwbuecheler",
  "email" : "cwbuecheler@nospam.com"
}

```

Você pode criar sua própria atribuição `_id` se você realmente quiser, mas eu acho melhor deixar para o Mongo fazer estas coisas. Isso vai fornecer um identificador único para cada simples entrada do nível superior da sua coleção. Vamos adicionar uma e ver como isso funciona. No seu Mongo client, digite isso:

```sh

db.usercollection.insert({ "username" : "testuser1", "email" : "testuser1@tesdomain.com" })

```

Algo importante de se notar aqui: este `db` significa nosso banco de dados, que como mencionado acima, nós definimos como `nodetest1`. A parte `usercollection` é nossa coleção. Note que não existe um passo onde nós criamos a coleção "usercollection". Isso porque a primeira vez que adicionamos isso, ele já irá se auto-criar. Prático. Ok, aperte enter. Assumingo que tudo ocorreu corretamente, você deve ver... nada. Isso não é muito animador, então digite isso:

```sh

db.usercollection.find().pretty()

```

No caso de você estar curioso, o método `.pretty()` nos fornece quebra de linha. Isso vai retornar:

```sh

{
    "_id" : ObjectId("5202b481d2184d390cbf6eca"),
    "username" : "testuser1",
    "email" : "testuser1@testdomain.com"
}

```
Exceto, claro, que seu ObjectID vai ser diferene deste mencionado, pois o Mongo vai gerará-lo automaticamente. Isto é tudo que temos que escrever para o MongoDB a partir do client app, e se você já trabalhou com serviços JSON antes, você provavelmente estará pensando "ó, wow, isso será fácil de implementar na web." ... você está certo!

Uma nota rápida sobre a estrutura do DB: obviamente ao longo da jornada você não vai armazenar nada em nível alto. Existem toneladas de recursos na internate para o projetos de esquema para o MongoDB. Google é seu amigo!

Agora que temos um registro, vamos adicionar um pouco mais. Em seu console Mongo, digite o seguinte:

```sh

newstuff = [{ "username" : "testuser2", "email" : "testuser2@testdomain.com" }, { "username" : "testuser3", "email" : "testuser3@testdomain.com" }]
db.usercollection.insert(newstuff);

```

Note que, sim, nós passamos um array com múltiplos objetos para nossa coleção. Prático! Outro uso de `db.usercollection.find().pretty()` vai mostrar todos os três registros:

```sh

{
        "_id" : ObjectId("5202b481d2184d390cbf6eca"),
        "username" : "testuser1",
        "email" : "testuser1@testdomain.com"
}
{
        "_id" : ObjectId("5202b49ad2184d390cbf6ecb"),
        "username" : "testuser2",
        "email" : "testuser2@testdomain.com"
}
{
        "_id" : ObjectId("5202b49ad2184d390cbf6ecc"),
        "username" : "testuser3",
        "email" : "testuser3@testdomain.com"
}

```

Agora, vamos começar realmente a interagir com o servidor web e o site que configuramos anteiormente.

### PASSO 5 - LIGANDO O MONGO COM O NODE

Aqui é onde a borracha encontra o asfalto. Vamos começar a criar uma página que apenas mostra nossas entradas no DB de forma bem ligeira. Aqui o HTML que vamos gerar:

```html

<ul>
    <li><a href="mailto:testuser1@testdomain.com">testuser1</a></li>
    <li><a href="mailto:testuser2@testdomain.com">testuser2</a></li>
    <li><a href="mailto:testuser3@testdomain.com">testuser3</a></li>
</ul>

```

Eu sei que isso não é ciência astronáutica, mas esta é a questão. Vamos fazer apenas um simples ler-e-escrever do DB neste tutorial, não tentar fazer um website inteiro. Primeiramentem precisamos adicionar algumas linhas no nosso arquivo principal `app.js` - o coração e a alma da nossa app - em favor de realmente nos conectar-mos a instância MongoDB. Abra o arquivo `app.js` e no topo dele você vai ver:

```js

var express = require('express');
var routes = require('./routes');
var user = require('./routes/user');
var http = require('http');
var path = require('path');

```

Agora adicione estas 3 linhas:

```js

var express = require('express');
var routes = require('./routes');
var user = require('./routes/user');
var http = require('http');
var path = require('path');

// Novo código
var mongo = require('mongodb');
var monk = require('monk');
var db = monk('localhost:27017/nodetest1');

```

Estas linhas dizem que nossa app vai conversar com o MongoDB, e vamos usar o Monk para fazer isso, e nosso banco de dados está localizado em `localhost:27017/nodetest1`. Note que 27017 é a porta que sua instância MongoDB deve estar rodando. Se por algum motivo você a mudou, obviamente use esta porta então. Agora olhe para a parte de baixo do arquivo, onde você tem isso:

```js

app.get('/', routes.index);
app.get('/users', user.list);
app.get('/helloworld', routes.helloworld);

```

Adicione a seguinte linha no final:

```js

app.get('/userlist', routes.userlist(db));

```

Esta linha diz que quando o usuário navegar para /userlist, nós vamos passar a variável "db" (nosso objeto do banco de dados) para a rota *userlist*. Mas nós NÃO temos uma rota userlist ainda, então vamos criar uma.

### PASSO 6 - PUXANDO DADOS DO MONGO E MOSTRANDO-OS

Abra o arquivo `nodetest1/routes/index.js` em seu editor. Ele tem a rota index, e a rota /helloworld. Vamos adicionar uma terceira:

```js

exports.userlist = function(db) {
    return function(req, res) {
        var collection = db.get('usercollection');
        collection.find({},{},function(e, docs){
            res.render('userlist', {
                "userlist" : docs
            });
        });
    };
};

```

Ok... Isso está ficando bem complicado. Tudo que isso está realmente fazendo, porém, é rodar uma função que envolve para passarmos nossa variável db, e então fazer a página renderizar como os outros dois "exports" neste arquivo de rota. Nós então dizemos em cada coleção que queremos usar ('usercollection') e fazer um `find`, então retornando o resultado como a variável `docs`. Uma vez que temos estes documentos, nós então vamos renderizar uma *userlist* (que vai precisar de um template Jade correspondente), dando isso a esta userlist para que ela possa trabalhar, e passando nosso documento do db como variável.

Vamos agora configurar nosso template Jade. Navegue até `nodetest1/views` e abra `index.jade`. Após isso, **salve imediatamente este arquivo como `userlist.jade`**. Então edite o HTML para se parecer com isso:

```jade

extends layout

block content
    h1.
        User List

    ul
        each user, i in userlist
            li
                a(href='mailto:#{user.email}')= user.username

```

Isto está dizendo que vamos receber um conjunto de documentos chamado de userlist do nosso arquivo roteador, e então para entrada (nomeado 'user' durante o loop), vamos pegar o valor 'email' e 'username' do objeto e colocar em nosso html. Nós também temos a contagem - i - útil, mas neste caso nós não precisamos dela.

Tudo está configurado. Salve o arquivo, e vamos reiniciar nosso servidor node. Se lembra de como fazer isso? Vá para o terminal e aperte `ctrl c` para encerrar o processo de `app.js` se ele ainda estiver rodando. Então digite:

```js

$ node app.js

```

Agora abra o seu navegador e vá para `http://localhost:3000/userlist` e maravilhe-se com o resultado.

![hello user](http://cwbuecheler.com/web/tutorials/2013/node-express-mongo/browsershot3.png)

Você está agora puxando dados do DB e mostrando na sua página web. Muito bom!

Há mais uma coisa que eu gostaria muito de cobrir neste tutorial, mas como ele já está tão longo quanto a Bíblia, vou explicar brevemente isso aqui. Você pode facilmente mudar sua *view* userlist de uma página manipulada pelo Express e template Jade para uma boa e velha resposta JSON. Você pode então acessar isso com AJAX e manipular no lado do cliente, com jQuery por exemplo, ao invés de no lado do servidor. Eu não vou me opor se você quiser fazer assim, mas não posso cobrir isso, então vou apenas apontar o caminho para [res.json](http://expressjs.com/api.html#res.json) e dizer "siga por aqui, não é tão difícil".

Vamos acabar com isso.

## PARTE 4 - O SANTO GRAAL - ESCREVENDO NO DB

Escrever no banco de dados não é difícil. Essencialmente nós precisamos configurar uma rota que pega um POST, ao invés de um GET. Nós podemos usar AJAX aqui, e honestamente é minha preferência na maioria das vezes... mas este é realmente um tutorial diferente, então vamos manter uma abordagem de *colocar e mostrar resultados*. Mais uma vez, não tão difícil adaptar essas coisas para funcionarem da maneira que você quer.

### PASSO 1 - CRIE SUA ENTRADA DE DADOS

Vamos passar rapidamente aqui: dois inputs feios e sem estilo mais um botão *submit*. Estilo 1996. Após isso, vamos começar com o `app.get()`; e então dar algo para ser pego. Abra o `app.js` e encontre a parte das chamadas `app.get()`, e adicione isso no final delas:

```js

app.get('/newuser', routes.newuser);

```

Então você vai ter:

```js

app.get('/', routes.index);
app.get('/users', user.list);
app.get('/helloworld', routes.helloworld);
app.get('/userlist', routes.userlist(db));

// Novo código
app.get('/newuser', routes.newuser);

```

Como todas as requisições `app.get`, nós precisamos ajustar a rota para reconhecer o que servir. Abra `routes/index.js` e adicione o seguinte:

```js

exports.newuser = function ( req, res ) {
	 res.render( 'newuser', { title: 'Add New User' } );
};

```

Agora nós apenas precisamos de um template. Abra `views/index.jade`, salve como `newuser.jade`, e substitua todo o arquivo com este conteúdo:

```jade

extends layout

block content
    h1= title
	      form#formAddUser( name='adduser', method='post', action='/adduser' )
		    input#inputUserName( type='text', placeholder='username', name='username' )
		    input#inputUserEmail( type='text', placeholder='useremail', name='useremail' )
		    button#btnSubmit( type='submit' ) submit

```

Aqui nós criamos um formulário com o ID "formAddUser" (eu gosto de nomear meus IDs com o tipo de coisa que ele está identificando. É uma peculiaridade pessoal). O *method* é o `post` e a *action* é `adduser`. Bastante simples. Abaixo disso nós definimos nossos dois inputs e nosso botão.

Se você reiniciar o servidor node e ir para `http://localhost:3000/newuser`, você vai ver nosso formulário em toda sua glória.

![form](http://cwbuecheler.com/web/tutorials/2013/node-express-mongo/browsershot4.png)

### PASSO 2 - CRIANDO NOSSAS FUNÇÕES DB

Ok, o mesmo processo de antes. Primeiro vamos editar o `app.js`, então nosso arquivo `route`, e então nosso template Jade. Exceto que não existe um template Jade aqui porque nós estamos postando e então encaminhando. Veja abaixo. Vai tudo fazer sentido! Vamos começar: Abra `app.js` e mais uma vez encontre a pilha de chamadas `app.get`:

```js

app.get('/', routes.index);
app.get('/users', user.list);
app.get('/helloworld', routes.helloworld);
app.get('/userlist', routes.userlist(db));
app.get('/newuser', routes.newuser);

```

Agora adicione o seguinte em baixo desta lista:

```js

app.post('/adduser', routes.adduser(db));

```

Note que isso é um `app.post`, não um `app.get`. Se você quer separar essa parte dos `app.get` com um comentário ou nova linha, eu não vou lhe impedir. Vamos configurar nossa rota.

Volte para `routes/index.js` para criarmos nossa função de inserção. Essa é grande, então eu comentei o código bem cuidadosamente. Aqui está:

```js

exports.adduser = function (db) {
    return function (req, res) {
       
        // Pega os valores do form. Eles dependem do atributo "name"
        var userName = req.body.username;
        var userEmail = req.body.useremail;

        // Configura nossa coleção
        var collection = db.get('usercollection');

        // Envia ao DB
        collection.insert({
            "username" : userName,
            "email" : userEmail
        }, function (err, doc) {
            if (err) {
                // Se isso falhar, retorna um erro
                res.send("Ocorreu um problema ao adicionar informação ao banco de dados");
            }
            else {
                // Se funcionar, configura o header para a barra de endereço não continuar dizendo /adduser
                res.location("userlist");
                // E depois a página de sucesso
                res.redirect("userlist");
            }
        }); 
    };
};

```

Obviamente no mundo real você vai querer de uma tonelada a mais de validação, checagem de erros, e coisas do tipo. Você vai querer checar por nomes de usuários e emails duplicados, por exemplo, e também checar se a entrada de email parece com uma legítima. Mas isso vai funcionar por agora. Como você pode ver, adicionando os dados com sucesso ao DB, vamos em seguida retornar o usuário a página *userlist*, onde ele deve ver a nova entrada adicionada.

Existem formas mais suvaes de se fazer isso? Com certeza, porém vamos ficar nessa forma básica por agora. Agora, vamos adicionar alguns dados!

### PASSO 3 - CONECTANDO E ADICIONANDO DADOS AO SEU BANCO DE DADOS

**Assegure-se que o mongod está rodando!** Então volte para seu terminal, encerre o processo do servidor node e volte a rodá-lo, reiniciando-o:

```sh

$ node app.js

```

Assumindo que seu servidor está rodando, e deve estar, retorne para o navegador e vá para `http://localhost:3000/newuser` novamente. Temos nosso empolgante formulário, exatamente como antes, exceto que agora vamos preencher com alguns valores antes de enviarmos ele. Eu coloquei o *username* como "noderocks" e o *email* como "noderocks@rockingnode.com"... você pode colocar o que quiser.

![add user](http://cwbuecheler.com/web/tutorials/2013/node-express-mongo/browsershot5.png)

Clique em submit, e veja... voltamos ao `/userlist` e essa é nossa nova entrada!

![new add user](http://cwbuecheler.com/web/tutorials/2013/node-express-mongo/browsershot6.png)

Estamos oficialmente lendo e escrevendo a partir do nosso banco de dados MongoDB usando Node.js, Express e Jade. Você é agora o que as crianças chamam de desenvolvedor "full stack" (provavelmente não um BOM, por enquanto, mas eu não prometi isso).

Parabéns. Sério. Se você seguiu todo este caminho, e se realmente prestou atenção no que você fez e não apenas copiou e colou código, você deve ter agora uma compreensão sólida de rotas e views, ler do banco de dados e postar no DB. Isso é **tudo** que você precisa para começar a desenvolver qualquer app que você queira. Eu não sei você, mas acho isso realmente muito legal.

## PARTE 5 - PRÓXIMOS PASSOS

A partir daqui, existem milhões de diferentes direções que você pode seguir. Você pode checar sobre [Mongoose](http://mongoosejs.com/), que é outro pacote de manipulador Mongo para Node/Express. É maior que o Monk, mas também faz mais coisas. Você pode checar sobre Stylus, o pré-processador CSS que vem com o Express. Você pode pesquisar "Node Express Mongo Tutorial" e ver algumas coisas. Apenas continue explorando e construindo!

Eu espero que este tutorial tenha sido útil. Eu o escrevi porque eu poderia ter usado isso quando comecei, e não consegui encontrar algo parecido com este nível, ou tenha quebrado as coisas em longos, longos, looongos detalhes. Se você chegou até aqui, obrigado por isso!

---


**Veja o fim do artigo original para ler a lista de agradecimentos e indicações. [Link](http://cwbuecheler.com/web/tutorials/2013/node-express-mongo/)**
