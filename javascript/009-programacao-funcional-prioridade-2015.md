# Functional Programming should be your #1 priority for 2015
## OOP cannot save us from the Cloud Monster anymore.

* **Artigo Original**: [Functional Programming should be your #1 priority for 2015](https://medium.com/@jugoncalves/functional-programming-should-be-your-1-priority-for-2015-47dd4641d6b9)
* **Tradução**: [Eric Douglas](https://github.com/ericdouglas)

You probably have heard expressions like “Clojure”, “Scala”, “Erlang” or even “Java now has lambdas”. And you might know it has something to do with "Functional Programming". If you’re participating to any Programming Community, this subject probably has popped up already.

If you'd google "Functional Programming", you'll see there's nothing new. The second language ever created already embraces it, it appeared in 50's and was named Lisp. So why the heck people are only excited about it now? Around 60 years later?

## At the beginning, Computers were really slow

Believe it or not, Computers were waaay slower than the DOM. No, really. And at that time, there's 2 main mindsets in terms of design and implementation of programming languages:

1. Start from the Von Neumann Architecture and add abstraction.
1. Start from Mathematics and remove abstraction.

The computers didn't have that much processing power to deal with abstractions from all the way down to evaluate functional programs. So, Lisp ended up being deadly slow and, therefore, not suited for the job. That's when imperative programming started its domination, specially with the raise of C.

### But Computers has improved a lot

Now it's virtually OK to run the most of applications out there without caring so much about which language it has been written in. Finally, functional languages got their second chance.

## Functional Programming 50.5

This isn't an introduction to FP at all. At the end of this section, you should be able to have an idea of what FP might be and how to start your journey.

You can understand Functional Programming as programming with functions, which is, in fact, much more literal than you can imagine now. You'll create functions in terms of other functions and compose functions (Do you remember the `f ∘ g` from school? It'll be useful now). That's all.

Here's a (non-exhaustive) list of FP features:

1. First-Class Functions
1. High-Order Functions
1. Pure Functions
1. Closures
1. Immutable State

You shouldn't care about *fancy* names right now: Just understand what they mean.

**First-Class Functions** mean that you can store functions into a variable. I believe you already have done something like this in JavaScript:

```js
var add = function(a, b){
  return a + b
}
```

You're just storing an anonymous function, that receives `a` and `b` and returns `a + b`, into a variable named `add`.

**High-Order Functions** mean that functions can return functions or receive other functions as params.

Again, in JavaScript:

```js
document.querySelector('#button')
  .addEventListener('click', function(){
    alert('yay, i got clicked')
  }) 
```

or

```js
var add = function(a){
  return function(b){
    return a + b
  }
}
 
var add2 = add(2)
add2(3) // => 5 
```

Both of cases are an example of High-Order Functions, even though you've never coded anything like that, you probably have seen this pattern somewhere else.

**Pure Functions** mean that the function doesn't change any value, it just receives data and output data, just like our beloved functions from Mathematics. That also means that if you'd pass `2` for a function `f` and it returns `10`, it'll always return `10`. Doesn't it matter the environment, threads, or any evaluation order. They don't cause any side-effects in other parts of the program and it's a really powerful concept.

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

## Programação Orientada a Objetos não pode mais nos salvar

Aquele momento de que teríamos aplicações concorrentes e distribuidas rodando finalmente chegou.

Infelizmente, nós não estamos prontos: nosso "atual" (i.e., mais usado) modelo para concorrência e paralelismo, embora resolva o problema, ainda traz muita complexidade.

Para termos melhores aplicações, precisamos de uma maneira simples e confiável para fazer isto. Você se lembra das features de FP mencionados acima? Pure Functions (Funções Puras) e Immutable State (Estados Imutáveis)? Exatamente. Você pode rodar uma função milhares de vezes em diferentes cores ou máquinas que ainda assim não terá resultados diferentes dos que teve anteriormente. Sendo assim, você pode usar o mesmo código para rodar em 1 core ou em 1k de cores. Podemos ser felizes novamente.

"Mas por que eu não posso continuar usando POO?"

Ao menos para concorrência e paralelismo, POO não pode mais te salvar. Isso porque, POO precisa que haja estados mutáveis. Os métodos dos objetos que você chama, normalmente, é para alterar o estado do objeto em questão ou de outro. E trabalhando com concorrência precisamos adicionar muita complexidade para manter sincornizadas todas as threads que utilizam esses objetos concorrentemente.

Eu não estou aqui para provar que você deve sair do seu paradigma atual para o FP (mesmo que algumas pessoas diriam pra você fazer), mas definitivamente você precisa domina-lo: O Java 8 e C++11 incorporaram as expressões lambda em suas plataformas. Posso afirmar que toda linguagem moderna e ainda mantida vai contar com features de FP em breve. E a maioria delas já tem.

Vale a pena mensionar que você não vai parar de usar estados mutáveis. Nós precisamos para desenvolver funcionalidades com IO e etc, para termos bons softwares. Porém, a idéia principal que FP prove é: use estados mutáveis apenas quando for necessário.
