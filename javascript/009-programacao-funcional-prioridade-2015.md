# Programação Funcional Deve Ser Sua Prioridade número #1 em 2015
## OOP não pode mais nos salvar do *Cloud Monster*

* **Artigo Original**: [Functional Programming should be your #1 priority for 2015](https://medium.com/@jugoncalves/functional-programming-should-be-your-1-priority-for-2015-47dd4641d6b9)
* **Tradução**: [Eric Douglas](https://github.com/ericdouglas) e [Diogo Beato](https://github.com/diogobeato)

Você provavelmente ouviu expressões como "Clojure", "Scala", "Erlang", ou mesmo "Java agora tem lambdas". E você deve saber que isso tem alguma coisa a ver com "Programação Funcional". Se você participa de qualquer comunidade de programação, esse assunto provavelmente apareceu alguma vez.

Se você já buscou por "Functional Programming" (ou Programação Funcional), verá que não existe nada de novo. A segunda linguagem criada já utilizava isso, apareceu por volta dos anos 50, denominada *Lisp*. Por que então as pessoas estão empolgadas com isso apenas agora? 60 anos depois?!

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

## Object-oriented Programming cannot save us anymore