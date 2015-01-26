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

