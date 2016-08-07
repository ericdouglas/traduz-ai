# Objetos JavaScript em Detalhe

* **Artigo original**: [JavaScript Objects in Detail](http://javascriptissexy.com/javascript-objects-in-detail/)
* **Tradução**: [Eric Douglas](https://github.com/ericdouglas)

O tipo de dado central, o mais fundamental e freqüentemente utilizado, do JavaScript é o tipo Object. O JavaScript tem 1 tipo de dado complexo, o tipo Object, e tem 6 tipos de dados simples: Boolean, Number, String, Symbol, undefined e null. Note que estes tipos de dados simples (primitivos) são imutáveis (não podem ser alterados), enquanto os objetos são mutáveis (podem ser alterados).

## **O que é um Objeto**
Um objeto é uma lista não ordenada de tipos de dados primitivos (e às vezes tipos de dados referência) que são armazenados como uma série de pares nome-valor. Cada item na lista é chamado _propriedade_ (funções são chamadas de _métodos_).

Considere este simples objeto:

```js
var meuPrimeiroObjeto = {primeiroNome: "Richard", autorFavorito: "Conrad"};
```

Reiterando, pense num objeto como uma lista que contém ítens; e que cada item (uma propriedade ou um método) na lista é armazenado por um par nome-valor. Os nomes das propriedades no exemplo acima são `primeiroNome` e `autorFavorito`; e os valores são "Richard" e "Conrad".

Os nomes das propriedades podem ser uma string ou um número; mas se o nome da propriedade for um número, ele tem de ser acessado com o uso de colchetes. Mais sobre a notação colchetes mais tarde. Aqui está um outro exemplo de objetos com números sendo o nome da propriedade:

```javascript
var grupoDeIdade = { 30: "Crianças", 100: "Muito Velho" };
console.log(grupoDeIdade.30) // Isto vai lançar um erro

// Assim é como você vai acessar o valor da propriedade 30, para obter o valor "Crianças"
console.log(grupoDeIdade["30"]); // Crianças

// É melhor evitar usar números como nomes de propriedades.
```

Como um desenvolvedor JavaScript você muito freqüentemente irá usar os tipos de dados Object; na maioria das vezes para armazenar dados e para criar os seus próprios métodos e funções.


## **Tipo de Dados Referência e Tipos de Dados Primitivos**
Uma das principais diferenças entre o tipo de dados referência e os tipos de dados primitivos é que o valor do tipo referência é armazenado como uma referência (ponteiro de memória). Ele não é portanto diretamente armazenado na variável como um valor, como os dos tipos primitivos são. Por exemplo:

```javascript
// O tipo de dado primitivo String é armazenado como um valor
var pessoa = "Kobe";
var outraPessoa = pessoa; // outraPessoa = o valor de pessoa
pessoa = "Bryant"; // valor de pessoa mudado

console.log(outraPessoa); // Kobe
console.log(pessoa); // Bryant
```

É interessante notarmos que mesmo embora tenhamos alterado `pessoa` para "Bryant", a variável `outraPessoa` ainda retém o antigo valor que `pessoa` tinha.

Compare o dado primitivo salvo-como-valor demonstrado acima com o salvo-como-referência por objetos:

```javascript
var pessoa = {nome: "Kobe"};
var outraPessoa = pessoa;

pessoa.nome = "Bryant";

console.log(outraPessoa.nome);	// Bryant
console.log(pessoa.nome); // Bryant
```

Neste exemplo, nós copiamos o objeto `pessoa` para `outraPessoa`; mas por causa do valor em `pessoa` estar armazenado como uma referência e não como um valor de verdade, quando nós alteramos a propriedade `pessoa.nome` para "Bryant", a variável `outraPessoa` refletiu a mudança, pois ela nunca armazenou de fato a sua própria cópia do valor das propriedades de `pessoa`, somente tinha uma referência a ela.


## **As Propriedades dos Dados de Objetos Têm Atributos**
Cada propriedade de dados (a propriedade do objeto que armazena dados) não tem somente o par nome-valor, mas também 3 atributos (os 3 atributos são definidos como `true` por padrão):

- **Configurável**: Especifica se acaso a propriedade em si pode ser apagada ou os seus atributos mudados.
- **Enumerável**: Especifica se a propriedade pode ser retornada em um loop for/in ou do método **Object.keys()**. 
- **Gravável**: Especifica se o valor associado à propriedade pode ser alterado.

Note que o ECMAScript 5 especifica propriedades acessoras em companhia com as propriedades dados especificadas acima.
E as propriedades acessoras são funções ("getters" & "setters"). Aprenderemos mais sobre o ECMAScript 5 numa postagem já agendada
programada para 15 de fevereiro.

## **Criando Objetos**
Estes são os dois meios comuns de se criar objetos:

### 1. **Objetos Literais**
O mais comum e, de fato, o jeito mais fácil de se criar objetos é através de objetos literais como descrito aqui:

```javascript
// Este é um objeto vazio inicializado usando a notação de objeto literal
var meusLivros = {};

// Este é um objeto com 4 ítens, novamente usando a forma de objeto literal
var manga = {
	cor: "amarela",
	forma: "redonda",
	doçura: 8,
	quãoDoceEuSou: function () {
		console.log("Hmm Hmm Bom!");
	}
};
```

### 2. **Objeto Construtor**
O segundo meio mais comum de criar objetos é com o construtor `Object`. Um construtor é uma função usada para inicializar novos objetos, e você usa a palavra-chave `new` para chamar o construtor.

```javascript
var manga = new Object();

manga.cor = "amarela";
manga.forma = "redonda";
manga.doçura = 8;

manga.quãoDoceEuSou = function () {
	console.log("Hmm Hmm Bom!");
};
```

Mesmo você podendo usar algumas palavras reservadas como `for` para nomear propriedades em seus objetos, é totalmente aconselhável que você **NÃO** faça isso.

Objetos podem conter qualquer outro tipo de dados, incluindo Numbers, Arrays e mesmo outros Objects.


## **Padrões Práticos para Criação de Objetos**
Para simples objetos que podem ser usados somente uma vez na sua aplicação para guardar dados, os dois métodos apresentados acima serão suficientes para a criação de objetos.

Imagine agora que você tenha uma aplicação que mostre frutas e os detalhes sobre cada uma delas. Todas as frutas na sua aplicação têm estas propriedades: cor, forma, doçura, custo e uma função `mostrarNome`. Isso seria totalmente entediante e improdutivo para digitar toda vez que você quisesse criar um novo objeto fruta.

```javascript
var frutaManga = {
	cor: "amarela",
	doçura: 8,
	nomeFruta: "manga",
	terrasNativas: ["América do Sul", "América Central"],

	mostrarNome: function () {
		console.log("Esta é a " + this.nomeFruta + ".");
	},

	nativaDe: function () {
		this.terrasNativas.forEach(function (cadaPaís) {
			console.log("Originária da " + cadaPaís + ".");
		});
	}
};
```

Se você tiver 10 frutas, você vai ter que adicionar o mesmo código 10 vezes. E o que acontece se você tiver que mudar o código da função `nativaDe`? Você terá que fazer a alteração em 10 locais diferentes. Agora extrapole isto para a adição de objetos para membros de um website e subitamente você percebeu que a maneira que nós criamos os objetos até agora não é a ideal para se criar instâncias, especialmente quando estivermos desenvolvendo grandes aplicações.

Para resolver estes problemas repetitivos, os engenheiros de software inventaram padrões (soluções para tarefas comuns e repetitivas) para fazer o desenvolvimento de aplicações mais eficiente e dinamizado.

Aqui temos 2 padrões comuns para criação de objetos. Se você terminou o curso "Como Aprender JavaScript Corretamente", você viu nas lições em Codecademy que utilizavam este primeiro padrão freqüentemente:

### 1. **Padrão Construtor para Criação de Objetos**

```javascript
function Fruta(aCor, aDoçura, oNomeDaFruta, asTerrasNativas) {
	this.cor = aCor;
	this.doçura = aDoçura;
	this.nomeFruta = oNomeDaFruta;
	this.terrasNativas = asTerrasNativas;

	this.mostrarNome = function () {
		console.log("Isto é um(a) " + this.nomeFruta + ".");
	};

	this.nativaDe = function () {
		this.terrasNativas.forEach(function (cadaPais) {
			console.log("Originária de: " + cadaPais + ".");
		});
	};
}
```

Com este padrão pronto, é facílimo de se criar toda sorte de frutas. Daí:

```javascript
var frutaManga = new Fruta ("amarela", 8, "manga", ["América do Sul", "América Central", "Oeste da África"]);
frutaManga.mostrarNome(); // Isto é um(a) manga.
frutaManga.nativaDe();

// Originária de: América do Sul.
// Originária de: América Central.
// Originária de: Oeste da África.

var frutaAbacaxi = new Fruta ("marrom", 5, "abacaxi", ["Estados Unidos"]);
frutaAbacaxi.mostrarNome(); // Isto é um(a) abacaxi.
frutaAbacaxi.nativaDe(); // Originária de: Estados Unidos.
```

Se você tiver que alterar a função `mostrarNome`, você somente precisa fazer isso em um local. O padrão encapsula todas as funcionalidades e características das frutas apenas fazendo uma simples função Fruta com herança.

**Notas**: 
- Uma propriedade herdada é definida na propriedade *prototype* (protótipo) do objeto. Por exemplo: `algumObjeto.prototype.primeiroNome = "Riquinho";`


- Uma propriedade própria é definida diretamente no objeto em si; por exemplo:

```javascript

// Vamos criar um objeto primeiro
var umaManga = new Fruta();

// Agora nós vamos definir a propriedade mangaSabor
// diretamente no objeto umaManga,
// sendo essa uma propriedade própria do objeto umaManga,
// não uma propriedade herdada.

umaManga.mangaSabor = "algum valor";
```

- Para acessar uma propriedade de um objeto, nós podemos usar `meuObjeto.nomePropriedade`; por exemplo:

```javascript
console.log(umaManga.mangaSabor);	// "algum valor"
```


- Para invocar um método de um objeto, nós usamos `meuObjeto.nomeDoMétodo()`; por exemplo:

```javascript
// Primeiro vamos adicionar um método
umaManga.imprimirTreco = function () { return "Imprimindo"; };

// Agora vamos invocar o método imprimirTreco
umaManga.imprimirTreco();
```

### 2. **Padrão Prototípico para Criação de Objetos**

Antes de discutirmos sobre o Padrão Prototípico, você deve entender sobre protótipos no JavaScript. Se você não conhece, leia esta postagem: [Protótipos Javascript em uma Linguagem Simples](https://github.com/cerebrobr/traduz-ai/blob/master/javascript/006-prototipos-javascript-em-uma-linguagem-simples.md#prot%C3%B3tipos-javascript-em-uma-linguagem-simples)

```javascript
function Fruta() {

}

Fruta.prototype.cor = "amarela";
Fruta.prototype.doçura = 7;
Fruta.prototype.nomeDaFruta = "fruta genérica";
Fruta.prototype.terraNativa = "USA";

Fruta.prototype.mostrarNome = function () {
	console.log("Isto é um(a): " + this.nomeDaFruta + ".");
};

Fruta.prototype.nativaDe = function () {
	console.log("Originária de: " + this.terraNativa + ".");
};
```

E assim é como nós chamamos o construtor `Fruta()` neste padrão prototípico:

```javascript
var frutaManga = new Fruta;

frutaManga.mostrarNome();   // "Isto é um(a): fruta genérica."
frutaManga.nativaDe();		// "Originária de: USA."
```

## Leituras Adicionais

Para uma completa discussão sobre estes dois padrões e uma explicação minuciosa de como cada um funciona e as desvantagens de cada, leia o capítulo 6 de *Professional JavaScript for Web Developers*. Você também irá aprender qual padrão Zakas recomenda como o melhor para se usar (Dica: não é nenhum dos 2 acima).

## **Como Acessar Propriedades de um Objeto**
Os dois modos primários para acessar as propriedades de um objeto são com a notação com ponto e com a notação com colchetes.

### 1. **Notação com Ponto**

```javascript
// Até aqui temos usado a notação com ponto nos exemplos acima; aqui temos outro exemplo novamente:

var livro = {título: "Caminhos para Ir", páginas: 280, marcador1: "Página 20"};

// Para acessar as propriedades do objeto livro com a notação com ponto, faça isto:
console.log(livro.título);	// Caminhos para Ir
console.log(livro.páginas);	// 280
``` 

### 2. **Notação com Colchetes**

```javascript
// Para acessar as propriedades do objeto livro com a notação com colchetes, faça isto:
console.log(livro["título"]);	// Caminhos para Ir
console.log(livro["páginas"]);	// 280

// Ou, no caso de você ter o nome da propriedade numa variável:
var títuloDoLivro = "título";
console.log(livro[títuloDoLivro]);	// Caminhos para Ir
console.log(livro["marcador" + 1]);	// Página 20
``` 

Caso você acesse uma propriedade dum objeto que não exista, resultar-se-á em `undefined`.

## Propriedades Próprias e Herdadas

Objetos têm propriedades herdadas e propriedades próprias. As próprias são definidas no próprio objeto;, enquanto as propriedades herdadas foram herdadas do objeto protótipo do construtor (outro objeto).

Para descobrir se uma propriedade existe num objeto (seja por herança ou própria), você usa o operador `in`:

```javascript
// Cria um novo objeto escola com uma propriedade nomeEscola
var escola = { nomeEscola: "MIT" }; 

// Imprime true porque nomeEscola é uma propriedade própria do objeto escola
console.log("nomeEscola" in escola);	// true

// Imprime false porque nós não definimos a propriedade tipoEscola no objeto escola,
// e nem adicionamos esta propriedade ao objeto protótipo Object.prototype
console.log("tipoEscola" in escola);	// false

// Imprime true porque o objeto escola herdou o método toString de Object.prototype
console.log("toString" in escola);	// true
```

### hasOwnProperty()

Para descobrir se um objeto possui uma propriedade em específico como propriedade própria, você usa o método `hasOwnProperty`. Este método é utilíssimo pois, de tempos em tempos, você precisa enumerar um objeto e você quer somente as propriedades próprias, e não as herdadas.

```javascript
// Crie um novo objeto escola com a propriedade nomeEscola
var escola = { nomeEscola: "MIT" };

// Imprime true porque nomeEscola é um método próprio do objeto escola
console.log(escola.hasOwnProperty("nomeEscola"));	// true

// Imprime false porque o objeto escola herda o método toString de 
// Object.prototype, portanto toString não é uma propriedade própria do objeto escola
console.log(escola.hasOwnProperty("toString"));
```

### Acessando e Enumerando Propriedades nos Objetos

Para acessar propriedades enumeráveis (próprias e herdadas) nos objetos, você deve usar o loop `for/in` ou um loop `for` regular.

```javascript
// Crie um novo objeto escola com 3 propriedades próprias: nomeEscola, aprovadoEscola e localEscola
var escola = { nomeEscola: "MIT", aprovadoEscola: true, localEscola: "Massachusetts" };

// Use do loop for/in para acessar as propriedades no objeto escola
for (var cadaItem in escola) {
	console.log(cadaItem);	// Imprime nomeEscola, aprovadoEscola, localEscola
}
```

### Acessando Propriedades Herdadas

Propriedades herdadas de `Object.prototype` não são enumeráveis, então o loop `for/in` não as mostra. Entretanto, propriedades herdadas que são enumeráveis são reveladas na iteração do loop `for/in`. Por exemplo:

```javascript
// Use o loop for/in para acessar as propriedades no objeto escola
for (var cadaItem in escola) {
	console.log(cadaItem);	// Imprime nomeEscola, aprovadoEscola, localEscola
}

// Crie uma nova função EnsinoSuperior em que o objeto escola irá herdar.

/*	NOTA LATERAL: Como Wilson (um leitor atento) corretamente apontou nos comentários abaixo, a propriedade nívelEnsino 
não é exatamente herdada pelos objetos que usam o construtor EnsinoSuperior; ao invés disso, a propriedade 
nivelEnsino é criada como uma nova propriedade em cada objeto que usar o construtor EnsinoSuperior. 
A razão para que a propriedade não fora herdada é que nós usamos a palavra chave "this" para definir a propriedade
*/

function EnsinoSuperior () {
	this.nívelEnsino = "Universidade";
}

// Implemente a herança com o construtor EnsinoSuperior
var escola = new EnsinoSuperior;
escola.nomeEscola = "MIT";
escola.aprovadoEscola = true;
escola.localEscola = "Massachusetts";


// Use o laço for/in para acessar as propriedades do objeto escola
for (var cadaItem in escola) {
	console.log(cadaItem);	// Imprime nívelEnsino, nomeEscola, aprovadoEscola e localEscola
}
```

No último exemplo, note que a propriedade `nívelEnsino` que definimos na função `EnsinoSuperior` está listada nas propriedades do objeto `escola`, mesmo embora `nívelEnsino` não sendo uma propriedade própria; foi herdada.


### Atributo Prototype dos Objetos e Propriedade Prototype

O atributo `prototype` e a propriedade `prototype` de um objeto são conceitos criticamente importantes de se entender no JavaScript. Leia minha postagem [Protótipos Javascript em uma Linguagem Simples](https://github.com/cerebrobr/traduz-ai/blob/master/javascript/006-prototipos-javascript-em-uma-linguagem-simples.md#prot%C3%B3tipos-javascript-em-uma-linguagem-simples), para mais informações.


### Apagando Propriedades de um Objeto

Para remover uma propriedade de um objeto, usamos o operador `delete`. Você não pode apagar propriedades que foram herdadas, nem propriedades cujos atributos estão definidos como não configuráveis. Você deve apagar as propriedades herdadas no objeto protótipo (onde as propriedades foram definidas). Você também não pode remover propriedades do objeto global, as quais foram declaradas com as palavras-chaves `var`, `let`, `const`, `function` ou `class`.

O operador `delete` retorna `true` se a remoção acontecer com sucesso. E, surpreendentemente, ele também retorna `true` se a propriedade a ser apagada for inexistente ou se a propriedade não pôde ser apagada (como as que sejam não configuráveis or não serem próprias do objeto).

Estes exemplos ilustram aqueles:

```javascript
var listaNatal = { mike: "livro", jason: "suéter" }
delete listaNatal.mike;		// apaga a propriedade mike

for (var pessoas in listaNatal) {
	console.log(pessoas);
}

// Imprime somente jason
// A propriedade mike foi removida

delete listaNatal.toString;		// retorna true, mas toString não foi apagado pois é um método herdado

// Aqui nós chamamos o método toString e ele funciona muito bem - não foi apagado
listaNatal.toString();		// "[object Object]"

/* Você pode apagar uma propriedade de uma instância se a propriedade for uma propriedade própria dela. 
Por exemplo, nós podemos apagar a propriedade nívelEnsino do objeto escola que criamos acima, 
pois a nívelEnsino é definido na instância: Nós usamos a palavra-chave "this" para definir 
a propriedade quando nós declaramos a função EnsinoSuperior. Nós não definimos a propriedade 
nívelEnsino no protótipo da função EnsinoSuperior */

console.log(escola.hasOwnProperty("nívelEnsino"));	// true

// nívelEnsino é uma propriedade própria de escola, então nós podemos removê-la
delete escola.nívelEnsino;		// true

// A propriedade nívelEnsino foi apagada da instância escola
console.log(escola.nívelEnsino);	// undefined

// Mas a propriedade nívelEnsino ainda continua na função EnsinoSuperior
var novaEscola = new EnsinoSuperior;
console.log(novaEscola.nívelEnsino);	// Universidade

// Se tivéssemos definidos uma propriedade no protótipo da função EnsinoSuperior,
// tal como esta propriedade nívelEnsino2:
EnsinoSuperior.prototype.nívelEnsino2 = "Universidade 2";

// Então a propriedade nívelEnsino2 nas instâncias de EnsinoSuperior
// não seriam propriedades próprias.

// A propriedade nívelEnsino2 não é uma propriedade própria da instância escola
console.log(escola.hasOwnProperty("nívelEnsino2"));	// false
console.log(escola.nívelEnsino2); // Universidade 2

// Vamos tentar apagar a propriedade herdada nívelEnsino2
delete escola.nívelEnsino2;	// true	(sempre retorna true, como falado anteriormente)

// A propriedade herdada nívelEnsino2 não foi removida
console.log(escola.nívelEnsino2);	// Universidade 2
```


### Serializar e Desserializar Objetos

Para transferir os seus objetos via HTTP ou doutra forma convertê-los para uma string, você necessitará de serializá-los (converter para string); você pode usar a função `JSON.stringify` para serializar os seus objetos. Note que antes do ECMAScript 5, você tinha que usar a popular biblioteca json2 (de Douglas Crockford) para obter a função `JSON.stringify`. Ela está agora padronizada/nativa no ECMAScript 5.	

Para desserializar o seu objeto (convertê-lo para um objeto a partir de uma string), você usa a função `JSON.parse` da mesma biblioteca json2. Esta função também está padronizada/nativa no ECMAScript 5.

Exemplos de `JSON.stringify`:

```javascript
var listaNatal = { mike: "livro", jason: "suéter", chelsea: "iPad" }
JSON.stringify(listaNatal);
// Imprime esta string
// "{"mike":"livro","jason":"suéter","chels":"iPad"}"

// Para imprimir o objeto stringificado com formatação, adicione null e 4 como parâmetros:
JSON.stringify(listaNatal, null, 4);
/* 
"{
    "mike": "livro",
    "jason": "suéter",
    "chelsea": "iPad"
}"
*/

/* Exemplos JSON.parse */

// A seguir temos uma string JSON, então não podemos acessar suas propriedades com 
// a notação de pontos (como listaNatal.mike)
var listaNatalString = '{"mike": "livro", "jason": "suéter", "chelsea": "iPad"}';

// Vamos convertê-lo para um objeto
var listaNatalObjeto = JSON.parse(listaNatalString);

// Agora que temos um objeto, vamos usar a notação com pontos
console.log(listaNatalObjeto.mike);	// livro
```


Para uma cobertura mais detalhada de Objetos de JavaScript, incluindo as adições na ECMAScript 5 para lidar com objetos, leia o capítulo 6 de O Guia Definitivo JavaScript 6ª edição.
