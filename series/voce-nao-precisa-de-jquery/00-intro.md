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

Many start with #3, and #1 & #2 come much later (or never). If you don't understand what jQuery is actually doing for you, there will be many frustrating days ahead as the leaky abstractions come out of the woodwork, or if you are unable to use jQuery in a future project. This is a trap you must avoid if you want to effectively grow as a web developer.

Alguns começam com o #3 e #1, e o #2 vem somente depois (ou nunca).

## Cross-Browser Support

One of the most touted reasons for pulling in jQuery is to fix the "broken DOM API". This is a valid point, but only if you are working with IE7 or older. It's now 2014, and IE7 support is rare. The DOM API isn't really as bad as many people make it out to be. Basic traversal, manipulation, and creation of elements is straightforward and consistently supported in all modern browsers.

The Web API as a whole is, more or less, quite advanced and capable since IE8. Of course, there are gaps in pre-IE10 browsers and some very old Safari/WebKit implementations, but these gaps can be filled in, as needed, with small, directed libraries. In many cases, such as with cross-browser file upload support, MutationObserver support, or Web Component support, you'll need to pull in libraries other than jQuery. The point is, jQuery isn't a silver bullet, and there's a good chance that you will only ever really need a very small portion of what jQuery provides.

## JavaScript

Another common reason to pull in jQuery is to make up for perceived shortcomings in the underlying language itself (JavaScript). This is one of the most frivolous excuses. It's a bit much to pull in a 3rd-party dependency like jQuery simply for a marginally better way to loop over object properties and array elements. In fact, this is completely unnecessary with the existence of forEach and Object.keys() (both available in IE9+). Or perhaps you think $.inArray() is useful? Not since IE9, where Array.prototype.indexOf was made available as part of ES5. There are many more examples, but I'll save those for a future post.

## Ok, but do I really need to ditch jQuery?

No, of course not. If you feel comfortable with jQuery, and you are familiar enough with how jQuery works its magic, then, by all means, keep using it. This series of blog posts is all about empowering you to make use of the Web API and (when needed) directed libraries to solve problems in your web apps without pulling in large monolithic multi-purpose libraries that go mostly unused.

## Ok, but should I really ditch jQuery?

That's entirely up to you. This series is all about revealing the Web API and vanilla JavaScript. If you have to consistently support ancient browsers and jQuery makes your life easier, then maybe not. But, if you have reasonable browser s

## Coming Up Next

[Selecting elements without jQuery.](http://blog.garstasio.com/you-dont-need-jquery/selectors/) (Em inglês)