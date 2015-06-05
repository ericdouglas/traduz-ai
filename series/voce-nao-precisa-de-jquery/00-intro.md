# Você não precisa de jQuery (Não mais)

* **Artigo original**: [You Don't Need jQuery (anymore)](http://blog.garstasio.com/you-dont-need-jquery/why-not/)
* **Tradução**: [Gabriel](https://github.com/BielRibeiro)

**Grande parte** dos desenvolvedores web dependem do jQuery. Entre muitos, jQuery e Javascript parecem ser a mesma coisa. Então, por que você não deveria usá-lo? Por que você deveria parar de usar? Você não simplesmente precisa dele?

Com jQuery tudo fica mais fácil. Você não consegue desenvolver uma web app sólida sem ele. É muito difícil garantir que seu app funcionará corretamente em todos os browsers sem jQuery - A implementação da [Web API](http://en.wikipedia.org/wiki/Web_API) varia muito entre os browsers.
Todos os bons plug-ins que eu preciso, dependem do jQuery de qualquer maneira.

Usei estas desculpas acima durante a maior parte da minha carreira. E algumas delas em um momento eram mesmo boas desculpas. Em 2012, quando assumi a manutenção e o desenvolvimento de uma [grande biblioteca de file upload](https://github.com/FineUploader), [meu primeiro instinto foi o de reescrever tudo usando jQuery](https://github.com/FineUploader/fine-uploader/issues/326), porque eu pensei que aquilo facilitaria minha vida. A comunidade de usuários existente foi contra trazer qualquer dependência terceira para a biblioteca, como alternativa eu fui forçado a lidar com a API nativa do browser. E quer saber? Foi mais fácil colocar o jQuery de lado do que eu imaginei. Eu não precisava do jQuery, e nem você.

## A Armadilha

Há algum tempo atrás, quando eu entrei no mundo do desenvolvimento web, a primeira biblioteca que usei foi o jQuery. Na verdade, nunca me importei em aprender Javascript direito. Eu não tinha a menor idéia de como era a Web API, ou como mexer no DOM precisamente. O jQuery fez tudo por mim. Fui pego por esta enorme lacuna em meu conhecimento quando entrei em um projeto sem jQuery. Eu fui forçado a aprender desenvolvimento web direito, e eu nunca olhei para trás.

Eu cai numa armadilha, que muitos desenvolvedores web novos, casuais e amadores caem. Se primeiramente eu tivesse tomado o tempo para aprender Javascript e a API disponibilizada pelo browser, eu teria evitado muitos problemas. A sequência correta de eventos é:

1. Aprender Javascript
2. Aprender sobre a Web API
3. Aprender jQuery (ou qualquer outro framework/biblioteca que você pode precisar nos projetos)

Alguns começam com o #3 e #1, e o #2 vem somente depois (ou nunca).Se você não entende o que o jQuery na verdade está fazendo por você, haverão muitos dias frustrantes a frente como ?o aparecimento de abstrações furadas?, ou no caso de você não poder usar jQuery em um projeto futuro. Esta é uma armadilha que você precisa evitar se quiser crescer efetivamente como desenvolvedor web.

## Suporte Cross-Browser

Uma das razões mais usadas para valorizar o jQuery é a de arrumar a "DOM API quebrada". Este é um ponto válido, mas somente se você estiver trabalhando com o IE7 ou anterior.
Estamos em 2014, e o suporte a IE7 é raro. A API do DOM não é tão ruim como muitas pessoas a fazem ser. ?Travessia básica?, manipulação, e criação de elementos é consistentemente suportado por todos os browsers modernos.

The Web API as a whole is, more or less, quite advanced and capable since IE8. Of course, there are gaps in pre-IE10 browsers and some very old Safari/WebKit implementations, but these gaps can be filled in, as needed, with small, directed libraries. In many cases, such as with cross-browser file upload support, MutationObserver support, or Web Component support, you'll need to pull in libraries other than jQuery. The point is, jQuery isn't a silver bullet, and there's a good chance that you will only ever really need a very small portion of what jQuery provides.

A Web API como um todo, é mais ou menos, bastante avançada e capaz desde o IE8. É claro, existem falhas nos browsers pre-IE10 e algumas implementações muito antigas do Safari/Webkit, mas estas falhas podem ser solucionadas, conforme a necessidade, com bibliotecas pequenas e específicas. Em alguns casos, como com o suporte cross-browser do file upload, suporte a MutationObserver ou suporte a Web Component, você precisará integrar outras bibliotecas fora o jQuery. O ponto é, jQuery não é bala de prata, e existe uma boa chance que você só vai precisar de uma pequena parte do que o jQuery oferece.

## JavaScript

Another common reason to pull in jQuery is to make up for perceived shortcomings in the underlying language itself (JavaScript). This is one of the most frivolous excuses. It's a bit much to pull in a 3rd-party dependency like jQuery simply for a marginally better way to loop over object properties and array elements. In fact, this is completely unnecessary with the existence of forEach and Object.keys() (both available in IE9+). Or perhaps you think $.inArray() is useful? Not since IE9, where Array.prototype.indexOf was made available as part of ES5. There are many more examples, but I'll save those for a future post.

## Ok, but do I really need to ditch jQuery?

No, of course not. If you feel comfortable with jQuery, and you are familiar enough with how jQuery works its magic, then, by all means, keep using it. This series of blog posts is all about empowering you to make use of the Web API and (when needed) directed libraries to solve problems in your web apps without pulling in large monolithic multi-purpose libraries that go mostly unused.

## Ok, but should I really ditch jQuery?

That's entirely up to you. This series is all about revealing the Web API and vanilla JavaScript. If you have to consistently support ancient browsers and jQuery makes your life easier, then maybe not. But, if you have reasonable browser s

## Coming Up Next

[Selecting elements without jQuery.](http://blog.garstasio.com/you-dont-need-jquery/selectors/) (Em inglês)