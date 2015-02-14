# Aprenda Tudo sobre Handlebars.js Templating JavaScript

* **Artigo Original**: [Handlebars.js Tutorial: Learn Everything About Handlebars.js JavaScript Templating](http://javascriptissexy.com/handlebars-js-tutorial-learn-everything-about-handlebars-js-javascript-templating/)
* **Tradução**: [Eric Douglas](https://github.com/ericdouglas)

**Este é um tutorial sobre Handlebars.js e uma referência ao Handlebars.js** <br><br>

Este é um tutorial completo, e de fato uma referência, sobre templates Handlebars.js e, principalmente, templates JavaScript. Handlebars.js é um motor de templates no lado do cliente (pode ser usado no servidor também). É uma biblioteca JavaScript que você inclui em sua página da mesma maneira que você inclui qualquer arquivo .js. E com isso, você pode adicionar templates para sua página HTML que vão ser parseados e interpolados (valores de propriedades inseridos no lugar) com valores dos dados que você passou na função Handlebars.js.

Como funciona: Handlebars.js é um compilador escrito com JavaScript que pega qualquer HTML e expressão Handlebars e compila para uma função JavaScript. Esta função JavaScript derivada recebe um parâmetro, um objeto - seus dados - e retorna uma string com o HTML e os valores das propriedades do objeto inseridos dentro do HTML. Então, você acaba com um string (HTML) que tem valores de propriedades dos objetos inseridos em lugares relevantes, e insere a string na página.

#### Eu tenho que usar um motor de templates JavaScript? Se sim, por que?

Sim. Se você desenvolve ou planeja desenvolver aplicações JavaScript, você deve usar um sistema de templates JavaScript no lado do cliente para manter seu JavaScript e seu HTML suficientemente dissociado, que vai permitir que você gerencie seus arquivos HTML e JS de forma segura e fácil.

Claro, você pode usar [JSDOM](https://github.com/tmpvar/jsdom), ou pode usar templates no lado do servidor e enviar seus arquivos HTML via HTTP. Mas eu recomendo templates no lado do cliente porque isso é mais rápido que templates no servidor e fornece a forma mais fácil de criar e manter seus templates.

Acrescentando, quase todos os frameworks JavaScript front-end usam um motor de templates JavaScript, então você eventualmente usa template JavaScript, se você sempre usa um framework front-end ou backend.

> ## Tópicos neste post
> 
> * [Quando usar templates JS e por que Handlebars.js?](https://github.com/eoop/traduz-ai/blob/master/handlebars/001-aprenda-tudo-sobre-handlebars.md#quando-usar-templates-js-e-por-que-handlebarsjs)
> * [Visão geral sobre Handlebars.js](https://github.com/eoop/traduz-ai/blob/master/handlebars/001-aprenda-tudo-sobre-handlebars.md#vis%C3%A3o-geral-sobre-handlebarsjs)
> * [Comparando um projeto não handlebars com um projeto handlebars.js](https://github.com/eoop/traduz-ai/blob/master/handlebars/001-aprenda-tudo-sobre-handlebars.md#comparando-um-projeto-n%C3%A3o-handlebars-com-um-projeto-handlebarsjs)
> * [Aprenda a sintaxe Handlebars.js](https://github.com/eoop/traduz-ai/blob/master/handlebars/001-aprenda-tudo-sobre-handlebars.md#aprenda-a-sintaxe-handlebarsjs)
> * [Auxiliares Handlebars.js embutidos (condicionais e loops)](https://github.com/eoop/traduz-ai/blob/master/handlebars/001-aprenda-tudo-sobre-handlebars.md#auxiliares-handlebarsjs-embutidos-condicionais-e-loops)
> * [Auxiliares Personalizados Handlebars.js](https://github.com/eoop/traduz-ai/blob/master/handlebars/001-aprenda-tudo-sobre-handlebars.md#auxiliares-personalizados-handlebarsjs)
> * [4 maneiras de carregar/adicionar templates](https://github.com/eoop/traduz-ai/blob/master/handlebars/001-aprenda-tudo-sobre-handlebars.md#4-maneiras-de-carregaradicionar-templates)
> * [Handlebars.js com Backbone.js, jQuery, Ember.js e Meteor.js](https://github.com/eoop/traduz-ai/blob/master/handlebars/001-aprenda-tudo-sobre-handlebars.md#handlebarsjs-com-backbonejs-jquery-emberjs-e-meteorjs)

## Quando Usar Templates JS e por que Handlebars.js?

Temos alguns casos específicos de uso para motores de template JavaScript.

#### Quando Usar um Motor de Template JavaScript?

Você deve usar um motor de template JavaScript como Handlebars.js quando:

* Sempre que você usar um framework front-end JavaScript como Backbone.js, Ember.js, e similares. a maioria dos frameworks JavaScript front-end dependem de motores de template.
* A View da aplicação (a página HTML ou porções desta) vão ser atualizadas frequentemente, especilamente como um resultado de mudanças dos dados ou do servidor por meio de uma API REST ou a partir de dados do cliente.
* Você tem múltiplas tecnologias que dependem de seus dados a partir do servidor e você quer que todas elas processem os mesmos dados.
* Sua aplicação tem muita interatividade e é muito responsiva.
* Você está desenvolvendo uma aplicação web single page com múltiplas views.
* Você deseja gerenciar facilmente seu conteúdo HTML; você não quer que seu código JavaScript contenha importantes marcações HTML:
	```javascript
	shoesData.forEach (function (eachShoe) {
		// note a confusão entre HTML e JavaScript; é tedioso para acompanhar:
		theHTMLListOfShoes += '<li class="shoes">' + '<a href="/' + eachShow.name.toLowerCase() + '">' + eachShow.name + ' -- Price: ' + eachShoe.price + '</a></li>';
	});
	return theHTMLListOfShoes;

	```

#### Por que Handlebars.js (dos 8 ou mais motores template)?

Previsivelmente, temos muitos motores client-side (no lado do cliente) de templates JavaScript, mas nós vamos focar somente no Handlebars.js neste tutorial, visto que este é o melhor de todos. Alguns outros motores de template dignos são Underscore.js, Mustache.js, EJS e Dust.js.

Handlebars.js é uma extensão da linguagem de templates JavaScript Mustache; ele susbstitiu o Mustache.js.

Por que usar Handlebars.js? Aqui alguns motivos:

* Handlebars é um dos mais avançados (pré-compiladores e similares), rico de recurso e popular entre todos os motores de templates JavaScript, e tem a comunidade mais ativa.
* Handlebars é um motor de templates *logic-less*, que significa que há pouca ou nenhuma lógica em seus modelos que estão na página HTML. O mais importante uso de Handlebars, e qualquer motor de templates, é manter suas páginas HTML simples e limpas e desacopla-las de arquivos JavaScript lógicos, e o Handlebars cumpre esta proposta bem. Em adição, Dust.js é também um motor de templates *logic-less* e uma [digna alternativa](http://engineering.linkedin.com/frontend/client-side-templating-throwdown-mustache-handlebars-dustjs-and-more) para o Handlebars.js.
* Além disso, os frameworks JavaScript de última geração **Meteor.js** e **Derby.js**, que vamos cobrir nos próximos posts, são esperados para se tornarem *mainstream* nos meses seguintes, e ambos usam Handlebars.js. Para ser claro: Meteor.js usa Handlebars.js e Derby.js usa uma "sintaxe de template muito baseada no Handlebars". E **Ember.js usa Handlebars**, também.

	Enquanto Backbone.js é embalado com motor de templates Underscore.js, é super fácil usar Handlebars.js com o Backbone.js.

	Portanto, a experiência e conhecimento que você vai ganhar aprendendo Hanblebars.js agora vai ser muito valiosa, se você usa, ou planeja usar, qualquer framework JS.

Sucintamente, aprender Handlebars.js agora é um investimento e uma escolha sábia: você vai programar de forma mais eficaz agora e vai adaptar-se facilmente aos frameworks JS de amanhã e das próximas semanas e meses.


## Visão Geral Sobre Handlebars.js

Agora que vimos como usar o Handlebars em uma simples aplicação, vamos estudá-lo em detalhe.

### Como o Handlebars.js Funciona?

Como descrito na introdução: Handlebars.js é um compildar feito em JavaScript que pega qualquer HTML e expressão Handlebars e o compila para uma função JavaScript. Esta função JavaScript derivada então pega um parâmetro, um objeto - seu dado - e retorna uma string HTML com o valor da propriedade do objeto inserida (interpolada) dentro do HTML. Então, você termina com uma string (HTML) que tem o valor a partir da propriedade do objeto inserida nos lugares relevantes, e você insere a string na página.

Isto soa mais complicado do que é, vamos olhar mais de perto.

### As 3 Partes Principais dos Templates Handlebars

Para usar Handlebars, primeiro você liga o arquivo Handlebars.js no bloco *head* da sua página HTML, como se faz com jQuery ou qualquer arquivo .js... Então temos 3 partes do código que você usa para templates Handlebars:

1 . **Expressões Handlebars.js**
Expressões Handlebars são compostas de expressões Handlebars e qualquer conteúdo HTML ou expressões Handlebars dentro dentro da expressão (se a expressão é um bloco).

Uma simples expressão Handlebars é escrita dessa forma (onde "conteudo" pode ser uma variável ou uma função *helper* com - ou sem - parâmetros:

```html

{{ conteudo }}

```

Ou assim, no caso de bloco de expressões Handlebars (que nós vamos discutir em detalhe depois):

```html

{{#each}}
	Conteúdo HTML e outras expressões Handlebars vão aqui.
{{/each}}

```

Abaixo é uma expressão Handlebars com HTML. A variável *nomeCliente* é a propriedade que vai ser interpolada (seu valor vai ser inserido no lugar) pela função Handlebars.compile:

```html

<div> Name: {{ nomeCliente }} </div>

```

A saída vai ser a seguinte (se a variável nomeCliente tiver o valor "Richard"):

Name: Richard

Desde que você tenha que passar a expressão Handlebars (com qualquer HTML contido) para a função Handlebars.compile, uma tag `script` é usada para anexar cada template Handlebars quando eles estão na página HTML. Na verdade, a tag `script` não é necessária quando um template está no próprio arquivo HTML, mas é necessário quando o template Handlebars está junto com outro template Handlebars e outro conteúdo HTML.

**- Script Tag**

Templates Handlebars são embutidos nas tags `script` (onde as propriedades `type` das tags scripts são configuradas como `text/x-handlebars-template`). A tag script é similara tag script que você usa normalmente para incluir JavaScript na página HTML, exceto pelo atributo `type` que é diferente. Você recupera o conteúdo do HTML a partir da tag script e o passa para o compilador Handlebars.

Aqui temos um exemplo da tag `script` do Handlebars:

```html

<script id="header" type="text/x-handlebars-template">
	<div> Name: {{ headerTitle }}</div>
</script>

```

2 . **Dados (ou Contexto)**

A segunda parte do código no template Handlebars é o dado que você quer mostrar na página. Você passa seus dados como um objeto (um objeto regular JavaScript) para a função Handlebars. O *dado-objeto* é chamado de contexto. E este objeto pode ser composto de arrays, strings, números, outros objetos, ou uma combinação de todos eles.

Se o dado-objeto tem um array de objetos, você pode usar a função auxiliar Handlebars `each` (mais sobre auxiliares depois) para iterar o array, e o contexto atual é configurado para cada item dentro do array.

Aqui temos exemplos de configuração de objetos e como iterá-los com um template Handlebars.

- Objeto com array de objetos.

```javascript

// O objeto customers tem um array de objetos que vamos passar para o Handlebars:
var theData = {
	customers: [
		{
			firstName: "Michael", 
			lastName: "Alexander", 
			age: 20
		},
		{
			firstName: "John",
			lastName: "Allen",
			age: 29
		}
	]
};

```

Você pode usar o *auxiliar each* para iterar o objeto customer assim:

```html

<script id="header" type="text/x-handlebars-template">
	{{#each customers}} // note a referência ao objeto customers
		<li> {{ firstName }} {{ lastName }} </li>
	{{/each}}
</script>

```

Ou, uma vez que passamos o objeto customers como um array de objetos, nós podemos usar uma declaração de bloco auxiliar (mais sobre blocos auxiliares depois) como esta e referenciar o *customers* diretamente:

```html

<script id="header" type="text/x-handlebars-template">
	{{#customers}}
		<li> {{ firstName }}  {{ lastName }} </li>
	{{/customers}}
</script>

```

- Objeto com Strings

```javascript

var theData = {
	headerTitle: "Shop Page",
	weekDay: "Wednesday"
};

<script id="header" type="text/x-handlebars-template">
	<div> {{ headerTitle }} </div>
	Today is {{ weekDay }}
</script>

```

3 . **Função de Compilação do Handlebars**

A última parte de código que precisamos para templates Handlebars é realmente uma execução em dois passos:

	1. Compilar o template com a função Handlebars.compile.
	2. Então usar esta função compilada para invocar o objeto passado a ela (é preciso um objeto de dados como seu único parâmetro). E isto vai retornar uma string HTML com os valores do objeto interpolado inseridos dentro do HTML.

**Em resumo:** O a função Handlebars.compile pega o template como um parâmetro e retorna uma função JavaScript. Nós então usamos essa função compilada para executar o objeto de dados e retornar uma string com HTML e os valores do objeto interpolados. Então nós podemos inserir a string dentro da página HTML.

**Aqui temos as 3 partes juntas:**

**1 . Na página HTML:** Configuramos o template para usar expressões Handlebars, e adicionamos templates em uma tag script (se usar tags script: templates em um arquivo individual HTML não necessita de tags script):

```html

<script id="header" type="text/x-handlebars-template">
	<div> {{ headerTitle }} </div>
	Today is {{ weekDay }}
</script>

```

**2 . No arquivo JavaScript:** Inicialize o objeto de dados

```javascript

var theData = {
	headerTitle: "Shop Page",
	weekDay: "Wednesday"
};

// Recupere o HTML a partir da tag script que configuramos no passo 1
// Vamos usar o id (header) da tag script como alvo na página
var theTemplateScript = $("#header").html();

```

**3 . Ainda no arquivo JavaScript:** Quando nós usamos a função de compilação do Handlebars para compilar os templates.

Compilar o modelo recuperado pela tag script:

```javascript

// A função Handlebars.compile retorna uma função para a variável theTemplate
var theTemplate = Handlebars.compile(theTemplateScript);

```

Usando a função `theTemplate()` retornada pela função compiladora para gerar a string final com o objeto de dados interpolados. Nós passamos o objeto de dados como um parâmetro. Então anexamos a string gerada com HTML para a página:

```javascript

$(document.body).append( theTemplate( theData ) );

```

Isto vai retornar nosso HTML com os valores vindos do objeto, inseridos no lugar, e o resultado vai ser como este:

```html

Shop Page
Today is Wednesday

```


## Comparando um projeto não handlebars com um projeto handlebars.js

Para obter uma visão geral de alto nível do que o desenvolvimento com Handlebars implica, quando comparado com templates não JavaScript, vamos construir um rápido e pequeno projeto JavaScript.

Antes de usarmos Handlebars, vamos fazer uso do jQuery e JavaScript sem Handlebars, para termos uma noção de o quê estamos fazendo e como o Handlebars vai fazer alguma diferença.

Sem Handlebars, será como um típico projeto JavaScript/jQuery vai parecer quando você tiver conteúdo para adicionar a página com alguns dados. Vamos simplismente mostrar uma lista de sapatos e preços.

### Um pequeno projeto sem Handlebars

**1** . Download Handlebars.js e jQuery:
Faça o download da última versão do Handlebars através do GitHub (nós não vamos usá-la ainda, mas inclua na sua página). Pegue a versão completa, não a versão "runtime only" (mais sobre a versão runtime depois): https://github.com/wycats/handlebars.js/downloads

Também faça o download da última versão do jQuery aqui (nós vamos usá-lo ao longo deste tutorial): http://code.jquery.com/jquery-1.9.1.min.js

**2** . Crie um novo diretório em seu computador chamado "Handlebars_tuts" e coloque os arquivos jQuery e o Handlebars.js nele.

Ou você pode abrir o terminal (no MAC) e mudar para o diretório "Handlebars_tuts". Então digitar os comandos seguintes para fazer o download de ambos os arquivos diretamente no diretório com o comando curl:

```
curl http://code.jquery.com/jquery-1.9.1.min.js > jquery-1.9.1.min.js 
curl https://github.com/downloads/wycats/handlebars.js/handlebars-1.0.rc.1.min.js > Handlebars.js

```

**3** . Faça um arquivo `index.html` e adicione o seguinte:

```html

<html>
	<head>
		<script type="text/javascript" src="jquery-1.9.1.min.js"></script>
	</head>
	<body>
		The List of Shoes:
		<ul class="shoesNav"></ul>
	</body>
</html>

```

**4** . Crie um arquivo main.js e adicione o seguinte:

Note que este arquivo JS tem ambos HTML e JavaScript misturados em uma sopa insalubre:

```javascript

$(function () {
	var shoesData = [
		{ name: "Nike", price: 199.00 },
		{ name: "Loafers", price: 59.00 },
		{ name: "Wing Tip", price: 259.00 }
	];

	function updateAllShoes ( shoes ) {
		var theHTMLListOfShoes = "";

		shoesData.forEach (function ( eachShoe ) {
			// Note o HTML e JavaScript misturado - é tedioso de acompanhar
			theHTMLListOfShoes += '<li class="shoes">' + '<a href="/' + eachShoe.name.toLowerCase() + '">' + eachShoe.name + ' -- Price: ' + eachShoe.price + '</a></li>';
	    });

	    return theHTMLListOfShoes;
	}

	$(".shoeNav").append( updateAllShoes( shoesData ) );
});

```

Se você abrir este arquivo index.html em seu navegador, você deve ver uma simples lista com 3 itens. Isto é como normalmente nós desenvolvemos no front end sem um motor de templates JavaScript.

### Um pequeno projeto Handlebars

Agora vamos refatorar o código acima e usar Handlebars.js como templates.

**1** . Alterações no `index.html`:

Adicione este código logo abaixo do fechamento da tag `ul`.

```javascript
<script id="shoe-template" type="x-handlebars-template">​
   {{#each this}}​
    <li class="shoes">
    	<a href="/{{name}}">{{name}} -- Price: {{price}} </a>
	</li>​
    {{/each}}
​</script> 
```

**2** . Alterações no `main.js`:

E aqui está o código javascript refatorado que faz uso do Handlebars. 
Remova todo o código javascript e troque pelo código abaixo:

- Note que nos livramos da função `updateAllShoes()`.
- E também note que não existe HTML no javascript, todo o HTML está no HTML.

```javascript
$(function  () {
  var shoesData = [
	  {name:"Nike", price:199.00 }, 
	  {name:"Loafers", price:59.00 }, 
	  {name:"Wing Tip", price:259.00 }
  ];

   // Captura o html do template dentro da tag script
    var theTemplateScript = $("#shoe-template").html(); 
​
   // Compila o template
   var theTemplate = Handlebars.compile(theTemplateScript); 
   $(".shoesNav").append(theTemplate(shoesData)); 
​
	​// Passamos o objeto shoesData para ser compilado na função do handlebars.
	// A função irá inserir todos os valores do objeto nos seus devidos lugares no HTML e retorna o HTML como uma string. Então usamos jQuery para adicionar este conteúdo na página.
});
```

Quando você atualizar a página `index.html` deverá visualizar o mesmo resultado que foi obtido no exemplo acima sem Handlebars.

O procedimento ilustra um uso muito básico do Handlebars.js. Como você pode ver, usar Handlebars permite nos separar o HTML do Javascript. Isto é ainda mais importante quando nossa aplicação se torna mais complexa; o mais facil será desenvolver templates separados e gerenciá-los de uma forma eficaz. Enquanto que o exemplo sem Handlebars seria complicado de gerenciar quando nossa aplicação tornar-se maior.


### A principal diferença entre os dois projetos

Esta é a principal diferença entre o projeto sem Handlebars e o projeto com Handlebars: O projeto sem Handlebars possui uma marcação HTML importante dentro do código Javascript, o que torna mais difícil para gerenciar (criações e atualizações) do HTML.

```javascript
// Você pode ver o HTML e JS entrelados
​
​function updateAllShoes(shoes)  {
​var theHTMLListOfShoes = "";

shoesData.forEach(function (eachShoe)  {
 theHTMLListOfShoes += '<li class="shoes">' + '<a href="/' + eachShoe.name.toLowerCase() + '">' + eachShoe.name + ' -- Price: ' + eachShoe.price + '</a></li>';
    });
    return theHTMLListOfShoes;
}

$(".shoesNav").append(updateAllShoes(shoesData));
```

Enquanto que no projeto com Handlebars, o código Javascript não contém marcação HTML (a marcação HTML é na página HTML; apenas javascript no código javascript):

```javascript
var theTemplateScript = $("#shoe-template").html();
​var theTemplate = Handlebars.compile(theTemplateScript); 
$(".shoesNav").append(theTemplate(shoesData)); 
```

## Aprenda a sintaxe Handlebars.js

* Expressões Handlebars.js

Vimos acima a expressão Handlebars. Expressões Handlebars são escritas com (chaves duplas no início, seguido do conteúdo a ser avaliado e chaves duplas no final);

```
<div>{{ Conteúdo vem aqui }}</div>
```

A variável customerName é a propriedade (a expressão que será avaliada pelo compilador do Handlebars) que será interpolada (os seus valores serão inseridos no lugar) pela função compilada do Handlebars quando ele executa:

```
<div> Name: {{ customerName }} </div>
```

* Comentários

Isto é como você adiciona comentários em um template Handlebars:

```
{{! Tudo dentro desta expressão de comentário não será exibido }}
```

E você pode usar também comentários HTML, mas eles serão inseridos no código fonte como comentários HTML:

```
<!-- Comentário HTML que será inserido -->
```

* Blocos _(block)_

Blocos in Handlebars são expressões que possuem um bloco, são abertos com `{{# }}` e fechados com `{{/ }}`.

Falaremos mais sobre blocos depois com mais detalhes; esta é uma sintaxe para um bloco.

```
{{#each}}Conteúdo vem aqui. {{/each}}
```

Aqui é um bloco de if

```
{{#if algumValorForTrue}}Conteúdo vem aqui.{{/if}}
```

FALTA TRADUZIR:
> The words block and helper are sometimes used interchangeably because most built-in helpers are blocks, although there are function helpers and block helpers.


* Caminhos (com uso de ponto)

Um caminho no Handlebars é uma pesquisa de propriedades. Se temos uma propriedade _name_ que contém um objeto como:

```javascript
var objData = {
	name: {
		firstName: "Michael", 
		lastName:"Jackson"}
	}
```

Podemos usar caminhos aninhados (utilizando ponto) para pesquisar a propriedade que você quer:

```
{{name.firstName}}
```

* Caminho pai

Handlebars também possui caminho pai para pesquisar propriedades em um pai do contexto atual. Com um objeto de dados como:

```javascript
var shoesData = {
	groupName: "Celebrities", 
	users:[
		{name:{ firstName:"Mike",  lastName:"Alexander" }}, 
		{name:{ firstName:"John",  lastName:"Waters" }}
	]
};
 
// Podemos usar o caminho pai ../ para acessar a propriedade groupName:
​<script id="shoe-template" type="x-handlebars-template">​
   {{#users}}​
    <li>{{name.firstName}} {{name.lastName}} is in the {{../groupName}} group.</li>​
    {{/users}}
​</script>
```

O HTML renderizado será:

```
Mike Alexander is in the Celebrities group.
John Waters is in the Celebrities group.
```

* Contexto

Handlebars se refere ao objeto que você passou a sua função como _contexto_. Ao longo deste artigo, nós usamos "object data" e ás vezes "data" ou "objetc" para se referir ao contexto do objeto. Todas estas palavras são usadas indiferentemente todo o tempo, mas você vai entender sem problemas que nós estamos se referindo ao objeto que está sendo passado dentro da função Handlebars.

* Chaves tripla `{{{ }}}` para não escapar HTML.

Normalmente, você usa chaves dupla `{{ }}` para expressões Handlebars, e por padrão o conteúdo com chaves dupla escapa para "proteger contra problemas de XSS, que são causados por dados maliciosos passados para o servidor como JSON". Isto assegura que código malicioso no HTML não pode ser inserido dentro da página. Mas ás vezes você quer HTML cru. Para isto você pode usar chaves triplas `{{{ }}}`. As chaves triplas fazem com que o Handlebars não escape o HTML contido no conteúdo.

* Parcial (sub-templates)

As vezes você quer renderizar uma sessão de template dentro de um maior. Você usa partials para fazer isso no Handlebars, e a expressão é:

```
 {{> partialName}}
```

Vamos adicionar um template parcial ao projeto Handlebars que fizemos anteriormente. Nós vamos adicionar _color_ e _size_ em cada sapato.

Alterações para o `main.js`:

**1** . Substitua os dados do objeto existentes. Nós vamos adicionar propriedades para cor e tamanho:

```javascript
var shoesData = {
	allShoes:[
		{
			name:"Nike", 
			price:199.00, 
			color:"black", 
			size:10 
		}, 
		{
			name:"Loafers", 
			price:59.00, 
			color:"blue", 
			size:9 
		}, 
		{
			name:"Wing Tip", 
			price:259.00, 
			color:"brown", 
			size:11 
		}
	]
};
```

**2** . Adicione o código abaixo, antes da linha com o `append` do jQuery:

```javascript
Handlebars.registerPartial("description", $("#shoe-description").html());
```

Alterações para a `index.html`:

**1** . Substitua o template `shoe-template` por este:

```javascript
<script id="shoe-template" type="x-handlebars-template">​
	{{#each allShoes}}​
		<li class="shoes">​
			<span class="shoe-name"> {{name}} - </span> price: {{price}}
				{{> description}}
		</li>​
	{{/each}}
​</script>
```

**2** . Adicione este novo template para o *description*, logo abaixo do *shoe-template*:

```javascript
<script id="shoe-description" type="x-handlebars-template">​
	​<ul>​
		<li>{{color}}</li>​
		<li>{{size}}</li>​
	​</ul>​
​</script>
```

O HTML final deverá mostrar cor e tamanho abaixo de cada item da lista.

É assim que você adiciona um parcial (ou sub-template para o modelo principal).

## Auxiliares Handlebars.js embutidos (condicionais e loops)

Como vimos anteriormente Handlebars é um _logic-less_ (possui pouca ou nenhuma lógica embutida nos templates) motor de template. Mas deve haver uma maneira para executar lógica complexa quando usamos os templates. Handlebars fornece esta funcionalidade com os *Auxiliares* (Helpers), que são condicionais e loops para a execução de lógicas simples.

São expressões e blocos lógicos que fornecem a lógica necessária para os templates na página HTML. Você pode adicionar funcionalidade complexa com o seu próprio auxiliar.

### Auxiliares embutidos

Os auxiliares embutidos (condicionais e loops) são:

* *each* `{{#each}}`

Vimos este auxiliar anteriormente. Ele permite você iterar sobre um _array_ ou _objeto_ provido em um objeto de dados. Por exemplo, se você possui um _array_ de itens e você gostaria de lista-los na página, você pode fazer isto:

```javascript
var favoriteFruits = {
	allFruits:["Tangerine", "Mango", "Banana"]
};  
```
Em seguida nós usamos o bloco `each` dentro da tag script. Note que _this_ está se referindo a cada item do array *allFruits*.

```javascript
<script id="fruits-template" type="x-handlebars-template">​
	// Lista de frutas
   {{#each allFruits}}​
    <li>{{this}} </li>​
    {{/each}}
​</script>
```

Isto irá resultar no seguinte HTML:

```
 <li> Tangerine </li>​
​<li> Mango </li>​
​<li> Banana </li> 
```

Também, se o objeto de dados passado para o auxiliar *each* não for um array, o objeto inteiro é o contexto atual e nós usamos a palavra _this_.

Se os dados se parecem com isto:

```javascript
 var favoriteFruits = {
	firstName:"Peter", 
	lastName:"Paul"
};
```

Ao contrário disto:

```javascript
 var favoriteFruits = {
	customers: {
		firstName:"Peter", 
		lastName:"Paul"
	}
};
```

Nós usamos a palavra _this_ como visto aqui:

```javascript
{{#each this}}​
	<li>{{firstName}} {{lastName}} </li>​
{{/each}}
```

* Propriedades aninhadas com o auxiliar `each`:

Vamos usar as propriedades aninhadas (que nós aprendemos anteriormente) com o auxiliar `each`:

```javascript
var favoriteFruits = {
	allFruits:[{
		fruitName:"Tangerine", 
		naitiveTo:[{
			country:"Venezuela"}
		]},
		{
			fruitName:"Mango"
	}] 
};
```

```
{{#each allFruits}}​
	<li>{{fruitName}} {{naitiveTo.0.country}} </li>​
{{/each}}
```

* Auxiliar *if*: `{{#if}}`

O auxiliar `if` funciona apenas como uma instrução regular, exceto que ele não aceita qualquer lógica condicional. Ele checa se há valores como *verdadeiro*, qualquer valor não vazio ou não nulo e/ou similar. O bloco é apenas renderizado se o `if` for um valor verdadeiro.

Aqui são alguns exemplos:

Nós verificamos para ver se a propriedade `userActive` é verdadeira. Se for, o bloco é renderizado.

```
<div class="user-data">​
	{{#if userActive}}​
		Welcome, {{firstName}}
	{{/if}}
​</div>
```

Um dos desenvolvedores do Handlebars aconselhou: esta é a melhor maneira para checar a propriedade `length` de uma variável, para capturar casos onde um _array_ poderia ser vazio:

```
<div class="user-data">​
	{{#if userActive.length}}​
		Welcome, {{firstName}}
	{{/if}}
​</div> 
```

Como mencionado acima, o auxiliar `if` não avalia lógica condicional, assim nós não podemos fazer isto:

```
<div class="user-score">​
	{{#if userScore > 10 }}​
		Congratulations, {{firstName}}, You did it.
	{{/if}}
​</div> 
```

Nós temos que usar um auxiliar customizado (discutimos isso em breve) para adicionar tal lógica condicional.

* *Else*: `{{else}}`

A sessão `else` (note que isto não é um bloco próprio) é bastante simples, desde que seja sessão de um bloco. Isto pode ser usado com um bloco `if` ou qualquer outro bloco. O conteúdo da sessão `else` é apenas renderizado se expressões forem avaliadas como falso.

Nós verificamos para ver se a propriedade `userLoggedIn` é verdadeira. Se não for, o conteúdo do bloco `else` é renderizado.

```
<div class="user-data">​
	{{#if userLoggedIn}}​
		Welcome, {{firstName}}
	{{else}}
		Please Log in.
	{{/if}}
​</div>
```

* Auxiliar *unless*: `{{#unless}}` 

Como uma alternativa para o auxiliar `else`, você usa o auxiliar de bloco `unless`. O conteúdo entre o bloco `unless` apenas é renderizado se a expressão for avaliada como um valor falso. Portanto esta é a melhor maneira se você for querer checar apenas valores falsos:

Nós substituimos o bloco `if` e `else` por apenas o bloco `unless`, para renderizar o conteúdo apenas se a propriedade for avaliada como falso.

Esta expresssão lê: apenas renderiza o conteúdo do bloco se a propriedade `userLoggedIn` for avaliada como falsa.

```
<div class="user-data">​
	{{#unless userLoggedIn}}​
		Please Log in.
	{{/unless}} 
​</div>
```

* *With* helper: `{{#with}}`

O auxiliar `with` permite atingir uma propriedade específica de um objeto. Por exemplo, nós sabemos que o objeto sempre é o contexto atual em um template Handlebars, como nós temos aprendido em algumas sessões anteriores. Mas se você precisa atingir uma propriedade diferente para o contexto atual, você pode usar o auxiliar `with`, assim você poder usar o caminho do pai (../) para fazer isto. Vamos ver o auxiliar `with` em ação:

Se nós temos um objeto de contexto como:

```javascript
var shoesData = {
	groupName:"Celebrities", 
	celebrity:{
		firstName:"Mike", 
		lastName:"Alexander" 
	} 
};  
```

Nós podemos usar o bloco `with` para atingir a propriedade *groupName* onde nós precisamos acessar os seus valores: 

```
<script id="shoe-template" type="x-handlebars-template">​
	{{groupName}} Group
		{{#with celebrity}}​
			<li>{{firstName}} {{lastName}}</li>​
		{{/with}}
​</script>
```

E este é o HTML renderizado:

> Celebrities Group
> – Mike Alexander

O bloco `with` é provavelmente um que você raramente vai usar.


















