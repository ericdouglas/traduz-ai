# Uma Suave Introdução ao JavaScript Funcional: Parte 4

* **Artigo Original**: [A GENTLE INTRODUCTION TO FUNCTIONAL JAVASCRIPT: PART 4](http://jrsinclair.com/articles/2016/gentle-introduction-to-functional-javascript-style/)
* **Tradução**: [Eric Douglas](https://github.com/ericdouglas)

> *Escrito por James Sinclair em 11 de Fevereiro de 2016* 

Essa é a parte 4 de uma série de 4 artigos sobre introdução a programação funcional no JavaScript. No último artigo vamos ver sobre *high-order functions* (funções de ordem superior): funções para criar funções. Neste artigo, nós discutimos como usar essas novas ferramentas com estilo. 

- [Parte 1: Blocos fundamentais e motivação](009-uma-suave-introducao-ao-javascript-parte-1.md)
- [Parte 2: Trabalhando com Arrays e Listas](010-uma-suave-introducao-ao-javascript-parte-2.md)
- [Parte 3: Funções para fazer funções](011-uma-suave-introducao-ao-javascript-parte-3.md)
- Parte 4: Fazendo isso com estilo (esse artigo)

## Fazendo Isso Com Estilo
No artigo anterior vimos sobre `partial`, `compoese`, `curry`, `pipe`, e também como podemos usá-las para juntar pequenas, simples funções e criar outras grandes e complicadas. Mas o que isso faz pra gente? Vale a pena se importar quando já estamos escrevendo código perfeitamente válido?

Parte da resposta é que sempre é útil ter mais ferramentas disponíveis para realizar o trabalho - desde que você saiba como usá-las - e programação funcional certamente nos dá um conjunto útil de ferramentas para escrever JavaScript. Mas eu penso que há mais do que isso. Programação Funcional nos abre um diferente *estilo* de programação. Isso por sua vez nos permite conceitualizar problemas e soluções de formas diferentes.

Existem duas funcionalidades chave para programação funcional:

1. Escrever funções puras, o que é importante se você deseja testar programação funcional; e
1. Estilo de programação *pointfree* (sem pontos), o que não é *tão* importante mas bom de se entender.

### Pureza
Se você ler sobre programação funcional, você eventualmente vai se deparar com o conceito de funções *puras* e *impuras*. Funções puras são funções que preenchem dois critérios:

1. Chamar a função com as mesmas entradas sempre *retorna* as mesmas saídas.
1. Chamar a função não produz efeitos colaterais: Sem chamadas na rede (*network*); nenhuma leitura ou escrita de arquivos; nenhuma consulta em banco de dados; nenhum elemento DOM modificado; nenhuma modificação de variáveis globais; e nenhuma saída no console. Nada.

Funções impuras deixam programadores funcionais desconfortáveis. Tão desconfortáveis que eles as evitam o máximo possível. O problema com isso é que o grande ponto de escrever programas de computador *são* os efeitos colaterais. Fazer uma chamada na rede e renderizar elementos DOM está no núcleo do que uma aplicação web faz; esse foi o motivo pelo qual o JavaScript foi inventado.

Então o que um aspirante a programador funcional faz? Bom, a chave é que nós não evitamos funções impuras inteiramente, nós apenas damos a elas uma boa quantidade de respeito, e deixá-mos de lidar com elas até que nós absolutamente precisemos. Elaboramos um plano claro e testado para o que queremos fazer *antes* de tentar fazer isso. Como Eric Elliot colocou em [*The Dao of Immutability*](https://medium.com/javascript-scene/the-dao-of-immutability-9f91a70c88cd):

> Separation: Logic is thought. Effects are action
