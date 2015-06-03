# Você não precisa de jQuery (Não mais)

* **Artigo original**: [You Don't Need jQuery (anymore)](http://blog.garstasio.com/you-dont-need-jquery/why-not/)
* **Tradução**: [Gabriel](https://github.com/BielRibeiro)

**Grande parte** dos desenvolvedores web dependem do jQuery. Entre muitos, jQuery e Javascript parecem ser a mesma coisa. Então, por que você não deveria usá-lo? Por que você deveria parar de usar? Você não simplesmente precisa dele?

jQuery makes everything easier. You can't develop a solid web app without it. It's far too difficult to ensure your app will work properly in all browsers without jQuery - the Web API implementation varies wildly between browsers. All the good plug-ins I need depend on jQuery anyway.

Com jQuery tudo fica mais fácil. Você não consegue desenvolver uma web app sólida sem ele. É muito difícil garantir que seu app funcionará corretamente em todos os browsers sem jQuery - A implementação da [Web API](http://en.wikipedia.org/wiki/Web_API) varia muito entre os browsers.


I bought into the excuses above, for most of my career. And some of them were even good excuses, at one time. When I took over maintenance and development of [a large cross-browser file upload library](https://github.com/FineUploader) in 2012, [my first instinct was to rewrite it all using jQuery](https://github.com/FineUploader/fine-uploader/issues/326), because that would make my life easier, I thought. The existing user community was against bringing any 3rd-party dependencies into the library, so I was forced to deal with the native browser API instead. You know what? It was a lot easier to toss jQuery to the side than I imagined. I didn't need jQuery, and neither do you.

## The Trap

A while back, when I entered the world of web development, the first library I used was jQuery. In fact, I didn't even bother to learn proper JavaScript. I didn't have a clue what the Web API looked like, or how to deal with the DOM directly. jQuery did everything for me. This huge gap in my knowledge caught up with me when I ended up on a project without my jQuery crutch. I was forced to learn proper web development, and I never looked back.

I fell into a trap, and this is a trap that many new, occasional, and hobbyist web developers fall into. Had I taken the time to understand JavaScript and the API provided by the browser first, I would have saved myself a lot of trouble. The proper sequence of events is:

1. Learn JavaScript
2. Learn the Web API
3. Learn jQuery (or any other framework/library that you may need across projects)

Many start with #3, and #1 & #2 come much later (or never). If you don't understand what jQuery is actually doing for you, there will be many frustrating days ahead as the leaky abstractions come out of the woodwork, or if you are unable to use jQuery in a future project. This is a trap you must avoid if you want to effectively grow as a web developer.

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