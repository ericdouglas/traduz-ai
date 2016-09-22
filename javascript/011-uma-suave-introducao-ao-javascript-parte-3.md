# Uma Suave Introdução ao JavaScript Funcional: Parte 3

* **Artigo Original**: [A GENTLE INTRODUCTION TO FUNCTIONAL JAVASCRIPT: PART 3](http://jrsinclair.com/articles/2016/gentle-introduction-to-functional-javascript-functions/)
* **Tradução**: [Eric Douglas](https://github.com/ericdouglas)

> *Escrito por James Sinclair em 6 de Fevereiro de 2016* 

Essa é a parte 3 de uma série de 4 artigos introduzindo a programação funcional no JavaScript. No último artigo vimos como podemos usar a programação funcional com listas e arrays. Neste artigo iremos examinar *high-order functions* (funções de ordem superior) - funções para fazer funções.

- [Parte 1: Blocos fundamentais e motivação](009-uma-suave-introducao-ao-javascript-parte-1.md)
- [Parte 2: Trabalhando com Arrays e Listas](010-uma-suave-introducao-ao-javascript-parte-2.md)
- Parte 3: Funções para fazer funções (esse artigo)
- Parte 4: Fazendo isso com estilo

## Funções Para Fazer Funções
No fim do último artigo, eu disse que ir a fundo no caminho funcional não é para todo mundo. Isso é porque além das funções que processam listas, as coisas começam a ficar um pouco estranhas. O que quero dizer é que começamos abstraindo coleções de instruções em funções. Então, começamos a abstrair loops `for` em `map` e `reduce`. O próximo nível de abstração é começar a refatorar padrões de *criação* de funções. Começamos a criar funções para fazer outras funções. Isso pode ser poderoso e elegante, mas começa a se parecer bem menos com o JavaScript que você está acostumado a escrever.

## Mais Blocos Fundamentais
Funções para fazer outras funções algumas vezes são chamadas *high order functions* (funções de ordem superior). Porém para entendê-las, precisamos revisitar algumas funcionalidades nativas do JavaScript que tornam as funções de ordem superior possíveis.

### *Closures* e Escopo
Uma das coisas mais difíceis para alguém entender no JavaScript é quais variáveis uma função pode "ver". No JavaScript, se você definir uma variável dentro de uma função, ela poderá ser vista fora da função. Por exemplo:

```js
var thing = 'bat';
    
var sing = function() {
    // This function can 'see' thing
    var line = 'Twinkle, twinkle, little ' + thing;
    log(line);
};

sing();
// Twinkle, twinkle, little bat

// Outside the function we can't see message though
log(line);
// undefined
```

Entretando, se nós definirmos uma função dentro de uma função, a função interna pode ver as variáveis da função exterior:

```js
var outer = function() {
    var outerVar = 'Hatter';
    var inner = function() {
         // We can 'see' outerVar here
         console.log(outerVar);
         // Hatter
         
         var innerVar = 'Dormouse';
         // innerVar is only visible here inside inner()
    }
    
    // innerVar is not visible here.
}
```

Isso leva um tempo para se acostumar. As regras são bem simples, mas uma vez que comecemos a passar variáveis como argumentos, se torna mais difícil rastrear quais funções podem ver quais variáveis. Se isso está inicialmente confuso, seja paciente: Veja o ponto onde você definiu a função e descubra quais variáveis são "visíveis" nesse ponto. Elas podem não ser o que você espera se você apenas olhar no ponto onde você está chamando a função.

### A Variável Especial `arguments`
Quando você cria uma função no JavaScript, ele cria uma variável especial chamada `arguments`, que é *parecida* com um array. Ela contém os argumentos que são passados à função. Por exemplo:

```js
var showArgs = function(a, b) {
    console.log(arguments);
}
showArgs('Tweedledee', 'Tweedledum');
//=> { '0': 'Tweedledee', '1': 'Tweedledum' }
```

Note que a saída é mais como um objeto com chaves que são inteiros do que um verdadeiro array.

Algo interessante sobre `arguments` é que essa variável contém *todos* os argumentos passados na chamada da função, independentemente de quantos foram definidos. Então se você chamar uma função passando argumentos a mais, eles estarão disponíveis na variável `arguments`.

```js
showArgs('a', 'l', 'i', 'c', 'e');
//=> { '0': 'a', '1': 'l', '2': 'i', '3': 'c', '4': 'e' }
```

A variável `arguments` também tem uma propriedade `length`, assim como um array:

```js
var argsLen = function() {
    console.log(arguments.length);
}
argsLen('a', 'l', 'i', 'c', 'e');
//=> 5
```

É geralmente útil ter a variável `arguments` como um array verdadeiro. Nesses casos nós podemos converter a variável `arguments` em um array usando o método nativo de array chamado `slice`. Pelo fato de `arguments` não ser um array de verdade, temos que fazer isso por um caminho alternativo:

```js
var showArgsAsArray = function() {
    var args = Array.prototype.slice.call(arguments, 0);
    console.log(args);
}
showArgsAsArray('Tweedledee', 'Tweedledum');
//=> [ 'Tweedledee', 'Tweedledum' ]
```

A variável `arguments` é normalmente utilizada para criar funções que podem pegar um número variável de argumentos. Isso vai ser útil depois, como iremos ver.

### *Call* e *Apply*
Vimos antes que arrays no JavaScript têm alguns métodos nativos como `.map` e `.reduce`. Bom, funções também têm alguns métodos nativos.

A forma comum de chamar uma função é escrevendo parênteses, e qualquer parâmetro depois do nome da função. Por exemplo:

```js
function twinkleTwinkle(thing) {
    console.log('Twinkle, twinkle, little ' + thing);
}
twinkleTwinkle('bat');
//=> Twinkle, twinkle, little bat
```

Um dos métodos nativos das funções é o `call` e ele permite que você chame uma função de outra forma:

```js
twinkleTwinkle.call(null, 'star');
//=> Twinkle, twinkle, little star
```

O primeiro argumento do método `.call` define a quê a variável especial `this` vai se referir dentro da função. Podemos ignorar isso por agora. Quaisquer argumentos depois deste serão passados diretamente para a função.

O método `.apply` é bem parecido com `.call`, exceto que ao invés de passar argumentos individuais um por um, `.apply` permite que você passe um array de argumentos como o segundo parâmetro. Por exemplo:

```js
twinkleTwinkle.apply(null, ['bat']);
//=> Twinkle, twinkle, little bat
```

Ambos esses métodos vão ser úteis quando nós estivermos criando funções que criam outras funções.

### Funções Anônimas
O JavaScript nos permite criar funções em tempo de execução. Onde quer que tenhamos a possibilidade de criar uma variável, e então fazer algo com essa variável, o JavaScript nos permite adicionar uma definição de função nesse local. Isso é frequentemente usado com `map` e `reduce`, por exemplo:

```js
var numbers = [1, 2, 3];
var doubledArray = map(function(x) { return x * 2}, numbers);
console.log(doubledArray);
//=> [ 2, 4, 6 ]
```

Funções como essa criadas em tempo de execução são chamadas funções anônimas, uma vez que elas não tem um nome. Elas também são chamadas as vezes de funções lambda.

## Aplicação Parcial
Algumas vezes pode ser útil preencher previamente os argumentos de uma função. Por exemplo, imagine que tenhamos feito uma função `addClass()` que pega um nome de classe e um elemento DOM como parâmetros:

```js
var addClass = function(className, element) {
    element.className += ' ' + className;
    return element;
}
```

Gostaríamos de usar isso com `map` para adicionar uma classe a vários elementos, mas temos um problema: `map` passa itens do array um por um como o primeiro parâmetro para a função *callback*. Então como vamos dizer a `addClass` qual nome de classe adicionar?

A solução é criar uma nova função que chama `addClass` com o nome da classe que queremos:

```js
var addTweedleClass = function(el) {
    return addClass('tweedle', el);
}
```

Agora temos uma função que recebe apenas um parâmetro. Agora ela está adequada para ser passada para a função `map`:

```js
var ids = ['DEE', 'DUM'];
var elements = map(document.getElementById, ids);
elements = map(addTweedleClass, elements);
```

Mas se quisermos passar outra classe, temos que criar outra função:

```js
var addBoyClass = function(el) {
    return addClass('boy', el);
}
```

Nós começamos a nos repetir... então, vamos ver se podemos encontrar alguma abstração para esse padrão. E se tivermos uma função que cria outra função com o primeiro parâmetro preenchido previamente?

```js
var partialFirstOfTwo = function(fn, param1) {
    return function(param2) {
        return fn(param1, param2);
    }
}
```

Note a primeira declaração `return`. Nós criamos uma função que retorna outra função.

```js
var addTweedleClass = partialFirstOfTwo(addClass, 'tweedle');
var addBoyClass = partialFirstOfTwo(addClass, 'boy');

var ids = ['DEE', 'DUM'];
var elements = map(document.getElementById, ids);
elements = map(addTweedleClass, elements);
elements = map(addBoyClass, elements);
```

Isso funciona bem quando nós sabemos que nossa função recebe exatamente dois parâmetros. Mas e se quisermos fazer a aplicação parcial em uma função que recebe mais de uma variável? Para esses casos precisamos de uma função de aplicação parcial mais generalisada. Vamos fazer uso dos métodos `slice` e `apply` descritos acima:

```js
var argsToArray = function(args) {
    return Array.prototype.slice.call(args, 0);
}

var partial = function() {
    // Convert the arguments variable to an array 
    var args = argsToArray(arguments);

    // Grab the function (the first argument). args now contains the remaining args.
    var fn = args.shift();

    // Return a function that calls fn
    return function() {
        var remainingArgs = argsToArray(arguments);
        return fn.apply(this, args.concat(remainingArgs));
    }
}
```

Agora, os detalhes de *como* essa função funciona não é tão importante como *o que* ela faz. Essa função nos permite aplicar parcialmente qualquer número de variáveis a funções que recebem qualquer número de parâmetros.

```js
var twinkle = function(noun, wonderAbout) {
    return 'Twinkle, twinkle, little ' +
        noun + '\nHow I wonder where you ' +
        wonderAbout;
}

var twinkleBat = partial(twinkle, 'bat', 'are at');
var twinkleStar = partial(twinkle, 'star', 'are');

console.log(twinkleBat());
// "Twinkle, twinkle, little bat
// How I wonder where you are at"

console.log(twinkleStar());
// "Twinkle, twinkle, little star
// How I wonder where you are"
```

O JavaScript tem um método nativo que meio que funciona como `partial` chamado `bind`. Ele está disponível como um método em todas as funções. O problema é que ele espera que seu primeiro parâmetro seja um objeto que você deseja vincular (*bind*) a variável especial `this`. Isso significa, por exemplo, se você quer aplicar parcialmente algo a `document.getElementById`, você tem que passar `document` como o primeiro parâmetro, dessa forma:

```js
var getWhiteRabbit = document.getElementById.bind(document, 'white-rabbit');
var rabbit = getWhiteRabbit();
```

Na maior parte do tempo porém, nós não precisamos da variável especial `this` (especialmente se estivermos usando um estilo funcional de programação), então podemos apenas passar `null` como o primeiro parâmetro. Por exemplo:

```js
var twinkleBat = twinkle.bind(null, 'bat', 'are at');
var twinkleStar = twinkle.bind(null, 'star', 'are');
```

Você pode ler mais sobre [`.bind` na referência JavaScript do MDN](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_objects/Function/bind).

### Composição
Falamos no último artigo que programação funcional está relacionada a pegar pequenas, simples funções e uní-las para fazer coisas mais complexas. Aplicação parcial, como vimos acima, é uma ferramenta que torna esse processo mais fácil. Com aplicação parcial nós podemos converter nossa função `addClass` em uma função que podemos usar com `map`. Composição é uma outra ferramenta para combinar simples funções.

A forma mais simples de composição é com duas funções, `a` e `b`, ambas esperando apenas um parâmetro. `Compose` cria uma terceira função, `c`. Chamando `c` com um parâmetro `x` retorna o resultado de chamar `a` com o resultado de chamar `b` com `x`... Que confusão. É muito mais simples de entender vendo um exemplo:

```js
var composeTwo = function(funcA, funcB) {
    return function(x) {
        return funcA(funcB(x));
    }
}

var nohow = function(sentence) {
    return sentence + ', nohow!';
}
var contrariwise = function(sentence) {
    return sentence + ' Contrariwise…';
}

var statement = 'Not nothin&amp;rsquo;';
var nohowContrariwise = composeTwo(contrariwise, nohow);
console.log(nohowContrariwise(statement));
//=> Not nothin&amp;rsquo;, nohow! Contrariwise…
```

Isso é muito bom. Podemos fazer muita coisa apenas com `composeTwo`. Mas, se você começar a escrever funções "puras" (vamos discutir isso depois), então você pode encontrar-se precisando juntar mais que duas funções. Para isso iremos precisar de uma função `compose` mais genérica:

```js
var compose = function() {
    var args = arguments;
    var start = args.length - 1;
    return function() {
        var i = start;
        var result = args[start].apply(this, arguments);
        i = i - 1;
        while (i >= 0) {
            result = args[i].call(this, result);
            i = i - 1;
        }
        return result;
    };
};
```

Novamente, *como* isso funciona não é tão importante quanto *o que* você pode fazer com isso. E à primeira vista, `compose` pode não parecer tão incrível. Podemos escrever a função acima dessa forma com `compose`:

```js
var nohowContrariwise = compose(contrariwise, nohow);
```

Mas isso não parece muito mais conciso do que escrever desta forma:

```js
var nohowContrariwise = function(x) {
    return nohow(contrariwise(x));
}
```

O real poder da composição se torna claro uma vez que a combinamos com a função `curry`. Mesmo sem o *currying* nós podemos ver que se tivermos uma coleção de pequenas funções utilitárias, nós podemos usar `compose` para fazer nosso código mais claro e mais conciso. Por exemplo, imagine que temos um poema em texto simples:

```js
var poem = 'Twas brillig, and the slithy toves\n' + 
    'Did gyre and gimble in the wabe;\n' +
    'All mimsy were the borogoves,\n' +
    'And the mome raths outgrabe.';
```

Esse poema não vai ser mostrado muito bem no navegador, então vamos adicionar algumas quebras de linha. E, enquanto estivermos ai, vamos traduzir *brillig* para algo mais fácil de entender. E então vamos envolver tudo em uma tag parágrafo e em um bloco de citação. Vamos começar criando duas funções bem simples, e construir o resto a partir disso:

```js
var replace = function(find, replacement, str) {
    return str.replace(find, replacement);
}

var wrapWith = function(tag, str) {
    return '<' + tag + '>' + str + '</' + tag + '>'; 
}

var addBreaks      = partial(replace, '\n', '<br/>\n');
var replaceBrillig = partial(replace, 'brillig', 'four o’clock in the afternoon');
var wrapP          = partial(wrapWith, 'p');
var wrapBlockquote = partial(wrapWith, 'blockquote');

var modifyPoem = compose(wrapBlockquote, wrapP, addBreaks, replaceBrillig);

console.log(modifyPoem(poem));
//=> <blockquote><p>Twas four o’clock in the afternoon, and the slithy toves<br/>
//   Did gyre and gimble in the wabe;<br/>
//   All mimsy were the borogoves,<br/>
//   And the mome raths outgrabe.</p></blockquote>
```

Note que se você ler os argumentos passados para `compose` da esquerda para direita, eles estão na ordem inversa que eles são aplicados. Isso é por causa que `compose` reflete a ordem que eles estaria se tivessem sido escritos como chamadas de funções aninhadas. Algumas pessoas acham isso um pouco confuso, então a maioria das bibliotecas auxiliares fornecem uma forma reversa chamada `pipe` ou `flow`.

Usando uma função `pipe`, poderíamos escrever nossa função `modifyPoem` da seguinte maneira:

```js
var modifyPoem = pipe(replaceBrillig, addBreaks, wrapP, wrapBlockquote);
```

### Currying
Uma limitação de `compose` é que essa função espera que todas as funções passadas recebam apenas um parâmetro. Isso não é um grande problema agora que temos uma função `partial` - podemos converter nossas funções multi-parâmetros para funções que recebem um parâmetro com certa facilidade. Mas isso é um pouco tedioso. `Currying` é como uma aplicação parcial "turbinada".

Os detalhes da função `curry` são um pouco complicados, então vamos ver um exemplo primeiro. Temos uma função `formatName` que coloca o apelido de uma pessoa entre aspas. Essa função recebe três parâmetros. Quando nós chamamos a versão *curried* de `formatName` com menos que três parâmetros, ela retorna uma nova função com os parâmetros passados parcialmente aplicados:

```js
var formatName = function(first, surname, nickname) {
    return first + ' “' + nickname + '” ' + surname;
}
var formatNameCurried = curry(formatName);

var james = formatNameCurried('James');

console.log(james('Sinclair', 'Mad Hatter'));
//=> James “Mad Hatter” Sinclair

var jamesS = james('Sinclair')

console.log(jamesS('Dormouse'));
//=> James “Dormouse” Sinclair

console.log(jamesS('Bandersnatch'));
//=> James “Bandersnatch” Sinclair
```

There’s some other things to notice about curried functions
