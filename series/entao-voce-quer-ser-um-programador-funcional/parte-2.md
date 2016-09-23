# Então você quer ser um Programador Funcional? (Parte 2)

* **Artigo original**: [So You Want to be a Functional Programmer (Part 2)](https://medium.com/@cscalfani/so-you-want-to-be-a-functional-programmer-part-2-7005682cec4a#.q2xydwfne)
* **Tradução**: [Gabriel](https://github.com/gabriel-ribeiro-ir)

Dar o primeiro passo para entender os conceitos de Programação Funcional é o mais importante e algumas vezes o passo mais díficil. Mas isto não tem de ser assim. Não com a perspectiva correta.

Parte anterior: [Parte 1](https://medium.com/@cscalfani/so-you-want-to-be-a-functional-programmer-part-1-1f15e387e536#.es7crd8y1)

### Lembre Amigável

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

Mesmo que a Javascript não seja uma Linguagem Funcional Pura, você pode fazer algumas operações funcionais com ela. Então aqui está as últimas duas funções refatoradas em uma única função passando a **função de parse** como parâmetro chamado **parseFunc**:

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


