# Uma introdução ao MongoDB

O MongoDB se declarou a principal referência em base de dados NoSQL. Com bases em suas estatísticas é dificil argumentar que a tecnologia não é popular.

MongoDB vem sido usado no prestigiado 'MEAN' stack (referência a utlização das tecnologias de desenvolvimento: MongoDB, Angular, Express e Nodejs). Os benefícios de se utilizar uma base de dados documental como esta significa que é possível trabalhar com documentos (registros) com sintaxe semelhante ao JSON em toda a estrutura de um aplicativo.

É possível trabalhar com JSON no frontend (Angular), backend (Node) e na base de dados (MongoDB).

Para sabe mais benefícios de se trabalhar com o MongoDB, visite o site [oficial](http://mongodb.org/). Agora vamos prosseguir e entrar em detalhes de como instalar e user o  MongoDB.   

##Instalando e utilizando o MongoDB

É possível intalar o MongoDB em seu computador seguindo alguns passos simples:

1. Realizar os procedimentos de instalação 
2. Criar um diretório ƒisico onde a base de dados será armazenada

E ,simples assim, nós podermos usar o MongoDB localmente!

As instruções a seguir são para Mac, o site oficial possui uma documentação ótima para instalação em [Windows](http://docs.mongodb.org/manual/tutorial/install-mongodb-on-windows/) e [Linux](http://docs.mongodb.org/manual/tutorial/install-mongodb-on-ubuntu/).

A pasta de trabalho padrão na instalação será /data/db. Será nesta pasta que tudo sera armazenado, por isso você precisa ter certeza que possui permissão para utilizar este diretório para que o MongoDB possa gravar dados nele.

Nós podemos instalar no Mac manualmente ou usando o Homebrew. Neste artigo iremos focar no Homebrew.

```sh

//Atualize todos os seus packages

$ brew update 

// Instale o MongoDB

$ brew install mongodb

```
Se voce preferir não usar o Homebrew, é só seguir as [instruções](http://docs.mongodb.org/manual/tutorial/install-mongodb-on-os-x/#install-mongodb-manually) no site oficial. 

## Começando a utilizar o MongoDB

Existem dois passos que devem ser seguidos para se utilizar o MongoDB localmente:


1. Começar o processo (Iniciar o MongoDB)
2. Conectar

Parece bem simples, não é? Vamos começar!


### Iniciando uma instância do MongoDB

Antes de começarmos a criar e salvar registros em nossa base de dados, nós precisamos iniciar uma instância do MongoDB. Não conseguiremos salvar nada se não iniciarmos esta instância (ou serviço). Basta um comando bem simples para inicia-la:

```sh

$ mongod

```
Assim que entrarmos com este comando, o MongoDB irá ser iniciado e você deve ver uma mensagem: 'waiting for connections on port 27017' (Esperando por conexões na porta 27017).

![Inicio do MongoDB](https://scotch.io/wp-content/uploads/2014/11/mongo-start.jpg)


### Conectando a uma instância do MongoDB

Com nossa instância do MongoDB rodando, agora só precisamos nos conectar a ela. Enquanto  o comando 'mongod' inicia uma instância do MongoDB, o comando 'mongo' se conecta a ela.
With our MongoDB service running, we just have to connect to it. While mongod starts MongoDB, the command to connect to it is:
```sh
$ mongo

```sh
Agora estamos conectados e podemos enviar commandos ao MongoDB.

![Inicio do MongoDB](https://scotch.io/wp-content/uploads/2014/11/mongo-connect.jpg)

Note que no lado direito, estamos conectados ('connecting to test') e no lado esquerdo nossa conexão foi logada ('1 connection new open'). 

## Comandos de Base de Dados Comuns

Listar todas as Base de Dados

```sh
$ show dbs

```
### Criando uma base de dados

O MongoDB não irá criar uma base de dados até que você insira alguma informação na base de dados.

O truque é que você não precisa se preocupar em criar uma base de dados explicitamente! Você pode simplesmente usar a base de dados (mesmo que não a tenha criado ainda), cirar uma coleção e um document (registro) e tudo será criado automáticamente para você!


Iremos analisar a criação de coleções e documentos na seção de comandos para CRUD (Criar, Ler, Atualizar e Deletar).


Mostrar base de dados atual
```sh

$ db

```
Selecionar uma base de dados


```sh

$ use db_name

```

## Comandos para CRUD (Criar, Ler, Atualizar e Deletar)

Criar
```sh

// Salvar um usuário

$ db.users.save({ name: 'Alberto' });

// Salvar vários usuários 

$ db.users.save({ name: 'Alberto'}, { name: 'Maria' });

```
Ao salvar um documento (registro) na coleção de usuários na base de dados que está sendo utilizada, você criou, com sucesso, tanto a base de dados quanto a coleção (se estas ainda não existiam).

Ler

```sh
// Mostrar todos os usuários

$ db.users.find();

// Encontrar um usuário específico

$ db.users.find({ name: 'Maria' });

```
Atualizar
```sh
db.users.update({ name: 'Maria' }, { name: 'Maria da Graça' });
```
Deletar
```sh
// Remover todos
db.users.remove({});

// Remover um
db.users.remove({ name: 'Maria' });

```
Este é apenas uma visão geral dos tipos de comandos que podem ser feitos. A [documentação](http://docs.mongodb.org/manual/core/crud-introduction/) do MongoDB é bem extensa, o que a torna um ótimo recurso para aqueles que queiram se aprofundar.

O MongoDB também possui um ótimo [tutorial](http://try.mongodb.org/) interativo para guia-lo na execução dos comandos acima.

## Usando MongoDB com Node.js

Utilizar o MongoDB local com Node é bem simples. Nós utilizaremos o 'mongooseJS', um package do Node para trabalhar com o MongoDB.

Tudo que você precisa fazer é configurar o mongoose para se conectar a base de dados local. Isso é um processo simples já que nem precisamos de cria a base de dados. Só precisamos nos certificar que a instância do MongoDB foi inicada:

```sh

$ mongod

```
Nós também daremos ao mongoose um nome para nossa base de dados a ser conectada (ela não precisa ser criada antes já que o mongoose irá criá-la para nós).

Segue uma amostra de código de como criar uma base de dados no Node.

```js

// Requere os packages que precisamos

var mongoose = require('mongoose');

mongoose.connect('mongodb://localhost/nome_base_de_dados');

É isso! A partir do momento em que começarmos a salvar itens em nossa base de dados, esta será criada com o nome de 'nome_base_de_dados'

Para mais informações de como usar o Mongoose com o Node, leia: [Using MongooseJS in Node.js and MongoDB Applications](https://scotch.io/bar-talk/using-mongoosejs-in-node-js-and-mongodb-applications)


## Ferramenta GUI: Robomongo

Mesmo que seja fácil de usar o terminal para acessar nossas base de dados MongoDB, exite também uma GUI para aqueles que gostem deste tipo de interface.

Faça o download do [Robomongo](http://robomongo.org/) e o inicie.

Criar uma conexão para nossa base de dados é bem fácil. Basta colocar o endereço como 'localhost' e a porta em 27017. Nomeie sua conexão como quiser.
https://scotch.io/wp-content/uploads/2014/11/robomongo-connect-local.jpg
robomongo-connect-local
![Robomongo](https://scotch.io/wp-content/uploads/2014/11/robomongo-connect-local.jpg)

Uma vez que você conectar, terá acesso a todas as bases de dados e suas respectivas coleções! Agora você tem uma GUI para lidar com operações de base de dados. É possível até abrir o terminal (shell) e utilizar os comandos discutidos acima.

![Robomongo Terminal](https://scotch.io/wp-content/uploads/2014/11/robomongo-local-db.jpg)

## Conclusão

O MongoDB é uma ótima base de dados NoSQL que pode ser configurada rápidamente e usada em qualquer dos seus aplicativos. Em aplicativos que usam Node, é possível iniciar rápidamente e chegar na parte mais divertida do desenvolvimento, a construção do aplicativo!

