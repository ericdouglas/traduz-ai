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

This works great when we know
