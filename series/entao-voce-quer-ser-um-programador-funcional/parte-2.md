# Então você quer ser um Programador Funcional? (Parte 2)

* **Artigo original**: [So You Want to be a Functional Programmer (Part 2)](https://medium.com/@cscalfani/so-you-want-to-be-a-functional-programmer-part-2-7005682cec4a#.q2xydwfne)
* **Tradução**: [Gabriel](https://github.com/gabriel-ribeiro-ir)

Dar o primeiro passo para entender os conceitos de Programação Funcional é o mais importante e algumas vezes o passo mais díficil. Mas isto não tem de ser assim. Não com a perspectiva correta.

Parte anterior: 

[Parte 1](parte-1.md) (em português)
[Parte 1](https://medium.com/@cscalfani/so-you-want-to-be-a-functional-programmer-part-1-1f15e387e536#.es7crd8y1) (em inglês)

### Lembrete Amigável

Por favor leia o código lentamente. Tenha certeza de ter entendido antes de seguir em frente. Cada seção é baseada na anterior.

Se você se apressar, pode perder alguma coisa que será importante mais tarde.

### Refatoração

Vamos pensar em refatoração por um minuto. Aqui vai um pouco de código Javascript:

<pre>
	<code>
function validateSsn(<b>ssn</b>) {
    if (<b>/^\d{3}-\d{2}-\d{4}$/</b>.exec(<b>ssn</b>))
        console.log('Valid <b>SSN</b>');
    else
        console.log('Invalid <b>SSN</b>');
}
function validatePhone(<b>phone</b>) {
    if (<b>/^\(\d{3}\)\d{3}-\d{4}$/</b>.exec(<b>phone</b>))
        console.log('Valid <b>Phone Number</b>');
    else
        console.log('Invalid <b>Phone Number</b>');
}
	</code>
</pre>

Todos nós já escrevemos código como este ao longo do tempo, começamos a reconhecer que estas duas funções são praticamente iguais e diferenciam-se somente por algumas coisas (mostradas em **negrito**).

Ao invés de copiar **validadeSsn**, colar e editar para criar **validatePhone**, nós deveríamos criar uma única função e parametrizar as coisas que nós editamos depois de colar.

Neste exemplo, nós passaríamos como parâmetro o **value**, a **regular expression** e a **mensagem** (pelo menos a última parte da mensagem).

O código refatorado:

<pre>
<code>
function validateValue(<b>value, regex, type</b>) {
    if (<b>regex</b>.exec(<b>value</b>))
        console.log('Invalid ' + <b>type</b>);
    else
        console.log('Valid ' + <b>type</b>);
}
</code>
</pre>

Os parâmetros **ssn** e **phone** no código antigo, agora são representados pelo **value**.

As expressões regulares **/^\d{3}-\d{2}-\d{4}$/** e **/^\(\d{3}\)\d{3}-\d{4}$/** são representadas pelo **regex**.

E por último, a última parte da mensagem **SSN** e **'Phone Number'** são representados pelo **type**.

Ter uma função é muito melhor do que ter duas funções. Ou pior, três, quatro ou dez funções. Isto mantém seu código limpo e fácil de manter.

Por exemplo, se houver um bug, você precisa arrumar somente em um lugar versus procurar por todo o codebase para para achar o lugar onde esta função PODE ter sido posta e modificada.

Mas o quê acontece quando você tem a seguinte situação:

<pre>
<code>
function validateAddress(address) {
    if (<b>parseAddress</b>(address))
        console.log('Valid Address');
    else
        console.log('Invalid Address');
}
function validateName(name) {
    if (<b>parseFullName</b>(name))
        console.log('Valid Name');
    else
        console.log('Invalid Name');
}
</code>
</pre>

Aqui **parseAddress** e **parseFullName** são funções que recebem uma **string** e retornam **true** se ela for analisada.

Como refatoramos isto?

Bem, nós podemos usar **value** para **address** e **name**, e **type** para **'Address'** e **'Name'** como fizemos antes mas existe uma função onde nossa expressão regular costumava estar.

Se nós somente pudéssemos passar uma função como parâmetro...

### High-Order Functions

Muitas linguagens não suportam passar funçõe como parâmetros. Algumas sim mas elas não facilitam.

> Em Programação Funcional, uma função é um cidadão de primeira classe da linguagem. Em outras palavras, uma função é somente outro valor.

Uma vez que função são somente valores, podemos passá-las como parâmetros.

Mesmo que a Javascript não seja uma Linguagem Puramente Funcional, você pode fazer algumas operações funcionais com ela. Então aqui estão as últimas duas funções refatoradas em uma única função passando a **função de parse** como parâmetro chamado **parseFunc**:

<pre>
<code>
function validateValueWithFunc(value, <b>parseFunc</b>, type) {
    if (<b>parseFunc</b>(value))
        console.log('Invalid ' + type);
    else
        console.log('Valid ' + type);
}
</code>
</pre>

Nossa nova função é chamada de **High-order Function**.

> High-order Functions também recebe funções como parâmetros, retorna funções ou ambas.

Agora podemos chamar nossa high-order function para as 4 funções anteriores (isto funciona na Javascript porque **Regex.exe** retorna um valor verdadeiro quando uma combinação é encontrada):

```
validateValueWithFunc('123-45-6789', /^\d{3}-\d{2}-\d{4}$/.exec, 'SSN');
validateValueWithFunc('(123)456-7890', /^\(\d{3}\)\d{3}-\d{4}$/.exec, 'Phone');
validateValueWithFunc('123 Main St.', parseAddress, 'Address');
validateValueWithFunc('Joe Mama', parseName, 'Name');
```

Isto é muito melhor do que ter 4 funções identicas.

Mas note as expressões regulares. Elas são um pouco verbosas. Vamos limpar nosso código refatorando-o:

<pre>
<code>
var <b>parseSsn</b> = /^\d{3}-\d{2}-\d{4}$/.exec;
var <b>parsePhone</b> = /^\(\d{3}\)\d{3}-\d{4}$/.exec;
validateValueWithFunc('123-45-6789', <b>parseSsn</b>, 'SSN');
validateValueWithFunc('(123)456-7890', <b>parsePhone</b>, 'Phone');
validateValueWithFunc('123 Main St.', <b>parseAddress</b>, 'Address');
validateValueWithFunc('Joe Mama', <b>parseName</b>, 'Name');
</code>
</pre>

Assim está melhor. Agora quando queremos analisar um número de telefone, não precisamos copiar e colar a expressão regular.

Mas imagine que temos mais expressões regulares para analisar, não somente **parseSsn** e **parsePhone**. Cada vez que criamos um analisador de expressão regular, temos que lembrar de adicionar o **.exec** no final. E acredite, isto é fácil de esquecer.

Podemos nos proteger disso criando uma high-order function que retorna a função **exec**:

<pre>
<code>
function <b>makeRegexParser</b>(regex) {
    return regex.exec;
}
var parseSsn = <b>makeRegexParser</b>(/^\d{3}-\d{2}-\d{4}$/);
var parsePhone = <b>makeRegexParser</b>(/^\(\d{3}\)\d{3}-\d{4}$/);
validateValueWithFunc('123-45-6789', parseSsn, 'SSN');
validateValueWithFunc('(123)456-7890', parsePhone, 'Phone');
validateValueWithFunc('123 Main St.', parseAddress, 'Address');
validateValueWithFunc('Joe Mama', parseName, 'Name');
</code>
</pre>

Aqui, **makeRegexParser** recebe uma expressão regular e retorna a função **exec**, que recebe uma string. **validateValueWithFunc** passará a string, **value**, para a função de parse, isto é **exec**.

**parseSsn** e **parsePhone** são efetivamente as mesmas de antes, a função **exec** da expressão regular.

Isto é uma ligeira melhoria, mas é mostrada aqui para dar um exemplo de high-order function que retorna uma função.

Contudo, você pode imaginar os benefícios de fazer esta mudança caso **makeRegexParser** fosse mais complexa.

Aqui está um outro exemplo de high-order function que retorna uma função:

```
function makeAdder(constantValue) {
    return function adder(value) {
        return constantValue + value;
    };
}
```

Aqui temos **makeAdder** que recebe **constantValue** e retorna **adder**, uma função que irá adicionar essa constante para qualquer valor que for passado.

Aqui está como pode ser usada:

```
var add10 = makeAdder(10);
console.log(add10(20)); // prints 30
console.log(add10(30)); // prints 40
console.log(add10(40)); // prints 50
```

Nós criamos uma função, **add10**, passando a constante **10** para **makeAdder** que retorna a função que irá somar **10** para tudo.

Perceba que a função **adder** tem acesso a **constantValue** mesmo depois de **makeAdder** retorna. Isto é porque **constantValue** estava em seu escopo quando **adder** foi criado.

Este comportamento é bastante importante porque sem ele, funções que retornam funções não seriam muito úteis. Então é importante entendermos como elas funcionam e como este comportamento é chamado.

Este comportamento é chamado de **Closure**.

### Closures

Aqui está um exemplo planejado para mostrar o uso de closures:

<pre>
<code>
<b>function grandParent</b>(g1, g2) {
    var g3 = 3;
    return <b>function parent</b>(p1, p2) {
        var p3 = 33;
        return <b>function child</b>(c1, c2) {
            var c3 = 333;
            return g1 + g2 + g3 + p1 + p2 + p3 + c1 + c2 + c3;
        };
    };
}
</code>
</pre>

Neste exemplo, **child** tem acesso as suas variáveis, as variáveis de **parent** e **grandParent**.

A **parent** tem aceso as suas variáveis e as da **grandParent**.

A **grandParent** tem acesso somente as suas variáveis.

(Veja a pirâmide acima para esclarecimento.)

Aqui está um exemplo de seu uso:

```
var parentFunc = grandParent(1, 2); // returns parent()
var childFunc = parentFunc(11, 22); // returns child()
console.log(childFunc(111, 222)); // prints 738
// 1 + 2 + 3 + 11 + 22 + 33 + 111 + 222 + 333 == 738
```

Aqui, **parentFunc** mantém o escopo de **parent** vivo visto que **grandParent** retorna **parent**.

Do mesmo modo, **childFunc** mantém o escopo de **child** vivo visto que **parentFunc** que é apenas **parent**, retorna **child**.

Quando uma função é criada, todas as variáveis em seu escopo **em tempo de criação** são acessíveis a ela pelo tempo de vida da função. Uma função existe enquanto houver uma referência à ela. Por exemplo, o escopo de **child** existe enquanto **childFunc** referenciá-lo.

> Uma closure é um escopo de uma função que é mantido vivo por uma referência àquela função.

Note que na Javascript, closures são problemáticas visto que as variáveis são mutáveis, isto é, elas podem mudar de valor do tempo em que elas foram "enclausuradas" para o tempo que a função retornada é chamada.

Agradecidamente, variáveis em Linguagens Funcionais são imutáveis, eliminando esta fonte comum de bugs e confusão.

### Meu cérebro!!!

Por enquanto chega.

Nas próximas partes deste artigo, eu vou falar sobre Functional Composition, Currying, funções funcionais comuns (ex: map, filter, fold etc.), e mais.

A seguir: 

[Parte 3](parte-3.md) (em português)
[Parte 3](https://medium.com/@cscalfani/so-you-want-to-be-a-functional-programmer-part-3-1b0fd14eb1a7#.zffq7cklj) (em inglês)