# Então você quer ser um Programador Funcional? (Parte 1)

* **Artigo original**: [So You Want to be a Functional Programmer (Part 1)](https://medium.com/@cscalfani/so-you-want-to-be-a-functional-programmer-part-1-1f15e387e536#.es7crd8y1)
* **Tradução**: [Gabriel](https://github.com/gabriel-ribeiro-ir)

Dar o primeiro passo para entender os conceitos de Programação Funcional é o mais importante e algumas vezes o passo mais díficil. Mas isto não tem de ser assim. Não com a perspectiva correta.

### Aprendendo a dirigir

Quando acabamos de aprender a dirigir, nós fazemos um grande esforço. Parecia fácil quando víamos outras pessoas dirigindo. Mas acaba se tornando mais difícil do que pensamos.

Nós praticamos no carro de nossos pais e não nos aventuramos pela rodovia até dominarmos as ruas de nossa vizinhança.

Mas através de muita prática e alguns momentos de pânico que nossos pais gostariam de esquecer, nós aprendemos a dirigir e finalmente pegamos nossa habilitação.

Com a habilitação em mãos, pegamos o carro em qualquer oportunidade que temos. Com cada viagem, nós melhoramos e nossa confiança aumenta. Então chega o dia em que temos que dirigir o carro de outra pessoa, ou o nosso finalmente entregou os pontos e temos que comprar um novo.

Como foi aquela primeira vez atrás do volante de um **carro diferente**? Foi como a primeiríssima vez? Nem chega perto. Na primeira vez foi tudo tão estranho. Nós já estivemos em um carro antes disso, mas somente como passageiros. Desta vez nós estamos no assento do motorista. Os únicos com todos os controles.

Mas quando dirigimos nosso segundo carro, simplesmente nos perguntamos algumas coisas simples como, onde vai a chave, onde estão os faróis, como usamos a seta e como ajustamos os retrovisores.

Depois disto, dirigimos suavemente. Mas por que desta vez foi tão fácil comparada à primeira?

Porque o novo carro é muito parecido com o antigo. Ele tinha todas as coisas básicas que um carro precisa e eles estavam praticamente no mesmo lugar.

Algumas coisas foram implementadas de forma diferente e talvez tivesse algumas funcionalidades adicionais, mas nós não as usamos na primeira, ou mesmo na segunda vez que dirigimos. Eventualmente, nós aprendemos todas as novas funcionalidades. Pelo menos aquelas que nos importam.

Bem, aprender linguagens de programação é parecido com isto. A primeira é a mais difícil. Mas uma vez que você tenha uma em seu cinturão, as subsequentes são mais fáceis.

Quando você começa a segunda linguagem, você pergunta coisas como, "Como eu crio um módulo? Como faço uma busca em um array? Quais são os parâmetros da função substring?"

Você está confiante que pode aprender esta nova linguagem porque ela te lembra a antiga, talvez com algumas coisas novas que esperançosamente farão sua vida mais fácil.

### Sua primeira Nave Espacial

Se você tem dirigido um carro sua vida toda ou uma dúzia de carros, imagine que você está prestes a estar atrás do volante de uma nave espacial.

Se você estivesse indo voar em uma nave espacial, não poderia esperar que sua habilidade na estrada ajudasse muito. Você estaria começando do zero. (Afinal somos todos programadores. Nós contamos a partir do zero.)

Você começaria seu treinamento com a expectativa de que as coisas são bastante diferentes no espaço e que voar nesta geringonça é muito diferente que dirigir no chão.

A física não mudou. Somente a maneira que você navega dentro do mesmo Universo.

E é a mesma coisa com aprender Programação Funcional. Você deve esperar que as coisas serão um pouco diferentes. E muito do que você sabe sobre programação **não** será traduzível.

Programação é pensar e Programação Funcional te ensinará a pensar diferente. Tanto que você provavelmente nunca mais voltará a pensar da mesma maneira.

### Esqueça tudo o que você sabe

As pessoas amam dizer esta frase, mas é um pouco verdade. **Aprender programação funcional é como começar do zero**. Não completamente, mas efetivamente. Existem muitos conceitos similares mas é melhor esperar que você vai ter de re-aprender tudo.

Com a perspectiva correta você terá as expectativas certas, e com as expectativas certas você não desistirá quando as coisas ficarem difíceis.

Existem todos os tipos de coisas que você está acostumado a fazer como programador que não poderá mais fazer em Programação Funcional.

Assim como no seu carro, você costumava dar a ré para sair da garagem. Mas em uma nave espacial não existe marcha a ré. Agora você pode pensar, "O QUÊ? SEM MARCHA A RÉ?! COMO DIABOS EU DEVERIA DIRIGIR SEM MARCHA A RÉ?!"

Bem, acontece que você não precisa de marcha a ré em uma nave espacial por causa de sua habilidade de manobrar em um espaço tridimensional. Uma vez que você entenda isto, você nunca mais sentirá falta da marcha a ré outra vez. De fato, algum dia, você vai pensar em quanto limitado o carro realmente era.

> Aprender Programação Funcional leva um tempo. Então seja paciente.

Então vamos sair do mundo frio da Programação Imperativa e vamos dar um mergulho suave na fonte quentinha da Programação Funcional.

O que se segue nesta série de artigos são Conceitos de Programação Funcional que ajudarão você antes de mergulhar na sua primeira Linguagem Funcional. Ou se você já mergulhou, vai te ajudar a compreender.

Por favor não corra. Tome seu tempo de leitura deste ponto em diante e não tenha pressa para entender os exemplos de código. Você pode até mesmo parar de ler depois de cada seção para aprofundar as ideias, e então voltar depois para terminar.

O que realmente importa é que você **entenda**.

### Pureza

Quando Programadores Funcionais falam de Pureza, eles estão se referindo a Funções Puras.

Funções Puras são funções simples. Elas somente operam em seus parâmetros de entrada.

Aqui está um exemplo de Função Pura com Javascript:

	var z = 10;
	function add(x, y) {
	  return x + y;
	}

Note que a função **add** NÃO toca na variável z. Ela não lê nem escreve na variável **z**. Ela somente lê **x** e **y**, seus parâmetros, e retorna o resultado da adição deles.

Isto é uma função pura. Se a função **add** tivesse acesso a **z**, não seria mais pura.

Aqui está outra função para considerar:

	function justTen() {
	  return 10;
	}

Se a função **justTen** é pura, então ela pode somente retornar uma constante. Correto?

Pelo motivo de não passarmos parâmetros para ela. E uma vez que, para ser pura, ela não pode acessar nada que não seja seus parâmetros, a única coisa que ela pode retornar é uma constante.

Uma vez que funções puras que não recebem parâmetros não fazem nada, elas não são muito úteis. Seria melhor se **justTen** fosse definida como uma constante.

> As Funções Puras mais úteis recebem pelo menos um parâmetro

Considere esta função:

	function addNoReturn(x, y) {
	  var z = x + y;
	}

Perceba como esta função não retorna nada. Ela adiciona **x** e **y** e guarda na variável **z** mas não a retorna.

É uma funçao pura, já que lida somente com seus parâmetros. Ela faz a adição, mas não retornando seu resultado, é inútil.

> Toda função pura precisa retornar alguma coisa para ser **útil**

Vamos considerar a primeira funçao **add** novamente:

	function add(x, y) {
		return x + y;
	}
	console.log(add(1, 2)); // imprime 3
	console.log(add(1, 2)); // ainda imprime 3
	console.log(add(1, 2)); // SEMPRE vai imprimir 3

Observe que **add(1, 2)** é sempre **3**. Não é uma grande surpresa, mas somente porque a função é pura. Se a função **add** usasse algum valor externo, então você **nunca** poderia prever seu comportamento.

> Funções puras **sempre** produzem a mesma saída dados os mesmos parâmetros.

Visto que funções puras não podem mudar variáveis externas, todas as funções a seguir são **impuras**:

	writeFile(fileName);
	updateDatabaseTable(sqlCmd);
	sendAjaxRequest(ajaxRequest);
	openSocket(ipAddress);

Todas estas funções tem o que é chamado de **Side Effects** (Efeitos colaterais). Quando você as chama, elas alteram arquivos e tabelas de banco de dados, enviam dados para um servidor ou pedem ao SO por um socket. Elas fazem mais do que somente operar em seus parâmetros de entrada e retornar uma saída. Portanto, você *nunca* pode prever o que estas funções retornam.

> Funções puras **não possuem** side effects.

Em linguagens de programação imperativas como Javascript, Java, e C#, os Side Effects estão por toda a parte. Isto torna o debugging mais díficil porque uma variável pode ser alterada em **qualquer lugar** do seu programa. Então quando você tem um bug por causa de uma variável que mudou para o valor errado na hora errada, onde você procura? Por toda a parte? Isto não é bom.

Neste ponto você provavelmente está pensando, "COMO DIABOS EU FAÇO QUALQUER COISA **SOMENTE** COM FUNÇÕES PURAS?!"

Em Programação Funcional você não só escreve Funções Puras.

Linguagens Funcionais não podem eliminar os Side Effects, elas só podem confiná-los. Desde que os programas tem de interagir com o mundo real, algumas partes precisam ser impuras. O objetivo é minimizar a quantidade de código impuro e segregá-lo do resto do seu programa.

### Imutabilidade

Você se lembra de quando viu o seguinte trecho de código:

	var x = 1;
	x = x + 1;

E quem quer que estive te ensinando falou para você esquecer o que você aprendeu em matemática? Na matemática, **x** nunca pode ser igual a **x + 1**.

Mas em Programação Imperativa, isto significa: Pegue o valor atual de **x** e adicione **1** a ele e guarde o resultado de volta no **x**.

Bem, em Programação Funcional, **x = x + 1** é ilegal. Então você deve **relembrar** o que você **esqueceu** da matemática... mais ou menos isto.

> **Não** existem variáveis em Programação Funcional.

Valores guardados ainda são chamados de variáveis por causa da história mas eles são constantes, por exemplo: Uma vez que **x** ganha um valor, será seu valor pela vida toda.

Não se preocupe, **x** é uma variável local, então sua vida será curta. Mas enquanto estiver viva, ela nunca muda.

Aqui está um exemplo de variáveis consantes em Elm, uma Linguagem Funcional Pura para Desenvolvimento Web:

	addOneToSum y z =
		let
			x = 1
		in
			x + y + z

Se você não está acostumdo com a sintaxe ML-Style, permita-me explicar. **addOneToSum** é uma função que recebe 2 parâmetros, **y** e **z**.

Dentro do bloco **let** é atribuido a **x** o valor **1**, isto é, seu valor será igual a **1** pelo resto de sua vida. Sua vida acaba quando a função é encerrada ou mais precisamente quando o bloco **let** é avaliado.

Dentro do bloco **in**, o cálculo pode incluir valores definidos no bloco **let**, vide **x**. O resultado do cálculo **x + y + z** é retornado, ou mais precisamente, **1 + y + z** é retornado já que **x = 1**.

Novamente eu posso te ouvir perguntando "COMO DIABOS EU SUPOSTAMENTE FARIA ALGUMA COISA SEM VARIÁVEIS?!"

Vamos pensar em quando nós queremos modificar variáveis. No geral, existem 2 casos que vem em mente: mudanças multi-valores (ex. mudando um único valor de um objeto ou registro) e mudanças de valor único (ex. contadores de loop).

Programação Funcional lida com mudanças de valores em um registro fazendo uma cópia do registro com os valores alterados. Isto é feito eficientemente sem ter de copiar todas as partes do registro, usando estruturas de dados que torna isso possível.

Programação funcional resolve mudanças de valor único exatamente da mesma forma, fazendo uma cópia dele.

Oh, sim e **não** possuindo loops.

"O QUÊ? SEM VARIÁVEIS E AGORA SEM LOOPS?! EU TE ODEIO!!!"

Calma. Não é que não podemos fazer loops (sem trocadilhos), é só que não existem construtores específicos de loop como **for, while, do, repeat**, etc.

> Programação Funcional usa recursão para fazer looping.

Aqui estão duas maneiras de fazer loops em Javascript:

	// construtor de loop simples
	var acc = 0;
	for (var i = 1; i <= 10; ++i)
		acc += i;
	console.log(acc); // imprime 55

	// sem construtor de loop ou variáveis (recursão)
	function sumRange(start, end, acc) {
		if (start > end)
			return acc;
		return sumRange(start + 1, end, acc + start)
	}
	console.log(sumRange(1, 10, 0)); // imprime 55

Perceba que a recursão, a abordagem funcional, realiza o mesmo que um loop **for** chamando a si mesma com um *novo* start (**start + 1**) e um *novo* acumulador (**acc + start**). Ela não modifica os valores antigos Ao invés disso usa novos valores calculados a partir dos antigos.

Infelizmente, isto é díficil de ver no Javascript, mesmo que você gaste um tempinho estudando, por duas razões. Primeiro, a sintaxe do Javascript é espalhafatosa e segundo, provavelmente você não está acostumado a pensar recursivamente.

No Elm, isto fica mais fácil de ler e por consequência fácil de entender.

	sumRange start end acc =
		if start > end then
			acc
		else
			sumRange (start + 1) end (acc + start) 

Aqui está como é executado:

	sumRange 1 10 0 =      -- sumRange (1 + 1)  10 (0 + 1)
	sumRange 2 10 1 =      -- sumRange (2 + 1)  10 (1 + 2)
	sumRange 3 10 3 =      -- sumRange (3 + 1)  10 (3 + 3)
	sumRange 4 10 6 =      -- sumRange (4 + 1)  10 (6 + 4)
	sumRange 5 10 10 =     -- sumRange (5 + 1)  10 (10 + 5)
	sumRange 6 10 15 =     -- sumRange (6 + 1)  10 (15 + 6)
	sumRange 7 10 21 =     -- sumRange (7 + 1)  10 (21 + 7)
	sumRange 8 10 28 =     -- sumRange (8 + 1)  10 (28 + 8)
	sumRange 9 10 36 =     -- sumRange (9 + 1)  10 (36 + 9)
	sumRange 10 10 45 =    -- sumRange (10 + 1) 10 (45 + 10)
	sumRange 11 10 55 =    -- 11 > 10 => 55
	55

Você provavelmente está pensando que os loops **for** são mais fáceis de entender. Enquanto isto é discutível e mais como uma questão de **familiaridade**, loops não recursivos requerem Mutabilidade, o que é ruim.

Eu não expliquei os benefícios da Imutabilidade aqui mas veja a seção **Global Mutate State** em [Por quê Programadores Precisam de Limites](https://medium.com/@cscalfani/why-programmers-need-limits-3d96e1a0a6db#.awvlo8d5l) para aprender mais.

Um benefício óbvio é que se você tem acesso a um valor em seu programa, você somente tem acesso a leitura, o que significa que ninguém mais pode alterar este valor. Mesmo você. Então sem mutações acidentais.

Além disso, se seu programa é multi-thread, então nenhuma outra thread pode puxar seu tapete. Aquele valor é constante e se outra thread quer mudá-lo, terá que criar um novo valor a partir do antigo.

De volta aos anos 90, eu escrevi uma Game Engine para o [Creature Crunch](https://www.youtube.com/watch?v=uIOYSjBRORM) e a maior fonte de bugs foram em problemas multithreading. Desejava conhecer sobre imutabilidade naquela época. Mas lá atrás eu estava mais preocupado com a diferença entre as velocidades 2x ou 4x de um CD-ROM na performance do jogo.

> Imutabilidade cria código simples e seguro.

### Meu cérebro!!!

Por enquanto chega.

Nas próximas partes deste arigo, eu vou falar sobre Higher-order Functions, Functional Composition, Currying e mais.

A seguir: [Parte 2](https://medium.com/@cscalfani/so-you-want-to-be-a-functional-programmer-part-2-7005682cec4a) (em inglês)

