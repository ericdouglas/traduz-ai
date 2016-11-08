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



### Meu cérebro!!!

Por enquanto chega.

Nas próximas partes deste artigo, eu vou falar sobre Currying, funções funcionais comuns (ex: map, filter, fold etc.), Transparência Referencial e mais.

A seguir:

[Parte 5](https://medium.com/@cscalfani/so-you-want-to-be-a-functional-programmer-part-5-c70adc9cf56a#.1hjllzi8t) (em inglês)
