# Então você quer ser um Programador Funcional? (Parte 3)

* **Artigo original**: [So You Want to be a Functional Programmer (Part 3)](https://medium.com/@cscalfani/so-you-want-to-be-a-functional-programmer-part-3-1b0fd14eb1a7#.zffq7cklj)
* **Tradução**: [Gabriel](https://github.com/gabriel-ribeiro-ir)

Dar o primeiro passo para entender os conceitos de Programação Funcional é o mais importante e algumas vezes o passo mais díficil. Mas isto não tem de ser assim. Não com a perspectiva correta.

Parte anterior: 

[Parte 2](parte-2.md) (em português)
[Parte 2](https://medium.com/@cscalfani/so-you-want-to-be-a-functional-programmer-part-2-7005682cec4a#.q2xydwfne) (em inglês)

### Composição de Funções

Como programadores, somos preguiçosos. Nós não queremos buildar, testar e fazer deploy de código que escrevemos de novo, de novo e de novo outra vez.

Nós estamos sempre tentando descobrir formas de fazer o trabalho uma vez e como podemos reusá-lo para fazer alguma outra coisa.

Reuso de código soa bem, mas é díficil de alcançar. Faça o código muito específico e não poderá reusá-lo. Faça-o muito genérico e também pode ser díficil de usá-lo logo de cara.

Então o que precisamos é um equilíbrio entre os dois, uma forma de fazer pequenas peças reutilizáveis que podemos usar como blocos de construção para construir funcionalidades mais complexas.

Em Programação Funcional, funções são nossos blocos de construção. Nós as escrevemos para fazer tarefas muito específicas e então nós as juntamos como bloquinhos de Lego.

Isto é chamado de **Composição de Funções**.

Então como isto funciona? Vamos começar com duas funções Javascript:

```
var add10 = function(value) {
	return value + 10;
};
var mult5 = function(value) {
	return value * 5;
};
```

Isto é muito verboso então vamos reescrever usando a notação de [**arrow function**](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Functions/Arrow_functions)

```
var add10 = value => value + 10;
var mult5 = value => value * 5;
```

Bem melhor. Agora vamos imaginar que também queremos ter uma função que recebe um valor , adiciona 10 a ele e depois multiplique o resultado por 5. Podemos escrever:

`var mult5AfterAdd10 = value => 5 * (value + 10)`

Acabamos de usar funções para criar **mult5AfterAdd10**, mas existe uma forma melhor.

Em matemática, **f ∘ g** é uma composição funcional e é lida **"f composta com g"** ou, mais comumente, **"f depois de g"**. Então **(f ∘ g)(x)** é equivalente a chamar **f** depois de chamar **g** com **x** ou simplesmente, **f(g(x))**.

Em nosso exemplo, temos **mult5 ∘ add10** ou **"mult5 depois de add10"**, por isso o nome da nossa função **mult5AfterAdd10**.

E isto é examente o que fizemos. Nós chamamos **mult5** depois de ter chamado **add10** com **value** ou simplesmente, **mult5(add10(value))**.

Visto que Javascript não tem Composição de Funções nativamente, vamos ver no Elm:

```
add10 value =
	value + 10
mult5 value =
	value * 5
mult5AfterAdd10 value =
	(mult5 << add10) value
```

Usando o operador << infixo é como você *compõe* funções no Elm. Ele nos dá o senso visual de como o dado está fluindo. Primeiro, **value** é passado para **add10** então seus resultados são passados para **mult5**.

Note os parênteses em **mult5AfterAdd10**, **(mult5 << add10)**. Eles estão alí para assegurar que as funções são compostas primeiro antes de aplicar o **value**.

Você pode compor quantas funções quiser desta forma:

```
f x =
   (g << h << s << r << t) x
```


Aqui **x** é passado para a função **t** cujo resultado é passado para **r** cujo resultado é passado para **s** e assim por diante. Se você fizesse algo parecido em Javascript seria algo como **g(h(s(r(t(x)))))**, um pesadelo parentético.

### Notação Point-Free

Existe um estilo de escrever funções sem ter de especificar os parâmetros chamado **Notação Point-Free**. No começo este estilo vai parecer estranho mas conforme você continua, aprenderá a apreciar a brevidade.

Em **mult5AfterAdd10**, você vai perceber que **value** é especificado 2 vezes. Uma vez na lista de parâmetros e uma quando é usado.

```
-- Esta é uma função que espera 1 parâmetro
mult5AfterAdd10 value =
	(mult5 << add10) value
```

Mas este parâmetro é desnecessário desde que **add10**, a função mais à direita na composição, espera o mesmo parâmetro. A seguinte versão point-free é equivalente:

```
-- Esta também é uma função que espera 1 parâmetro
mult5AfterAdd10 =
	(mult5 << add10)
```

Existem muitos benefícios em usar a versão point-free.

Primeiro, nós não temos que especificar parâmetros redundantes. E visto que não temos que especificá-los, não precisamos pensar em nomes para todos eles.

Segundo, é mais fácil de ler e a razão disto é que é menos verboso. Este exemplo é simples, mas imagine uma função que receba mais parâmetros.

### Problema no Paraíso

Até agora vimos como Composição de Funções trabalham e como especificamos nossas funções na notação Point-Free para brevidade, clareza e flexibilidade.

Agora, vamos tentar usar estas idéias em um cenário levemente diferente e ver como sai. Imagine substituir **add10** com **add**:

```
add x y =
	x + y
mult5 value =
	value * 5
```

Como podemos escrever **mult5After10** somente com estas duas funções?

Pense um pouco sobre isto antes de continuar a leitura. Sério. Pense um pouco. Tente e faça.

Ok, se você levou um tempo pensando sobre isto, pode ser que tenha chegado a uma conclusão como:

```
-- Está errado !!!!
mult5AfterAdd10 =
	(mult5 << add) 10 
```

Isto não funciona. Por que? Porque **add** recebe 2 parâmetros.

Se não está óbvio no Elm, tente escrever com Javascript:

`var mult5AfterAdd10 = mult5(add(10)); // isto não funciona`

Este código está errado, mas por que?

Porque a função **add** está recebendo somente 1 de seus 2 parâmetros e então seus resultados incorretos são passados para **mult5**. Isto produzirá resultados incorretos.

De fato, em Elm, o compilador não deixará você escrever tal código "mal formado" (o que é uma das melhores coisas no Elm).

Vamos tentar novamente:

`var mult5AfterAdd10 = y => mult5(add(10, y)); // not point-free`

Isto não é point-free mas eu provavelmente poderia viver com isto. Mas agora não estou mais somente combinando funções. Estou escrevendo uma nova função. Além de que se ficar mais complicado, por exemplo se eu quiser compor **mult5AfterAdd10** com alguma outra coisa, vou encontrar um problema real.

Assim, pareceria que Composição de Funções tem uma utilidade limitada, desde que não podemos casar estas duas funções. Isto é ruim visto que é algo tão poderoso.

Como resolvemos isto? O que precisamos para resolver este problema?

Bem, seria muito bom se tívessemos algum jeito de dar a função **add** somente um de seus parâmetros antes do tempo e depois ela pegaria seu segundo parâmetro quando **mult5AfterAdd10** fosse chamada.

Acontece que existe um jeito e é chamado de **Currying**.

### Meu cérebro!!!

Por enquanto chega.

Nas próximas partes deste artigo, eu vou falar sobre Currying, funções funcionais comuns (ex: map, filter, fold etc.), Transparência Referencial e mais.

A seguir: 

[Parte 4](https://medium.com/@cscalfani/so-you-want-to-be-a-functional-programmer-part-4-18fbe3ea9e49#.j22a5ccmb) (em inglês)