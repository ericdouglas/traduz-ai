# Uma Suave Introdução ao JavaScript Funcional: Parte 4

* **Artigo Original**: [A GENTLE INTRODUCTION TO FUNCTIONAL JAVASCRIPT: PART 4](http://jrsinclair.com/articles/2016/gentle-introduction-to-functional-javascript-style/)
* **Tradução**: [Eric Douglas](https://github.com/ericdouglas)

> *Escrito por James Sinclair em 11 de Fevereiro de 2016* 

Essa é a parte 4 de uma série de 4 artigos sobre introdução a programação funcional no JavaScript. No último artigo vamos ver sobre *high-order functions* (funções de ordem superior): funções para criar funções. Nesse artigo, nós discutimos como usar essas novas ferramentas com estilo. 

- [Parte 1: Blocos fundamentais e motivação](009-uma-suave-introducao-ao-javascript-parte-1.md)
- [Parte 2: Trabalhando com Arrays e Listas](010-uma-suave-introducao-ao-javascript-parte-2.md)
- [Parte 3: Funções para fazer funções](011-uma-suave-introducao-ao-javascript-parte-3.md)
- Parte 4: Fazendo isso com estilo (esse artigo)

## Fazendo Isso Com Estilo
No artigo anterior vimos sobre `partial`, `compose`, `curry`, `pipe`, e também como podemos usá-las para juntar pequenas, simples funções e criar outras grandes e complicadas. Mas o que isso faz pra gente? Vale a pena se importar quando já estamos escrevendo código perfeitamente válido?

Parte da resposta é que sempre é útil ter mais ferramentas disponíveis para realizar o trabalho - desde que você saiba como usá-las - e programação funcional certamente nos dá um conjunto útil de ferramentas para escrever JavaScript. Mas eu penso que há mais do que isso. Programação Funcional nos abre um diferente *estilo* de programação. Isso por sua vez nos permite conceitualizar problemas e soluções de formas diferentes.

Existem duas funcionalidades chave para programação funcional:

1. Escrever funções puras, o que é importante se você deseja testar programação funcional; e
1. Estilo de programação *pointfree* (sem pontos), o que não é *tão* importante mas é bom de se entender.

### Pureza
Se você ler sobre programação funcional, você irá se deparar com o conceito de funções *puras* e *impuras*. Funções puras são funções que preenchem dois critérios:

1. Chamar a função com as mesmas entradas sempre *retorna* as mesmas saídas.
1. Chamar a função não produz efeitos colaterais: Sem chamadas na rede (*network*); nenhuma leitura ou escrita de arquivos; nenhuma consulta em banco de dados; nenhum elemento DOM modificado; nenhuma modificação de variáveis globais; e nenhuma saída no console. Nada.

Funções impuras deixam programadores funcionais desconfortáveis. Tão desconfortáveis que eles as evitam o máximo possível. O problema com isso é que o grande ponto de escrever programas de computador *são* os efeitos colaterais. Fazer uma chamada na rede e renderizar elementos DOM está no núcleo do que uma aplicação web faz; esse foi o motivo pelo qual o JavaScript foi inventado.

Então o que um aspirante a programador funcional faz? Bom, a chave é que nós não evitamos funções impuras inteiramente, nós apenas damos a elas uma boa quantidade de respeito, e deixamos de lidar com elas até que nós absolutamente precisemos. Elaboramos um plano claro e testado para o que queremos fazer *antes* de tentar fazer isso. Como Eric Elliot colocou em [*The Dao of Immutability*](https://medium.com/javascript-scene/the-dao-of-immutability-9f91a70c88cd):

> **Separação:** Lógica é pensamento. Efeitos são ações. Portanto, o sábio pensa antes de agir, e age somente quando o pensamento está terminado. Se você tentar executar efeitos e lógica ao mesmo tempo, você vai criar efeitos colaterais ocultos que causam *bugs* (problemas) na lógica. Mantenha as funções pequenas. Faça uma coisa de cada vez, e faça isso bem.

Em outras palavras, com programação funcional, nós geralmente trabalhamos a lógica do que estamos tentando fazer primeiro, antes de fazer qualquer coisa que potencialmente tenha efeitos colaterais.

Outra forma de pensar sobre isso é a diferença de usar uma metralhadora e um *rifle sniper*. Com a metralhadora você dispara o máximo de balas possível, contando com o fato que se você continuar atirando, eventualmente você vai acertar algo. Mas você pode também acertar coisas que você não queria. Porém um rifle sniper é direfente. Você escolhe o lugar mais vantajoso, alinha o tiro, toma em conta a velocidade do vento e a distância do alvo. Você pacientemente, metodicamente, cuidadosamente configura todas as coisas e no momento certo, puxa o gatilho. Muito menos munição, e um efeito muito mais preciso.

Então como tornamos nossas funções puras? Vamos ver um exemplo:

```js
var myGlobalMessage = '{{verb}} me';

var impureInstruction = function(verb) {
    return myGlobalMessage.replace('{{verb}}', verb);
}

var eatMe = impureInstruction('Eat');
//=> 'Eat me'
var drinkMe = impureInstruction('Drink');
//=> 'Drink me'
```

Essa função é impura pois depende da variável global `myGlobalMessage`. Se a variável mudar, se torna difícil dizer o que `impureInstruction` vai fazer. Uma forma de torná-la pura é mover a variável para dentro:

```js
var pureInstruction = function (verb) {
    var message =  '{{verb}} me';
    return message.replace('{{verb}}', verb);
}
```

Essa função agora vai sempre retornar o mesmo resultado dado um mesmo conjunto de entradas. Mas algumas vezes não podemos usar essa ténica. Por exemplo:

```js
var getHTMLImpure = function(id) {
    var el = document.getElementById(id);
    return el.innerHTML;
}
```

Essa função é impura pois depende do objeto `document` para acessar o DOM. Se o DOM mudar isso *pode* produzir resultados diferentes. Mas nós não podemos definir `document` dentro de nossa função porque isso é uma API do navegador, mas nós *podemos* passar isso como um parâmetro:

```js
var getHTML = function(doc, id) {
    var el = doc.getElementById(id);
    return el.innerHTML;
}
```

Isso pode parecer trivial e sem sentido, mas é uma técnica útil. Imagine você estava tentando fazer um teste unitário nessa função. Normalmente, teríamos que configurar algum tipo de browser para pegar o objeto `document` e poder testar isso. Uma vez que temos o parâmetro `doc`, é fácil passar um objeto *stub* - uma simulação do objeto real - ao invés disso:

```js
var stubDoc = {
    getElementById: function(id) {
        if (id === 'jabberwocky') {
            return {
                innerHTML: '<p>Twas brillig…'
            };
        }
    }
};

assert.equal(getHTML('jabberwocky'), '<p>Twas brillig…');
//=> test passes
```

Escrever esse *stub* pode parecer um pouco trabalhoso, mas agora podemos testar essa função sem precisar de um navegador. Se quisermos, podemos rodar isso na linha de comando sem um navegador *headless*. E como um bônus, o teste vai rodar muito, muito mais rápido do que um que tenha o objeto `document` inteiro.

Uma outra forma de ter uma função pura é retornar outra função que eventualmente irá fazer algo impuro quando a chamarmos. Isso inicialmente parece um truque sujo, mas é totalmente legítimo. Por exemplo:

```js
var htmlGetter = function(id) {
    return function() {
        var el = document.getElementById(id);
        return el.innerHTML;
    }
}
```

 A função `htmlGetter` é pura porque rodando-a não acessamos uma variável global - ao invés disso, ela sempre retorna exatamente a mesma função.

 Fazer coisas dessa forma não é muito útil para testes unitários, e isso não remove a impureza completamente - apenas a posterga. E isso não é necessariamente uma coisa ruim. Lembre-se, queremos lidar com toda a lógica previamente nas funções puras e depois puxar o gatilho em quaisquer efeitos colaterais.

### *Pointfree* (sem pontos)
Programação *Pointfree* ou programação *tácita* é um estilo particular de programação que funções de ordem superior como `curry` e `compose` tornam possível. Para explicar isso, vamos olhar novamente para o exemplo do poema do último artigo:

> **Nota do tradutor:** Programação tácita, também chamada estilo **point-free** (sem pontos), é um paradigma de programação em que definições de função não identificam os argumentos (ou "pontos") em que elas operam. Ao invés disso, as definições meramente compõem outras funções, onde essas são combinadores que manipulam os argumentos. [Fonte](https://en.wikipedia.org/wiki/Tacit_programming).

```js
var poem = 'Twas brillig, and the slithy toves\n' + 
    'Did gyre and gimble in the wabe;\n' +
    'All mimsy were the borogoves,\n' +
    'And the mome raths outgrabe.';

var replace = curry(function(find, replacement, str) {
    var regex = new RegExp(find, 'g');
    return str.replace(regex, replacement);
});

var wrapWith = curry(function(tag, str) {
    return '<' + tag + '>' + str + '</' + tag + '>'; 
});

var addBreaks      = replace('\n', '<br/>\n');
var replaceBrillig = replace('brillig', wrapWith('em', 'four o’clock in the afternoon'));
var wrapP          = wrapWith('p');
var wrapBlockquote = wrapWith('blockquote');

var modifyPoem = compose(wrapBlockquote, wrapP, addBreaks, replaceBrillig);
```

Note que `compose` espera que cada função passada receba exatamente um parâmetro. Então, usamos 'curry' para mudar nossas funções multi-parâmetros `replace` e `wrapWith` em funções de um único parâmetro. Note também que fomos um pouco criteriosos com a ordem de nossas funções, então `wrapWith`, por exemplo, recebe a *tag* como seu primeiro parâmetro ao invés do texto para ser envolvido. Se formos cuidadosos dessa forma na maneira como configuramos nossas funções, isso fará a criação de funções por composição fácil.

> **Nota do autor**: A maioria das bibliotecas funcionais (como [Ramda](http://ramdajs.com/)), também incluem utilitários para trabalhar com funções que não tem seus parâmetros em uma ordem conveniente.

Isso se torna tão fácil que você pode escrever *todo* o seu código dessa maneira. Mas note um pequeno efeito colateral: quando definimos a função `modifyPoem` final, nós nunca mencionamos em nenhum lugar que ela recebe um único argumento *string*. E se você olhar para as funções *curried*, `addBreaks`, `replaceBrillig`, `wrapP` e `wrapBlockquote`, nenhuma delas mencionam que recebem também apenas uma simples variável *string*. Isso é programação *pointfree* (sem pontos): começar com um conjunto base de funções utilitárias (como Ramda ou functional.js) e escrever seu código de uma forma que você nunca vai mencionar as variáveis de entrada.

O que isso nos dá? Bom, nada de especial em relação ao próprio código. A coisa inteligente sobre o estilo sem pontos é que ele te força a usar `compose`, `curry`, `pipe`, etc. Por sua vez, isso *encoraja fortemente* que você mantenha funções pequenas e simples reunidas de maneira sensata. Em outras palavras, isso é uma limitação autoimposta, como um *haiku* ou um soneto. Nem toda poesia tem que ser escrita dessa forma - e seguir todas as regras não garante um belo poema - mas algumas poesias escritas nesses estilos podem ser incrivelmente belas.

Fazer tudo no estilo sem pontos não é sempre prático. Algumas vezes, isso adiciona complicações desnecessárias em uma função simples. Mas dar uma chance e *tentar* escrever todas as suas funções sem pontos é uma boa maneira de entender melhor a programação funcional.

### Assinatura de Tipos de Hindley-Milner
Uma vez que você esteja fazendo tudo "sem pontos" (*pointfree*), isso deixa uma dúvida, como comunicar outros programadores qual tipo de parâmetro eles devem passar para sua função. Para facilitar isso, programadores funcionais desenvolveram uma notação especial para especificar quais tipos de parâmetro uma função recebe, e o que ela retorna. A notação é chamada *assinatura de tipos de Hindley-Milner*. Nós a escrevemos como comentários onde definimos a função. Vamos ver alguns exemplos:

```js
// instruction :: String -> String
var instruction = function(verb) {
    return verb + ' me';
}
```

A assinatura de tipo diz que `instruction` recebe uma *String* como entrada e retorna outra *String*. Por enquanto, tudo bem. E se tivermos uma função que recebe dois parâmetros?

```js
// wrapWith :: String -> (String -> String)
var wrapWith = curry(function(tag, str) {
    return '<' + tag + '>' + str + '</' + tag + '>'; 
});
```

Isso é um pouco mais complicado, mas não tão difícil. Essa notação diz que `wrapWith` recebe uma *String* e retorna uma *Função*, e essa função recebe uma *String* e retorna uma *String*. Note que isso funciona porque nós aplicamos *curry* na função. Quando estivermos usando esse estilo, está assumido que você sempre vai usar *curry* em todas suas funções.

E algo com três parâmetros ao invés de dois? Uma forma de escrever isso seria:

```js
// replace :: String -> (String -> (String -> String))
var replace = curry(function(find, replacement, str) {
    var regex = new RegExp(find, 'g');
    return str.replace(regex, replacement);
});
```

Agora temos uma função que retorna uma função que retorna uma função que retorna uma *string*. Isso ainda faz sentido, mas porque sempre assumimos que tudo está sendo *curried*, tendemos a remover os parênteses:

```js
// replace :: String -> String -> String -> String
```

E se tivermos um parâmetro de um tipo diferente:

```js
// formatDollars :: Number -> String
var formatDollars = replace('${{number}}', '{{number}}');

formatDollars(100);
//=> $100
```

Aqui temos uma função sem pontos (*pointfree*), e se torna claro porque as assinaturas de tipos são úteis. Essa pega um número e retorna uma *string*. E se tivéssemos um array?

```js
// sum :: [Number] -> Number
var sum = reduce(add, 0);
```

Essa recebe um array de números e retorna um número (assumindo que aplicamos *curry* na função `reduce` do segundo artigo).

Alguns exemplos finais:

```js
// identity :: a -> a
var identity = function(x) { return x };

// map :: (a -> b) -> [a] -> [b]
var map = curry(function(callback, array) {
    return array.map(callback);
});
```

A função `identity` acima recebe um parâmetro de um tipo e retorna uma variável do mesmo tipo. A função `map` por outro lado, recebe uma função que recebe uma variável do tipo *a* e retorna uma variável do tipo *b*. Depois então pega um array de valores, todos do tipo *a* e retorna um array de valores, todos do tipo *b*.

Você verá que bibliotecas como [Ramda](http://ramdajs.com/), por exemplo, usam essa notação para documentar todas as funções da biblioteca.

### Indo Mais Fundo
Nós mal arranhamos a superfície da programação funcional. Mas entendendo funções de primeira classe, aplicação parcial e composição nos dá os blocos básicos para irmos muito mais longe. Se você estiver interessado em ler mais, há uma lista de links úteis abaixo:

- [Can Your Programming Language do This?](http://www.joelonsoftware.com/items/2006/08/01.html) by Joel Spolsky
- [The Dao of Immutability](https://medium.com/javascript-scene/the-dao-of-immutability-9f91a70c88cd) by Eric Elliot
- [Why Ramda?](http://fr.umio.us/why-ramda/), by Scott Sauyet
- [Professor Frisby’s Mostly Adequate Guide to Functional Programming](https://github.com/MostlyAdequate/mostly-adequate-guide) by Brian Lonsdorf
- [JavaScript Allongé](https://leanpub.com/javascriptallongesix/read) by Reg “raganwald” Braithwaite
