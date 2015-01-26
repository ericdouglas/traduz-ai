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





