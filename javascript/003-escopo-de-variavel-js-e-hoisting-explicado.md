# Escopo e _Hoisting_ de Variáveis no JavaScript Explicados

* **Artigo Original**: [JavaScript Variable Scope and Hoisting Explained](http://javascriptissexy.com/javascript-variable-scope-and-hoisting-explained/)
* **Tradução**: [Eric Douglas](https://github.com/ericdouglas)

Nesta postagem, iremos aprender sobre escopo e hoisting (hasteamento) de variáveis no JavaScript e tudo sobre as idiossincrasias (peculiaridades) de ambos.

É imperativo que nós tenhamos entendimento de como o escopo de variável e o hasteamento de variável funcionam no JavaScript, se quisermos entender bem o JS. Estes conceitos podem parecer óbvios, mas não são. Há algumas importantes sutilezas que nós devemos compreender, se realmente quisermos ser desenvolvedores JavaScript bem sucedidos.

##Escopo de Variáveis
	
Um escopo de variável é o contexto no qual a variável existe. O escopo especifica de onde você pode acessar uma variável e se você tem acesso à variável naquele contexto.

Variáveis têm ou um escopo local ou um escopo global.

##Variáveis Locais (Escopo Nível-de-Função)
	
Ao contrário da maioria das linguagens de programação, o JavaScript não tem um escopo em nível-de-bloco (variáveis com o escopo envolto por chaves); como alternativa, no JavaScript temos escopo por nível-de-função. As variáveis declaradas dentro de uma função são variáveis locais, e são somente acessíveis por dentro dessa função ou por funções dentro dessa função. Veja a minha postagem de [Closures](https://github.com/cerebrobr/traduz-ai/blob/master/javascript/004-entenda-closures-no-javaScript-com-facilidade.md) para saber mais em como acessar variáveis em funções exteriores a partir de funções interiores.

Demonstração de Escopo de Nível-de-Função

```javascript
	var name = "Richard";

	function showName() {
		var name = "Jack"; // variável local; somente acessível na função showName
		console.log(name); // Jack
	}

	console.log(name); // Richard: a variável global
```

Sem Escopo de Nível-de-Bloco

```javascript
	var name = "Richard";
	// O bloco nesta condicional if não cria um contexto local para a variável name
	if (name) {
		name = "Jack"; // este name é a variável global name e está sendo alterada para "Jack" aqui
		console.log(name);	// Jack: ainda como a variável global
	}

	// Aqui, a variável name é a mesma variável global name, mas ela foi modificada na condicional if
	console.log(name); // Jack
```

### Se você não declarar as suas variáveis locais, encrenca está perto

Sempre declare suas variáveis locais antes de usá-las. Aliás, você deveria usar o [JSHint](http://www.jshint.com/) para verificar seu código por erros de sintaxe e guias de estilo. Aqui está o problema ao não declarar variáveis locais:

```javascript
// Se você não declarar suas variáveis locais com a palavra-chave var, 
// elas se tornam parte do escopo global
var name = "Michael Jackson";

function showCelebrityName() {
	console.log(name);
}

function showOrdinaryPersonName() {
	// Note a ausência da palavra-chave var, 
	// tornando esta variável global:
	name = "Johnny Evers";
	console.log(name);
}

showCelebrityName(); // Michael Jackson

// name não é uma variável local, ele simplesmente muda a variável global name
showOrdinaryPersonName(); // Johnny Evers

// A variável global é agora Johnny Evers, não mais o name da celebridade Michael Jackson
showCelebrityName(); // Johnny Evers

// A solução é declarar sua variável local com a palavra-chave var
function showOrdinaryPersonName() {
	// Agora name é sempre uma variável local
	// e não irá sobrescrever a variável global
	var name = "Johnny Evers";
	console.log(name);
}
```

### Variáveis locais têm prioridade sobre variáveis globais nas funções

Se você declarar uma variável global e uma variável local com o mesmo nome, a variável local terá prioridade quando você tentar usá-la dentro de uma função (escopo local):

```javascript
	var name = "Paul";

	function users() {
		// Aqui, a variável name é local
		// e prevalece sobre a mesma variável name no escopo global
		var name = "Jack";

		// A busca por name começa bem aqui dentro da função
		// antes de tentar enxergar fora da função no escopo global
		console.log(name);
	}

	users(); // Jack
```

## Variáveis Globais

Todas as variáveis declaradas fora de uma função estão no escopo global. No navegador, que é onde estamos interessados como desenvolvedores front-end, o contexto ou escopo global é o objeto _window_ (ou o documento HTML inteiro).

- Qualquer variável declarada ou inicializada fora de uma função é uma variável global, e estará portanto disponível para toda a aplicação. Por exemplo:

```javascript
// Para declarar uma variável global, você pode utilizar quaisquer das seguintes estratégias:
var myName = "Richard";

// ou mesmo
firstName = "Richard";

// ou
var name;
name;
```

É importante notar que todas as variáveis globais são anexadas no objeto _window_. Então, todas as variáveis globais que nós declaramos podem ser acessadas pelo object _window_ como assim:

```javascript
console.log(window.myName); // Richard;

// ou
console.log("myName" in window); // true
console.log("firstName" in window); // true
```

- Se uma variável é inicializada (atribuída com um valor) sem primeiro ser declarada com a palavra-chave var, ela é automaticamente adicionada ao contexto global, sendo deste modo uma variável global:

```javascript
function showAge() {
	// age é uma variável global porque ela não foi declarada
	// com a palavra-chave var dentro da função:
	age = 90;
	console.log(age); 
}

// age está no contexto global, portanto está disponível aqui também:
console.log(age); // 90
```

Demonstração de variáveis que estão no Escopo Global mesmo que pareça o contrário:

```javascript
// Ambas variáveis firstName estão no escopo global,
// mesmo embora a segunda esteja envolta pelo bloco {}:
var firstName = "Richard";
{
	var firstName = "Bob";
}

// Para reiterar: JavaScript não tem escopo por nível-de-bloco

// A segunda declaração de firstName simplesmente redeclara e sobrescreve a primeira
console.log(firstName); // Bob
```

Outro exemplo:

```javascript
for (var i = 1; i <= 10; ++i) {
	console.log(i); // imprime 1, 2, 3, 4, 5, 6, 7, 8, 9, 10
}

// A variável i é uma variável global e está acessível
// na seguinte função com o último valor que foi-lhe atribuído acima
function aNumber() {
	console.log(i);
}

// A variável i na função aNumber abaixo é a variável global i que foi alterada no laço acima.
// Seu último valor foi 11, atribuído um momento antes da saída do laço:
aNumber(); // 11
```

### Variáveis de setTimeout são Executadas no Escopo Global
Note que todas as funções no setTimeout são executadas no escopo global. Isto é ligeiramente complicado; considere isto:

```javascript
// O uso do objeto "this" dentro da função setTimeout refere-se ao objeto Window, não ao myObj

var highValue = 200;
var constantVal = 2;
var myObj = {
	highValue: 20,
	constantVal: 5,
	calculateIt: function () {
		setTimeout (function () {
			console.log(this.constantVal * this.highValue);
		}, 2000);
	}
};

// O objeto "this" na função setTimeout usou as variáveis globais highValue e constantVal,
// porque a referência ao "this" na função setTimeout refere-se ao objeto global window,
// não ao objeto myObj como anteciparíamos.

myObj.calculateIt(); // 400

// Este é um ponto importante a ser lembrado!
```

### Não Polua o Escopo Global

Se você quer se tornar um mestre em JavaScript, o que certamente você quer ser (caso contrário você estaria assistindo 'Honey Boo Boo' neste exato momento), você tem que saber que é importante evitar criar muitas variáveis no escopo global, tal como este:

```javascript
// Estas duas variáveis estão no escopo global e elas não deveriam estar aqui:
var firstName, lastName;

function fullName() {
	console.log("Full name: " + firstName + " " + lastName);
}
```

Este é o código melhorado e a maneira apropriada de se evitar poluir o escopo global:

```javascript
// Declare as variáveis dentro da função onde serão variáveis locais:
function fullName() {
	var firstName = "Michael", lastName = "Jackson";

	console.log("Full Name: " + firstName + " " + lastName);
}
```

Neste último exemplo, a função fullName está no escopo global.

## Hasteamento de Variáveis (Variable Hoisting)

Todas declarações de variáveis são hasteadas (erguidas e declaradas) ao topo da função se definida dentro duma função; ou no topo do contexto global se definida fora duma função.

É importante saber que somente declarações de variáveis são hasteadas ao topo, não a inicialização ou as atribuições (quando para uma variável é atribuído um valor) de variáveis.

Exemplo de Hasteamento de Variável:

```javascript
function showName() {
	console.log("First Name: " + name);
	var name = "Ford";
	console.log("Last Name: " + name);
}

showName ();
// First Name: undefined
// Last Name: Ford

// A razão para a primeira impressão ser undefined é que a variável local name foi hasteada ao topo da função
// Que significa que é esta a variável local que é chamada na primeira vez.
// Veja como o código realmente é processado pela engine (motor) JavaScript

function showName() {
	// name é hasteada (note que está indefinida (undefined) até este ponto,
	// já que a atribuição acontece depois mais abaixo):
	var name;
	console.log("First Name: " + name); // First Name: undefined

	name = "Ford"; // é atribuído um valor a name

	// Agora name é Ford
	console.log("Last Name: " + name); // Last Name: Ford
}
```

### Declaração de Função Sobrescreve Declaração de Variável Quando Hasteada

Ambas declarações de funções e variáveis são hasteadas para o topo do escopo que as contém. E a declaração de função tem prioridade sobre declarações de variável (mas não sobre atribuição de variável). Como notado acima, atribuição de variável não é hasteada, e nem atribuição de função. Como um lembrete, isto é uma atribuição de função: `var myFunction = function () {};`.
Aqui temos um exemplo básico para demonstração:

```javascript
// Tanto a variável quanto a função são nomeadas de myName:
var myName;

function myName() {
	console.log("Rich");
}

// A declaração da função sobrescreve a variável myName:
console.log(typeof myName); // function
```

```javascript
// Mas neste exemplo, a atribuição da variável sobrescreve a declaração da função

// Isso é a atribuição de variável (inicialização) que sobrescreve a declaração da função:
var myName = "Richard";

function myName() {
	console.log("Rich");
}

console.log(typeof myName); // string
```

É importante notar que as expressões função, tais como o exemplo abaixo, não são hasteadas:

```javascript
var myName = function() {
	console.log("Rich");
};
```

No modo estrito (`"strict mode";`), um erro ocorrerá se você atribuir a uma variável algum valor sem primeiro declarar a variável. Sempre declare as suas variáveis!

Sê bom. Durma bem. E curta programar!
