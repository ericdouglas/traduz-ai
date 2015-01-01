# Programação Funcional Deve Ser Sua Prioridade número #1 em 2015
## OOP não pode mais nos salvar do *Cloud Monster*

* **Artigo Original**: [Functional Programming should be your #1 priority for 2015](https://medium.com/@jugoncalves/functional-programming-should-be-your-1-priority-for-2015-47dd4641d6b9)
* **Tradução**: [Eric Douglas](https://github.com/ericdouglas) e [Diogo Beato](https://github.com/diogobeato)

Você provavelmente ouviu expressões como "Clojure", "Scala", "Erlang", ou mesmo "Java agora tem lambdas". E você deve saber que isso tem alguma coisa a ver com "Programação Funcional". Se você participa de qualquer comunidade de programação, esse assunto provavelmente apareceu alguma vez.

Se você já buscou por "Functional Programming" (ou Programação Funcional), verá que não existe nada de novo. A segunda linguagem criada já utilizava isso, apareceu por volta dos anos 50, denominada *Lisp*. Por que então as pessoas estão empolgadas com isso apenas agora? 60 anos depois?!

## No início, os computadores eram realmente lentos

Acredite nisso ou não, computadores eram muuuuito mais lentos que o DOM. Não, realmente. E naquele tempo, existiam duas mentalidades principais em termos de arquitetura e implementação de linguagens de programação:

1. Iniciar pela Arquitetura de Von Neumann e adicionar abstração.
1. Iniciar pela Matemática e remover abstração.

Os computadores não tinham tanto poder de processamento para lidar com abstrações por todo o caminho e avaliaros programas funcionais. Então, Lisp acabou sendo morta lentamente e, portanto, não adequada para o trabalho. Foi quando a programação imperativa começou sua dominação, especialmente com a ascenção do *C*.

### Mas os Computadores melhoraram muito

Agora é virtualmente possível rodar a maioria das aplicações sem se preocupar muito sobre em qual linguagem ela foi escrita. Finalmente, linguagens funcionais tiveram sua segunda chance.

## Programação Funcional 50.5

Isso não é uma introdução a *FP* (*functional programming*). No fim dessa seção, você deverá ter uma ideia o quê *FP* é e como começar sua jornada.

Você pode entender Programação Funcional por programação com funções, que é, de fato, muito mais literal do que você pode imaginar agora. Você vai criar funções em termos de outras funções e compor funções (você se lembra `f ∘ g` da escola? Isso vai ser útil agora).

Aqui temos uma (não-exaustiva) lista de características da FP:

1. First-Class Functions (funções de primeira classe)
1. High-Order Functions (funções de ordem superior)
1. Pure Functions (funções puras)
1. Closures
1. Immutable State (estado imutável)

Você não deve se preocupar com nomes *sofisticados* agora: apenas entenda o que eles significam.

**First-Class Functions** ou *funções de primeira classe* significa que você vai armazenar funções dentro de uma variável. Acredito que você já fez algo parecido com isso em JavaScript:

```js
var add = function(a, b){
  return a + b
}
```

Você está apenas armazenando uma função anônima que recebe `a` e `b`, e retorna `a + b`, dentro de uma variável denominada `add`.

**High-Order Functions** ou *funções de ordem superior* significa que funções podem retornar funções ou receber funções como parâmetro.

Novamente, em JavaScript:

```js
document.querySelector('#button')
  .addEventListener('click', function(){
    alert('yay, i got clicked')
  }) 
```

ou

```js
var add = function(a){
  return function(b){
    return a + b
  }
}
 
var add2 = add(2)
add2(3) // => 5 
```

Ambos os casos são um exemplo de funções de ordem superior, mesmo que você nunca tenha usado algo parecido com isso, provavelmente você já viu algo assim em algum lugar.

**Pure Functions** ou *funções puras* signifca que funções não mudam nenhum valor, elas apenas recebem dados e retornam dados, assim como nossas amadas funções da Matemática. Isso também significa que se você passar `2` para uma função `f` e ela retornar `10`, ela sempre irá retornar 10. Não importa o ambiente, threads, ou qualquer ordem de avaliação. Ela não causa nenhum efeito colateral em outras partes do programa e isso é realmente um conceito poderoso.

**Closures** mean that you can save some data inside a function that's only accessible to a specific returning function.

```js
var add = function(a){
  return function(b){
    return a + b
  }
}
 
var add2 = add(2)
add2(3) // => 5 
```

Check out the second example of High-Order Function again, the variable a was enclosed and is only accessible to the returning function.

**Immutable State** means that you can't keep any state at all. In this following code (in OCaml), you can use `x` and `5` interchangeably in your program. `x` will be forever `5`.

```js
let x = 5;;
x = 6;;
 
print_int x;;  (* prints 5 *)

```

It pretty much looks like a downside than a good feature. But you'll see it's going to save your life.

## Object-oriented Programming cannot save us anymore