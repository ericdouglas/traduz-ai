# Behaviour Driven Development com JavaScript

* **Artigo original**: [Behaviour Driven Development with JavaScript](http://gajus.com/blog/1/behaviour-driven-development-with-javascript)
* **Tradução**: [Eric Douglas](https://github.com/ericdouglas)

Este artigo é resultado de uma extensa pesquisa sobre BDD no JavaScript. Eu extraí o núcleo principal e a terminologia, e forneci exemplos práticos para ilustrar os benefícios do BDD.

## O que é BDD?
No capítulo 8 do livro *"Behaviour Driven Development with JavaScript"* por Marco Emrich, o autor cita várias definições de BDD. Eu destaco as seguintes:

> [BDD] amplia o TDD escrevendo casos de teste em uma linguagem natural que não programadores podem ler.
>
> **- M. Manca**

> BDD é uma metodologia ágil da segunda geração, de fora para dentro (*outside-in*), que envolve múltiplas partes interessadas, de várias escalas e com alta automatização. O BDD descreve um ciclo de interações com saídas bem definidas, resultando na entrega de software que funciona, software testado e software que realmente importa.
>
> **- Dan North**

Desenvolvedores puxam as funcionalidades dos *stakeholders* (partes interessadas) e as desenvolvem de fora para dentro. As funcionalidades são puxadas na orderm de seu valor de negócio. A funcionalidade mais importante é implementada primeiro e ela é implementada partindo das partes superficiais até suas partes mais específicas. Nenhum tempo é gasto construindo código desnecessário.

A evolução de um programador que está aprendendo TDD é um bom guia para entender o BDD:

1. A maioria dos desenvolvedores começam escrevendo testes unitários para código existe.
1. Depois de ter alguma prática eles aprendem sobre as vantagens de escrever o teste primeiro. Eles podem ter aprendido sobre boas práticas de testes como AAA, *[stubs](https://en.wikipedia.org/wiki/Test_stub)* (programas que simulam o comportamento de outros módulos) e *factories*.
1. Eles percebem que TDD pode ser usado como uma ferramenta de projeto para descobrir a API do código em produção.
1. Eventualmente eles percebem que TDD não é sobre testes. TDD é sobre definir comportamento e escrever especificações que servem como documentações vivas. Eles também descobrem *mocks* e desenvolvimento *outside-in*.

## Princípios do BDD
### *Baby steps* (passos de bebê)
Uma funcionalidade característica do BDD é a utilização de *"baby steps"* para obter um ciclo de *feedback* rápido. Na prática, isso significa escrever uma especificação simples que vai falhar, escrever a lógica do código faltante e retornar para a especificação. Essa troca contínua entre código e casos de teste resulta em menos verificações do código.

### *Red/Green/Refactor* (Vermelho/Verde/Refatorar)
Você quer alcançar essa rotina *Red/Green/Refactor*:

- Todas as especificações estão verde (depois da fase da implementação)?
- Tem uma especificação vermelha (depois de codificar outra especificação)?

A ideia é assegurar que o teste realmente funciona e pode pegar um erro.

### Triangulação
Ao invés de criar uma implementação real, você pode retornar um valor estático.

```js
var productBasket = function (products) {
    return {
        total: function () {
            return 0;
        }
    }
};

describe('Product basket', function () {
    describe('#total()', function () {
        it('returns 0 when basket is empty', function () {
            expect(productBasket([]).total()).toEqual(0);
        });
    })
});
```

Você sabe que sua implementação não está pronta, mas todas as especificações estão verdes. Isso implica que existe alguma espeficação faltando.

```js
var productBasket = function (products) {
    return {
        total: function () {
            if (!products.length) {
                return 0;
            } else {
                return products[0]
            }
        }
    }
};

describe('Product basket', function () {
    describe('#total()', function () {
        it('returns 0 when basket is empty', function () {
            expect(productBasket([]).total()).toEqual(0);
        });
        it('returns price of a single product in the basket', function () {
            expect(productBasket([10]).total()).toEqual(10);
        });
    })
});
```

Você prossegue até que tenha coberto casos de teste suficientes para produzir uma solução geral.

```js
var productBasket = function (products) {
    return {
        total: function () {
            return products.reduce(function (prev, curr) {
                return prev + curr;
            }, 0);
        }
    }
};

describe('Product basket', function () {
    describe('#total()', function () {
        it('returns 0 when basket is empty', function () {
            expect(productBasket([]).total()).toEqual(0);
        });
        it('returns price of a single product in the basket', function () {
            expect(productBasket([10]).total()).toEqual(10);
        });
        it('returns price of multiple products in the basket', function () {
            expect(productBasket([10, 20, 30]).total()).toEqual(60);
        });
    })
});
```

A ideia é ganhar um entendimento de como o algoritmo deve se comportar.

#### Resumo
*Behaviour Driven Development* é caracterizado por:

- Código orientado por especificação/erro.
- Uso de *"baby steps"* para alcançar um ciclo de *feedback* rápido.
- Adoção do ciclo *Red-Green-Refactor (RGR)* para evitar falsos positivos.

O resultado:

- Projeto de software orientado pelas necessidades.
- O ciclo RGR melhora a separação da carga de trabalho. *Red* é para a interface, *Green* é para a implementação.
- Especificações permitem refatoração de código.
- Testes automatizados que podem ser lidos como documentação. Prefira DAMP ao invés de DRY.

> **Refatoração**: alteração da estrutura do código interno sem prejuízo de comportamento externo.
>
> **DAMP**: Descriptive and Meaningful Phrases. (Frases descritivas e significativas)

## BDD na Prática
### Usando *Test Doubles*
#### *Dummies*
Objeto vazio que não faz nada.

- Objetos são passados mas nunca são utilizados de fato. Normalmente apenas são usados para completar uma lista de parâmetros. <a href="http://martinfowler.com/articles/mocksArentStubs.html"><sup>1</sup></a>

```js
describe('Product basket', function () {
    var productBasket;

    beforeEach(function () {
        productBasket = new ProductBasket();
    });

    describe('.productCount()', function () {
        it('returns number of products in the basket', function () {
            var productDummy1 = {},
                productDummy2 = {};

            productBasket.add(productDummy1);
            productBasket.add(productDummy2);

            expect(productBasket.productCount()).toEqual(2);
        });
    });
});
```

#### *Stubs*
Método projetado apenas para teste.

- Objetos *stub* fornecem uma resposta válida, mas ela é estática - não importa o valor que você passar, você sempre receberá a mesma resposta. <a href="http://stackoverflow.com/a/1830000/368691"><sup>2</sup></a>
- Um objeto que oferece respostas pré-definidas para chamadas de um método. <a href="http://stackoverflow.com/a/5180286/368691"><sup>3</sup></a>
- *Stubs* fornecem *canned answers* (respostas enlatadas) para chamadas feitas durante o teste, normalmente não respondendo a nada fora do que foi programado para o teste. *Stubs* também podem registrar informações sobre chamadas, como um *email gateway stub* que lembra as mensagens que ele "enviou", ou talvez apenas a quantidade de mensagens que ele "enviou". <a href="http://martinfowler.com/articles/mocksArentStubs.html"><sup>4</sup></a>
- Um *stub* nunca vai fazer um teste falhar. <a href="http://ayende.com/Wiki/Rhino+Mocks+3.5.ashx"><sup>5</sup></a>
- Um *stub* é um objeto artificial simples. Ele apenas assegura que os testes rodem suavemente/facilmente. <a href="http://stackoverflow.com/a/3459431/368691"><sup>6</sup></a>

```js
describe('Product', function () {
    var product;

    beforeEach(function () {
        product = new Product({name: 'foo'});
    });

    describe('.getName()', function () {
        it('returns name formatted using format function', function () {
            var formatFunctionStub = function (productName) { return productName + '-bar'; }

            expect(product.getName(formatFunctionStub)).toEqual('foo-bar');
        });
    });
});
```

#### *Mocks*
Objeto projetado apenas para teste.

- Objetos *mock* são usados em simulações de casos de teste - eles validam se certos métodos foram chamados nesses objetos.
- Um objeto no qual você define expectativas.

```js
describe('Product basket', function () {
    var productBasket;

    beforeEach(function () {
        productBasket = new ProductBasket();
    });

    describe('.getTotal()', function () {
        it('returns sum of product.getPrice()', function () {
            var productDummy1 = {getPrice: function () { return 10; }},
                productDummy2 = {getPrice: function () { return 20; }};

            productBasket.add(productDummy1);
            productBasket.add(productDummy2);

            expect(productBasket.getTotal()).toEqual(30);
        });
    });
});
```

### *Example Factory*
Método usado para instanciar o objeto SUT (*System Under Test* - sistema em teste) com valores canônicos, sobrescrevendo apenas as propriedades relevantes para o caso em teste.

```js
var SeminarFactory = {
    create: function (overwrite) {
        var defaultData,
            objectData;

        defaultData = {
            name: 'JavaScript basics',
            price: 100
        };

        objectData = Object.extend(defaultData, overwrite);

        return Seminar.create(objectData.name, objectData.price);
    }
};

describe('Seminar', function () {
    it('has a name', function () {
        var seminar = SeminarFactory.create({name: 'JavaScript'});

        expect(seminar.getName()).toEqual('JavaScript');
    });

    it('has a price', function () {
        var seminar = SeminarFactory.create({price: 10});

        expect(seminar.getPrice()).toEqual(10);
    });
});
```

###  Padrão SEE
Toda especificação precisa destas três partes e nada mais - nessa ordem:

1. *Setup* (configurar), e.g. criar um objeto.
1. *Execute* (executar), i.e introduzir mudanças estado do objeto.
1. *Expect* (esperar), i.e. declarar uma expectativa do que deveria ter ocorrido.

Você pode ter ouvido também sobre **Given, When, Then** (GWT):

- *Given* (dado) uma condição.
- *When* (quando) eu faço algo.
- *Then* (então) eu espero que algo aconteça.

Os dois são idênticos. O fraseado da última pode ser mais fácil de entender.

```js
describe('Division by Zero', function () {
    // Setup (or Given)
    var number;

    beforeEach(function () {
        number = 10;
    });

    describe('when dividing a number by zero', function () {
        // Execute (or When)
        it('we get Infinity', function () {
            // Expect (or Then)
            expect(number/0).toEqual(Infinity);
        });
    });
});
```

As a result, your specs must
