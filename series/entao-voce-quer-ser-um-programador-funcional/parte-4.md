# Então você quer ser um Programador Funcional? (Parte 4)

* **Artigo original**: [So You Want to be a Functional Programmer (Part 4)](https://medium.com/@cscalfani/so-you-want-to-be-a-functional-programmer-part-4-18fbe3ea9e49#.ybnyvxuyi)
* **Tradução**: [Gabriel](https://github.com/gabriel-ribeiro-ir)

Dar o primeiro passo para entender os conceitos de Programação Funcional é o mais importante e algumas vezes o passo mais díficil. Mas isto não tem de ser assim. Não com a perspectiva correta.

Parte anterior:

[Parte 3](parte-3.md) (em português)
[Parte 3](https://medium.com/@cscalfani/so-you-want-to-be-a-functional-programmer-part-3-1b0fd14eb1a7#.zffq7cklj) (em inglês)

### Currying
Se você se lembra da [Parte 3](parte-3.md), a razão pela qual tivemos problemas compondo **mult5** e **add** (in) é porque **mult5** recebia 1 parâmetro e **add** recebia 2.

Podemos resolver este problema facilmente restringindo todas as funções a receber somente 1 parâmetro.

Acredite em mim, isto não é tão ruim quanto parece.

Simplesmente escrevemos uma função add que usa 2 parâmetros mas recebe somente 1 por vez. **Curried functions** nos permite fazer isto.

> Uma Curried function é uma função que recebe somente 1 parâmetro por vez.

Isto vai permitir que passemos para **add** seu primeiro parâmetro antes de compormos ela com **mult5**.
Então quando **mult5AfterAdd10** é chamada, **add** receberá seu segundo parâmetro.

Em Javascript, podemos realizar isto reescrevendo *add**:

`var add = x => y => x + y`

Esta versão de *add** é uma função que recebe um parâmetro agora e então recebe outro depois.

Em detalhes, a função *add** recebe um único parâmetro **x**, e retorna uma **função** que recebe somente 1 parâmetro **y**, que acabará retornando o **resultado da adição entre x e y**.

Agora podemos usar esta versão de **add** para construir uma versão funcional de **mult5AfterAdd10**:

```
var compose = (f, g) => x => f(g(x));
var mult5AfterAdd10 = compose(mult5, add(10));
```

A função *compose* recebe 2 parâmetros, **f** e **g**. Então retorna uma função que recebe 1 parâmetro, **x**, que quando chamada aplicará **f** após **g** de **x**.

Então, o que fizemos exatamente? Bem, nós convertemos nossa antiga função **add** simples em uma versão *curried*. isto torna **add** mais flexível agora que o primeiro parâmetro, 10, pode ser passado na frente e o parâmetro final será passado quando **mult5AfterAdd10** é chamada.

Neste ponto, você deve estar pensando em como reescrever a função add em Elm. Acontece que você não precisa. Em Elm e outras linguagens funcionais, toda função é automaticamente *curried*.

Então a função **add** continua a mesma:

```
add x y =
    x + y
```

É assim que **mult5AfterAdd10** deveria ter sido escrito na [Parte 3](https://medium.com/@cscalfani/so-you-want-to-be-a-functional-programmer-part-3-1b0fd14eb1a7#.zffq7cklj):

```
mult5AfterAdd10 =
    (mult5 << add 10)
```

Sintaticamente falando, Elm supera linguagens imperativas como Javascript porque é otimizada para coisas funcionais como currying e composição.

### Currying e Refatoração

Outro ponto em que Currying brilha é durante a refatoração, quando criamos uma versão genérica de uma função com um monte de parâmetros e então a usamos para criar versões específicas com menos parâmetros.

Por exemplo, quando temos as seguintes funções que colocam colchetes e colchetes duplos ao redor de strings:

```
bracket str =
    "{" ++ str ++ "}"

doubleBracket str =
    "{{" ++ str ++ "}}"
```

Aqui está como utilizamos:

```
bracketedJoe =
    bracket "Joe"

doubleBracketedJoe =
    doubleBracket "Joe"
```

Nós podemos generalizar **bracket** e **doubleBracket**:

```
generalBracket prefix str suffix =
    prefix ++ str ++ suffix
```

Mas agora toda vez que usarmos **generalBracket** temos que passar os colchetes:

```
bracketedJoe =
    generalBracket "{" "Joe" "}"

doubleBracketedJoe =
    generalBracket "{{" "Joe" "}}"
```

O que realmente queremos é o melhor dos dois mundos.

Se nós reordenarmos os parâmetros de **generalBracket**, podemos criar **bracket** e **doubleBracket** aproveitando o fato de que as funções são curried:

```
generalBracket prefix suffix str =
    prefix ++ str ++ suffix

bracket =
    generalBracket "{" "}"

doubleBracket =
    generalBracket "{{" "}}"
```

Note que colocando os parâmetros que eram mais suscetíveis de serem estáticos, por exemplo **prefix** e **suffix**, e colocando os parâmetros mais propensos a mudar por último, por exemplo **str**, podemos facilmente criar versões especializadas de **generalBracket**.

> A ordem dos parâmetros é importante para aproveitar completamente o currying.

Note também que **bracket** e **doubleBracket** são escritos em notação point-free, por exemplo o parâmetro **str** é implícito. Ambos **bracket** e **doubleBracket** são funções esperando pelo seu parâmetro final.

Agora podemos usá-las como antes:

```
bracketedJoe =
    bracket "Joe"

doubleBracketedJoe =
    doubleBracket "Joe"
```

Mas desta vez estamos usando uma função *curried* genérica, **generalBracket**.

### Funções funcionais comuns

Vamos dar uma olhada em 3 funções comuns que são usadas em linguagens funcionais.

Mas primeiro, vejamos o seguinte código Javascript:

```
for (var i = 0; i < something.length; ++i) {
    // do stuff
}
```

Existe um problema com este código. Não é um bug. O problema é que este código é um boilerplate, isto é, código que é escrito de novo e de novo.

Se você programa em Linguagens imperativas como, Java, C#, Javascript, PHP, Python, etc., você se verá escrevendo este boilerplate mais que qualquer um.

É isso que está errado nele.

Então vamos matá-lo. Vamos colocá-lo em uma função (ou em um par de funções) e nunca mais escrever um loop for novamente. Bem, quase nunca. Pelo menos até mudarmos para uma linguagem funcional.

Vamos começar modificando um Array chamado **things**:

```
var things = [1, 2, 3, 4];
for (var i = 0; i < things.length; ++i) {
    things[i] = things[i] * 10; // MUTATION ALERT !!!!
}
console.log(things); // [10, 20, 30, 40]
```

UGH!! Mutabilidade!

Vamos tentar novamente. Desta vez não vamos mudar **things**:

```
var things = [1, 2, 3, 4];
var newThings = [];
for (var i = 0; i < things.length; ++i) {
    newThings[i] = things[i] * 10;
}
console.log(newThings); // [10, 20, 30, 40]
```

Beleza, não modificamos **things** mas tecnicamente nós mudamos **newThings**. Por enquanto, deixaremos isto passar, afinal de contas estamos em Javascript. Uma vez que mudarmos para uma Linguagem Funcional, nós não vamos conseguir modificar.

O objetivo aqui é entender como estas funções funcionam e nos ajudam a "reduzir barulho" em nosso código.

Vamos pegar este código e colocá-lo em uma função. Nós vamos chamar nossa primeira função comum de **map**, agora que ela mapeia cada valor no array antigo para novos valores no novo array.

```
var map = (f, array) => {
    var newArray = [];
    for (var i = 0; i < array.length; ++i) {
        newArray[i] = f(array[i]);
    }
    return newArray;
};
```

Perceba que passamos como parâmetro uma função **f**, para que nosso **map** possa fazer qualquer coisa que quisermos com cada item do **array**.

Agora podemos reescrever nosso código anterior para usar o **map**:

```
var things = [1, 2, 3, 4];
var newThings = map(v => v * 10, things);
```

Olha mãe. Sem loops for. E muito mais fácil de ler e portanto, descobrir sobre o que é.

Bem, tecnicamente existem loops for na função **map**. Mas pelo menos nós não temos mais que escrever aquele código boilerplate.

Agora vamos escrever outra função comum para **filtrar** coisas de um array:

```
var filter = (pred, array) => {
    var newArray = [];
for (var i = 0; i < array.length; ++i) {
        if (pred(array[i]))
            newArray[newArray.length] = array[i];
    }
    return newArray;
};
```

Note como a função pressuposta **pred** retorna TRUE se mantivermos o item ou FALSE se o removermos.

Aqui está como usamos **filter** para filtrar números ímpares:

```
var isOdd = x => x % 2 !== 0;
var numbers = [1, 2, 3, 4, 5];
var oddNumbers = filter(isOdd, numbers);
console.log(oddNumbers); // [1, 3, 5]
```

Utilizar nossa nova função **filter** é tão mais simples que codificar na mão com um loop for.

A função comum final é chamada de **reduce**. Tipicamente é usada para receber uma lista e reduzí-la a um único valor mas ela pode na verdade fazer muito mais.

Esta função é comumente chamada de **fold** em Linguagens Funcionais.

```
var reduce = (f, start, array) => {
    var acc = start;
    for (var i = 0; i < array.length; ++i)
        acc = f(array[i], acc); // f() takes 2 parameters
    return acc;
});
```

A função **reduce** recebe uma função de redução, **f**, um valor **start** inicial e um **array**.

Perceba que a função de redução, **f**, aceita 2 parâmetros, o item atual do **array**, e o acumulador, **acc**. A função vai usar estes parâmetros para produzir um novo acumulador a cada interação. O acumulador da interação final é retornado.

Um exemplo vai nos ajudar a entender como isto funciona:

```
var add = (x, y) => x + y;
var values = [1, 2, 3, 4, 5];
var sumOfValues = reduce(add, 0, values);
console.log(sumOfValues); // 15
```

Veja que a função **add** soma seus 2 parâmetros recebidos. Nossa função **reduce** espera uma função que recebe 2 parâmetros então elas funcionarão bem juntas.

Começamos com um valor **start** de zero e passamos nosso array, **values**, para ser somado. Dentro da função **reduce**, a soma é acumulada à medida que intera sobre os valores. O valor final acumulado é retornado como **sumOfValues**.

Cada uma destas funções, **map**, **filter** e **reduce** nos permite fazer operações de manipulação comuns em arrays sem precisar escrever boilerplate de loops for.

Mas em Linguagens Funcionais, elas são ainda mais úteis uma vez que não existem construtores de loop, somente recursão. Funções de iteração não são somente extremamente úteis. Elas são necessárias.

### Meu cérebro!!!

Por enquanto chega.

Nas próximas partes deste artigo, eu vou falar sobre Integridade referencial, ordem de execução, tipos e mais.

A seguir:

[Parte 5](https://medium.com/@cscalfani/so-you-want-to-be-a-functional-programmer-part-5-c70adc9cf56a#.nyj0fmsk3) (em inglês)
