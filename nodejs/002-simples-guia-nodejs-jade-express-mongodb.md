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

## Parte 1 - 15 minutos de instalação

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