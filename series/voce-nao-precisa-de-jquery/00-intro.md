# Você não precisa de jQuery (Não mais)

* **Artigo original**: [You Don't Need jQuery (anymore)](http://blog.garstasio.com/you-dont-need-jquery/why-not/)
* **Tradução**: [Gabriel](https://github.com/BielRibeiro)

**Muitos** desenvolvedores web dependem do jQuery. Para muitos, jQuery e Javascript parecem ser a mesma coisa. Então, por que não usá-lo? Por que parar de usá-lo? Você simplesmente não precisa dele?

Com jQuery tudo fica mais fácil. Você não consegue desenvolver uma web app sólida sem ele. É muito difícil garantir que seu app funcionará corretamente em todos os browsers sem jQuery - A implementação da [Web API](http://en.wikipedia.org/wiki/Web_API) varia muito entre os browsers.
Todos os bons plug-ins que eu preciso, dependem do jQuery de qualquer maneira.

Usei estas desculpas acima durante a maior parte da minha carreira. E algumas delas foram, por um momento, boas desculpas. Em 2012, quando assumi a manutenção e o desenvolvimento de uma [grande biblioteca de file upload](https://github.com/FineUploader), [meu primeiro instinto foi o de reescrever tudo usando jQuery](https://github.com/FineUploader/fine-uploader/issues/326), porque eu pensei que aquilo facilitaria minha vida. A comunidade de usuários existente foi contra trazer qualquer outra dependência para a biblioteca, então fui forçado a lidar com a API nativa do browser. E quer saber? Foi mais fácil colocar o jQuery de lado do que eu imaginei. Eu não precisava do jQuery, e nem você precisa.

## A Armadilha

Há algum tempo, quando entrei no mundo do desenvolvimento web, a primeira biblioteca que usei foi o jQuery. Na verdade, nunca me importei em aprender Javascript direito. Eu não tinha a menor ideia de como era a Web API, ou como mexer no DOM precisamente. O jQuery fez tudo por mim. Fui pego por esta enorme lacuna em meu conhecimento quando entrei em um projeto sem jQuery. Fui forçado a aprender desenvolvimento web direito, e nunca mais olhei para trás.

Cai numa armadilha que muitos desenvolvedores web novos, casuais e amadores caem. Se primeiramente tivesse gasto meu tempo para aprender Javascript e a API do browser, teria evitado muitos problemas. A sequência correta de estudos é:

1. Aprender Javascript
2. Aprender sobre a Web API
3. Aprender jQuery (ou qualquer outro framework/biblioteca que você pode precisar nos projetos)

Alguns começam com o #3 e #1, e o #2 vem somente depois (ou nunca). Se você não entende o que o jQuery na verdade está fazendo por você, haverão muitos dias frustrantes a frente, descobrindo ["vazamentos" em sua abstração](http://www.joelonsoftware.com/articles/LeakyAbstractions.html), ou no caso de você não poder usar jQuery em um projeto futuro. Esta é uma armadilha que você precisa evitar se quiser crescer efetivamente como desenvolvedor web.

## Suporte Cross-Browser

Uma das razões mais usadas para valorizar o jQuery é a de arrumar a API fraca do DOM. Este é um ponto válido, mas somente se você estiver trabalhando com o IE7 ou anterior.
Estamos em 2014, e o suporte a IE7 é raro. A API do DOM não é tão ruim como muitas pessoas costuman dizer. Percorrer, manipular e criar elementos é consistentemente suportado por todos os browsers modernos.

A Web API como um todo é, de certa forma, bastante avançada e capaz desde o IE8. É claro, existem falhas nos browsers pré-IE10 e em algumas implementações muito antigas do Safari/Webkit, mas estas falhas podem ser solucionadas, conforme a necessidade, com bibliotecas pequenas e específicas. Em alguns casos, como o suporte cross-browser do file upload, suporte a MutationObserver ou suporte a Web Component, você precisará integrar outras bibliotecas fora o jQuery. O ponto é que jQuery não é bala de prata, e existe uma boa chance de você precisar somente de uma pequena parte do que o jQuery oferece.

## JavaScript

Outra razão comum para se usar o jQuery, é a de usar atalhos para a linguagem que está por debaixo dos panos (Javascript). Esta é uma das desculpas mais esfarrapadas. É um pouco demais usar uma dependência como o jQuery simplesmente por uma maneira "melhor" de percorrer as propriedades de um objeto ou um array de elementos. Na verdade, isto é completamente desnecessário levando em conta a existência de forEach e Object.keys() (ambos disponíveis em IE9+). Ou talvez você ache que inArray() é útil? Não desde o IE9, onde [Array.prototype.indexOf](http://www.ecma-international.org/ecma-262/5.1/#sec-15.4.4.14) ficou disponível como parte do ES5. Existem muitos exemplos, mas eu vou guardá-los para um próximo post.

## Ok, mas eu preciso mesmo abandonar o jQuery?

Mas é claro que não. Se você se sente confortável com o jQuery, e está familiar o suficiente em como o jQuery faz sua mágica, então continue usando.
Esta série de posts é sobre como lhe capacitar para usar a Web API e (quando preciso) bibliotecas específicas para resolver problemas em seus web apps sem depender de grandes bibliotecas com vários propósitos que, na sua maioria, nem serão utilizados.

## Ok, mas eu devo abandonar o jQuery?

Isto é com você. Esta série toda é para mostrar a Web API e o vanilla Javascript. A resposta é **não** se você precisa dar suporte a browsers muito antigos e o jQuery faz sua vida mais fácil. Porém, se você pode suportar versões de browsers mais atuais, e/ou fica feliz em escrever seu próprio código, deixe o jQuery de lado.

## Próximo Post

[Selecting elements without jQuery.](http://blog.garstasio.com/you-dont-need-jquery/selectors/) (Em inglês)

[Selecionando elementos sem jQuery.](01-seletores.md) (Em português)