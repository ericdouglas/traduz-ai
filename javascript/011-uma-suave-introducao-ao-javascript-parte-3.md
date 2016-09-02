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

It is often useful to have the
