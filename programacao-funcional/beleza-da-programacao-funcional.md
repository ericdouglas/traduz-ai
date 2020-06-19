# A Beleza da Recursão e Pattern Matching (Casamento de Padrões) em Linguagens de Programação Funcionais

![Função lookup para retornar valor de uma árvore binária](https://i.imgur.com/lFXKbhd.png)

* **Artigo original**: [The beauty of recursion and pattern matching in functional programming languages](https://dev.to/ericdouglas/the-beauty-of-recursion-and-pattern-matching-in-functional-programming-languages-hmn)
* **Tradução**: [Eric Douglas](https://github.com/ericdouglas)

```erl
%% Função lookup recebe uma Key e um Node
lookup(_, {node, 'nil'}) ->
    undefined;
lookup(Key, {node, {Key, Val, _, _}}) ->
    {ok, Val};
lookup(Key, {node, {NodeKey, _, Smaller, _}}) when Key < NodeKey ->
    lookup(Key, Smaller);
lookup(Key, {node, {_, _, _, Larger}}) ->
    lookup(Key, Larger).

%% Explicação sobre parâmetros e estrutura de dados
%%
%% Nó vazio -> {node, 'nil'}
%% Nó não vazio -> {node, {Key, Val, Smaller, Larger}}
%% node -> é apenas um atom, como uma constante
%% Key e Val -> podem ser de tipos diferentes
%% Smaller e Larger -> Pode ser estruturas de nós vazios ou não vazios,
%%                       então você pode ter sua árvore binária
```

Gostaria de te convidar para admirar a beleza, concisão e poder do código acima [1]. 

É uma função que encontra um valor em uma árvore binária, usando _recursão_ e _pattern matching_ ([casamento de padrões](https://pt.wikipedia.org/wiki/Casamento_de_padr%C3%B5es)).

Podemos ver quão simples e extremamente declarativo é escrever uma função com esses conceitos e técnicas oriundas de linguagens de programação funcional.

**Temos absolutamente ZERO instruções dizendo COMO o algoritmo deve trabalhar!** Nós apenas expressamos **O QUÊ** ele deve fazer, não como.

O código foi escrito em **Erlang**.

## Explicação detalhada

Essa função vai buscar em uma árvore binária pela chave `Key` passada para ela, e vai retornar uma tupla com o valor `Val` (valor) associado com a chave passada, ou `undefined` se a chave não for encontrada.

Uma grande vantagem que temos no Erlang e Elixir é a possibilidade de criar mais casos para a mesma função apenas mudando o padrão em seus parâmetros.

A função tem o mesmo nome e aridade (número de parâmetros), mas como o padrão é diferente, é como se fosse uma outra função.

Os casos vão ser verificados de cima pra baixo, então devemos colocar os casos específicos primeiro, e casos gerais no fim. Pense nisso como uma forma funcional de escrever declarações `switch` ou `if / else`.

Nós temos quatro casos possíveis em nossa função `lookup/2`, então vamos ver o que cada um faz:

### `lookup(_, {node, 'nil'})`

O primeiro `lookup` vai ignorar o primeiro valor passado para ele (`_`), e se o segundo valor casar, ele vai retornar `undefined`. Como você pode ver, o segundo valor é um nó vazio.

### `lookup(Key, {node, {Key, Val, _, _}})`

O segundo `lookup` vai ver se a chave `Key` que passamos é a mesma `Key` do nó passado. Se for, nós acabamos de encontrar o nó que contém o valor que queremos retornar.

Nesse caso, `lookup` retorna `{ok, Val}`, onde `Val` é o valor que estávamos interessados.

### `lookup(Key, {node, {NodeKey, _, Smaller, _}}) when Key < NodeKey`

O terceiro `lookup` vai identificar com o caso onde a chave que passamos é diferente da chave do nó passado.

Também temos uma _cláusula de guarda_ na assinatura da função, para deixá-la mais específica. A cláusula de guarda `when Key < NodeKey` é importante para decidirmos em que direção devemos ir para buscar o valor que esperamos.

Nesse caso, sabemos que se a chave `Key` é menor que `NodeKey`, nós precisamos continuar nossa busca no nó `Smaller`. Então usamos _pattern match_ (casamento de padrão) para extrair o valor `Smaller` (menor) e ignorar o resto dos dados que não vamos usar com `_` novamente.

Para continuar nossa busca, quando nós paramos na terceira função `lookup`, nós vamos chamá-la recursivamente, agora com os seguintes dados `lookup(Key, Smaller)`. 

_Isso é **recursão** e **casamento de padrões** em ação! ❤_

### `lookup(Key, {node, {_, _, _, Larger}})`

No último caso, e o mais geral, é quando todas as prévias funções `lookup` não foram "casadas". Nós sabemos que isso só vai acontecer quando a chave `Key`  passada é diferente da `Key` do nó atual, e a `Key` passada é maior que a chave no nó atual, então temos que continuar a buscar no nó maior `Larger`.

Nesse caso, vamos chamar `lookup` recursivamente com os seguintes dados `lookup(Key, Larger)`.

**E é isso!**

Com ZERO instruções sobre COMO fazer, nós temos uma função super simples, declarativa e completa, que retorna um valor de uma árvore binária! Isso é absolutamente incrível e surpreendente <3.

Espero que tenha gostado!

Abraço! :)

## Referências

1. Código extraído do capítulo 7 - Recursão, seção "Mais que listas", do livro _Learn You Some Erlang for Great Good_. **[LINK](https://learnyousomeerlang.com/recursion)**
