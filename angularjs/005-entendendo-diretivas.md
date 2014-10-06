# Entendendo Diretivas

* **Artigo Original**: [Understanding Directives](https://github.com/angular/angular.js/wiki/Understanding-Directives)
* **Tradução**: [Eric Douglas](https://github.com/ericdouglas)

Este documento é uma tentativa de explicar como as Diretivas no AngularJS e suas engines compiladoras relacionadas trabalham, para que você não sinta que está se deparando com *um macarrão* a primeira vez que tentar resolver isso sozinho.

## Injetando, Compilando e Linkando funções

Quando você cria uma diretiva, existem essencialmente 3 camadas de função que para você definir:

```js
myApp.directive('uiJq', function InjectingFunction() {

  // === InjectingFunction === //
  // Lógica é executada 0 ou 1 vez por app
  // (dependendo se a diretiva é usada)
  // Útil para inicialização e configuração global

  return {
    compile: function CompilingFunction($templateElement, $templateAttributes) {
      
      // === CompilingFunction === //
      // Lógica é executada uma vez para cada instância de ui-jq no seu template original NÃO RENDERIZADO
      // Escopo está INDISPONÍVEL quando os modelos estão sendo cacheados.
      // Você PODE examinar o DOM e informação de cache sobre as variáveis ou expressões
      //  que você vai usar, mas ainda não consegue descobrir seus valores
      // Angular está cacheando os templates, agora é uma boa hora de injetar novos templates do Angular
      //  como filhos ou futuros irmãos para rodarem automaticamente.

      return function LinkingFunction($scope, $linkElement, $linkAttributes) {
        
        // === LinkingFunction === //
        // A lógica é executada uma vez para cada instância renderizada
        // 
      };

    }
  };

});
