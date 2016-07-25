# Sua Linha do Tempo para Aprender React

* **Artigo original**: [Your Timeline for Learning React](https://daveceddia.com/timeline-for-learning-react/)
* **Tradução**: [Eric Douglas](https://github.com/ericdouglas)

Esta é sua linha do tempo para aprender React.

Pense nestes passos como camadas em uma fundação.

Se você estivesse construindo uma casa, você pularia alguns passos para ter a casa concluída mais rapidamente? Talvez pular direto para o concreto antes de colocar algumas pedras no lugar? Você construiria as paredes no chão puro?

Ou sobre fazer um bolo de casamento: a parte de cima parece a mais legal de decorar, então por que não começar por ai? Vamos pensar sobre a parte de baixo depois.

Não?

Claro que não. Você sabe que tais coisas o levariam ao fracasso.

Então por que você abordaria React tentando aprender ES6 + Webpack + Babel + React + Redux + Routing + AJAX *tudo de uma vez*? Isso não soa como se você estivesse se levando para o fracasso?

Um pé na frente do outro. Reserve um pouco de tempo a cada dia, isso é provavelmente algo para algumas semanas de aprendizado.

O tema deste artigo é: evite ficar sobrecarregado. Lento e constante, assim se aprende React.

## Passo 0: JavaScript

Assumo que você já sabe JavaScript, pelo menos ES5. Se você ainda não sabe JS, pare o que está fazendo, aprenda o [básico](https://developer.mozilla.org/pt-BR/docs/Aprender/Getting_started_with_the_web/JavaScript_basico), e *só depois* continue em frente.

Se você já conhece ES6, vá em frente e use-o. Só para você saber, a API do React tem algumas diferenças entre ES5 e ES6. É útil saber ambos os "sabores" - quando algo dá errado, você vai encontrar uma ampla gama de respostas úteis se você conseguir traduzir mentalmente entre ambos os estilos.

## Passo 0.5: npm

npm é o gerenciador de pacotes dominante no mundo JavaScript. Não há muita coisa para se aprender aqui. Depois de [instalá-lo junto com Node.js](https://nodejs.org/en/), tudo que você realmente precisa saber é como instalar pacotes (`npm install <nome do pacote>`).

## Passo 1: React

Comece com o [Olá Mundo](https://daveceddia.com/test-drive-react). Use um arquivo HTML em branco com algumas tags `<script>` como no [tutorial oficial](https://facebook.github.io/react/docs/tutorial.html) ou use uma ferramenta como React Heatpack para ter as configurações básicas de forma rápida.

> Nota do tradutor: agora você pode iniciar um projeto React usando [create-react-app](https://github.com/facebookincubator/create-react-app), que é a mais nova ferramenta oficial do Facebook para iniciar projetos React sem a necessidade de nenhum tipo de configuração inicial.

## Passo 2: Crie algumas coisas, e jogue-as fora

Este é o embaraçoso passo do meio que várias pessoas pulam.

Não cometa esse erro. Seguir em frente sem ter forte domínio dos conceitos do React vai levá-lo para a "terra dos sobrecarregados".

Mas esse passo não é muito bem definido: o que devo construir? Um protótipo para o trabalho? Talvez um clone do Facebook, algo realmente substancial para realmente se acostumar com todas as novas ferramentas?

Bem, não, não essas coisas. Ou seria algo muito simples ou muito grande para um projeto de aprendizagem.

"Protótipos" para o trabalho são especialmente terríveis, pois *você sabe absolutamente* em seu coração que um "protótipo" não vai ser nada do tipo. Isso vai viver muito depois da fase de prototipagem, se fundir com o software lançado, e nunca vai ser jogado fora ou reescrito.

Usando um "protótipo" para o trabalho como um projeto de aprendizagem é problemático pois você começara a pensar sobre o *futuro*. Você sabe que isso vai ser mais do que apenas um protótipo, e ai você se preocupa - isso não deveria ter testes? Tenho que assegurar que a arquitetura vai escalar... Será que terei que refatorar essa confusão mais tarde? E isso não deveria ter testes?

Esse problema específico é o que eu quero enfrentar com meu livro *[Learn Pure React](https://daveceddia.com/learn-pure-react/)* (Aprenda React Puro).

Vou te dar uma ideia: projetos ideais estão entre um "Olá Mundo" e um "Twitter completo".

Crie algumas listas de coisas (lista de afazeres, cervejas, filmes). Aprenda como o fluxo de dados acontece.

Pegue uma interface de usuário grande (Twitter, Reddit, Hacker News, etc) e quebre-a em um pequeno pedaço para reconstruir - divida-a em componentes, construa as partes e renderize-as com dados estáticos.

Você pegou a ideia: pequenos aplicativos descartáveis. Eles **devem ser descartáveis**, caso contrário você vai ficar preso em manutenção e arquitetura, ou algum outro problema que simplesmente não importa por agora.

## Passo 3: Webpack

Ferramentas de build são um grande obstáculo. Configurar o Webpack parece *trabalho pesado*, e é uma mentalidade totalmente diferente da usada para escrever código de interface de usuário.

Eu recomendo [Webpack — The Confusing Parts](https://medium.com/@rajaraodv/webpack-the-confusing-parts-58712f8fcad9#.tbe59m7ep) como uma introdução ao Webpack e sua forma de pensar.

Uma vez entendido o que ele faz (empacota *todo tipo de arquivo*, não apenas JavaScript) - e como ele funciona (*loaders* para cada tipo de arquivo), a parte Webpack da sua vida vai ser muito mais feliz.

## Passo 4: ES6

Agora que você está no passo 4, você tem todos os passos acima como *contexto*. As partes do ES6 que você vai aprender agora vão ajudá-lo a escrever um código mais limpo, melhor e mais rápido. Se você tentasse memorizar tudo isso de início, você não iria reter - mas agora, você sabe como tudo isso se encaixa.

Aprenda as partes que você usa mais: *arrow functions*, `let` e `const`, `class`, *destructuring* e `import`.

## Passo 5: Roteamento

Algumas pessoas confundem *React Router* e *Redux* em suas cabeças - eles não estão relacionados ou são dependentes um do outro. Você pode (e deve!) aprender a usar o React Router antes de se aprofundar no Redux.

Nesse ponto você vai ter uma base sólida em "pensar da forma React", e a abordagem baseada em componentes do React Router vai fazer mais sentido do que se você tivesse tentado aprendê-lo no primeiro dia de estudo.

## Passo 6: Redux

Dan Abramov, o criador do Redux, [diz para você](https://github.com/gaearon/react-makes-you-sad) não adicionar Redux muito cedo, e por uma boa razão - é uma dose de complexidade que pode ser desastrosa no começo.

Os conceitos por atrás do Redux são muito simples. Mas existe um salto mental entre entender as partes e saber como aplicá-las em uma aplicação.

Dessa forma, repita o que você fez no passo 2: faça aplicativos descartáveis. Construa vários pequenos experimentos com Redux para de fato internalizar como ele funciona.

## Não faça

Você viu em algum lugar da lista "escolha um projeto padrão (boilerplate)"? **Não**.

Se aprofundar em React escolhendo um dos milhões de projetos boilerplate existentes por ai vai apenas te confundir. Eles incluem cada biblioteca possível, e força uma estrutura de diretórios sobre você - e nada disso é um requisito para aplicações pequenas, ou quando você está começando.

E se você está pensando "Mas Dave, eu não estou fazendo uma aplicação pequena, eu estou construindo uma aplicação complexa que vai servir milhões de usuários!"... volte e releia a parte sobre protótipos.

## Como encarar essa jornada

É muito para assimilar. É bastante coisa para aprender - mas existe uma progressão lógica. Um pé na frente do outro.
