# Angular 1: Alura

> Curso alura sobre angular 1

<!-- TOC -->

- [Angular 1: Alura](#angular-1-alura)
  - [Descrição](#descrição)
  - [Módulos](#módulos)
  - [Templates](#templates)
  - [Controllers](#controllers)
    - [Data-Binding](#data-binding)
  - [Diretivas](#diretivas)
    - [ng-repeat](#ng-repeat)
  - [$http](#http)
  - [Diretivas personalizadas](#diretivas-personalizadas)
    - [Manipulação de DOM](#manipulação-de-dom)
      - [$broadcast](#broadcast)
    - [RootScope](#rootscope)
    - [Templates externos](#templates-externos)
    - [Diretivas que buscam dados](#diretivas-que-buscam-dados)
  - [Filtrando dados](#filtrando-dados)
    - [Utilizando Models](#utilizando-models)
      - [Pseudo-classes](#pseudo-classes)
  - [Views, rotas e partials](#views-rotas-e-partials)
    - [LocationProvider e modo HTML5](#locationprovider-e-modo-html5)
    - [Wildcards](#wildcards)
  - [Formulários com AngularJS](#formulários-com-angularjs)
    - [Validação de Formulários](#validação-de-formulários)
      - [ng-options](#ng-options)
      - [Outras diretivas](#outras-diretivas)
  - [ngResource](#ngresource)
    - [GET](#get)
    - [DELETE](#delete)
    - [PUT](#put)
  - [Serviços](#serviços)
    - [Utilizando promisses com serviços](#utilizando-promisses-com-serviços)
  - [Blindagem de minificação](#blindagem-de-minificação)

<!-- /TOC -->

## Descrição

Quando desenvolvemos para back-end geralmente temos uma organização de códigos voltado a um paradigma MVC.

Quando estes desenvolvedores back-end se viram a desenvolver front-end, este código não é muito bem organizado, desta forma temos frameworks que visam trazer o paradigma para o ambiente do client.

Um dos frameworks mais famosos é o Angular.js

## Módulos

O angular é baseado na criação de módulos, como no modelo MVC, o módulo principal é comumente chamado de _main.js_ e fica na pasta _js_, é nele que serão definidos todos os outros módulos secundários ou qualquer outro tipo de configuração.

Inicialmente a sintaxe do angular inicia com a seguinte linha:

```js
angular.module('alurapic', []);
```

Isso significa que estamos criando um módulo chamado AluraPic sem nenhuma dependencia.

Para podermos ligar este módulo com nosso html, vamos adicionar um atributo chamado `ng-app` no nosso html:

```html
<!DOCTYPE html>
<html lang="pt-br" ng-app="alurapic">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width">
        <title>Alurapic</title>
        <link rel="stylesheet" href="css/bootstrap.min.css">
        <link rel="stylesheet" href="css/bootstrap-theme.min.css">
        <script src="js/lib/angular.min.js"></script>
        <script src="js/main.js"></script>
    </head>
    <body>
        <div class="container">
            <h1 class="text-center">Alurapic</h1>
        </div> <!-- fim container -->        
    </body>
</html>
```

Desta forma o Angular saberá qual é o módulo principal da aplicação, registrando que esta página estará ligada ao módulo do angular.

## Templates

Templates são modelos de visualização aonde lacunas são preenchidas no modelo principal. Por exemplo, vamos adicionar uma imagem ao nosso html, essa imagem pode ser fixa ou dinamica, se quisermos que ela seja dinamica podemos apenas criar uma anotação de que, ali, a imagem será alterada:

```html
<!DOCTYPE html>
<html lang="pt-br" ng-app="alurapic">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width">
        <title>Alurapic</title>
        <link rel="stylesheet" href="css/bootstrap.min.css">
        <link rel="stylesheet" href="css/bootstrap-theme.min.css">
        <script src="js/lib/angular.min.js"></script>
        <script src="js/main.js"></script>
    </head>
    <body>
        <div class="container">
            <h1 class="text-center">Alurapic</h1>
            <img class="img-responsive center-block" src="{{foto.url}}" alt="{{foto.titulo}}">
        </div> <!-- fim container -->        
    </body>
</html>
```

> Perceba o `{{foto.url}}`, que é um objeto dentro do nosso controlador do angular.

## Controllers

Para gerenciar as _angular expressions_ (que são os atributos com as chaves duplas), vamos precisar de um controller.

É uma boa prática criar cada parte do modelo angular em uma pasta e em um arquivo separado, pois facilita a manutenção.

Quando criamos um controller, precisamos ligar o mesmo a um módulo existente, para isto vamos repetir em cada arquivo a instrução `angular.module('alurapic')` porém sem os `[]`, pois não estamos definindo um novo objeto e sim atrelando coisas a um já existente.

Todo controller é definido por um nome e uma função:

```js
angular.module('alurapic')

  .controller("FotosController", function () {
    var foto = {
      titulo: "Leão",
      url: "http://www.fundosanimais.com/Minis/leoes.jpg"
    };
    
  });
```

Para ligarmos um controller a uma parte do nosso DOM, usamos a diretiva `ng-controller`

```html
<body ng-controller="FotosController">
        <div class="container">
            <h1 class="text-center">Alurapic</h1>
            <img class="img-responsive center-block" ng-src="{{foto.url}}" alt="{{foto.titulo}}">
        </div> <!-- fim container -->        
</body>
```

Além de termos de realizar isso, precisamos também definir um escopo para o controller, usamos a dependencia `$scope`:

```js
angular.module('alurapic')

  .controller("FotosController", function ($scope) {
    var foto = {
      titulo: "Leão",
      url: "http://www.fundosanimais.com/Minis/leoes.jpg"
    };
  });
```

Isso define nosso escopo do controller na view, ou seja, o `$scope` é o dominio de acesso da view, tudo que é disponibilizado pelo controller em `$scope` é visivel na view.

```js
angular.module('alurapic')

  .controller("FotosController", function ($scope) {
    $scope.foto = {
      titulo: "Leão",
      url: "http://www.fundosanimais.com/Minis/leoes.jpg"
    };
  });
```

Note que definimos uma nova variável como uma propriedade do `$scope`.

### Data-Binding

Angular possui um termo apropriado para associação de um dado disponibilizado por um controller para a view: data binding (associação/ligação de dados). Qualquer alteração no dado do controller dispara uma atualização da view sem que o desenvolvedor tenha que se preocupar ou intervir no processo.

Excelente! Conseguimos um resultado semelhante ao que tínhamos antes, com a diferença de que agora a AE (Angular Expression) de nossa view foi processada com os dados fornecidos por `FotosController`. Pode parecer pouco, mas isso abre a porteira para que possamos avançar ainda mais no framework da Google.

## Diretivas

### ng-repeat

É um loop baseado em uma ou mais propriedades de um controller.

Basicamente a diretiva `ng-repeat` fica colocada no elemento que será repetido.

No nosso controller temos uma lista de fotos:

```js
.controller("FotosController", function ($scope) {
  $scope.fotos = [{
    titulo: "Leão",
    url: "http://www.fundosanimais.com/Minis/leoes.jpg"
  },
  {
    titulo: "Leão2",
    url: "http://www.fundosanimais.com/Minis/leoes.jpg"
    },
  {
    titulo: "Leão3",
    url: "http://www.fundosanimais.com/Minis/leoes.jpg"
  },
  {
    titulo: "Leão4",
    url: "http://www.fundosanimais.com/Minis/leoes.jpg"
  }];
});
```

E então em nossa view, podemos usar a diretiva `ng-repeat`:

```html
<div class="panel panel-default" ng-repeat="foto in fotos">
                <div class="panel-heading">
                    <h3 class="panel-title text-center">{{foto.titulo}}</h3>
                </div>
                <div class="panel-body">
                    <img class="img-responsive center-block" src="{{foto.url}}" alt="{{foto.titulo}}">
                </div><!-- fim panel-body -->
            </div><!-- fim panel panel-default -->
```

Perceba que `ng-repeat="foto in fotos"` não é uma angular expression, mas sim uma expressão javascript.

## $http

O `$http` é um serviço de chamadas HTTP, ele é muito utilizado para chamadas ReST em API's.

Para preencher nossa lista, vamos utilizar uma URL própria que já temos em nossa base de dados simples.

Para injetarmos o `$http` no controller, vamos ter que colocar novamente uma nova dependencia no controller.

```js
angular.module('alurapic')

.controller("FotosController", function ($scope, $http) {
  $scope.fotos = [{
    titulo: "Leão",
    url: "http://www.fundosanimais.com/Minis/leoes.jpg"
  },
  {
    titulo: "Leão2",
    url: "http://www.fundosanimais.com/Minis/leoes.jpg"
    },
  {
    titulo: "Leão3",
    url: "http://www.fundosanimais.com/Minis/leoes.jpg"
  },
  {
    titulo: "Leão4",
    url: "http://www.fundosanimais.com/Minis/leoes.jpg"
  }];
});
```

Vamos transformar o nosso array de fotos em um objeto em branco: `$scope.fotos = [];`

Agora fazemos nossa requisição:

```js
$http.get('v1/fotos')
      .then(function (fotos) {
        $scope.fotos = fotos.data;
      })
      .catch(function (err) { 
        console.error(err);
      });
```

O `$http` é uma promise, ou seja, ela é uma chamada assíncrona, que retorna uma promessa de execução.

Existe um modelo menos verboso que podemos usar para escrever menos:

```js
$http.get('v1/fotos')
      .success(function (fotos) {
        $scope.fotos = fotos;
      })
      .error(function (err) { 
        console.error(err);
      });
```

## Diretivas personalizadas

Diretivas personalizadas são usadas para diminuir a complexidade de uma marcação HTML ou um novo elemento.

Vamos criar um novo módulo de diretivas em um novo arquivo. Depois vamos marcar que o módulo principal é dependente deste novo módulo de diretivas `angular.module('alurapic', ['all-directives']);`.

Uma diretiva deve __sempre__ retornar um DDO (_Directive Definition Object_).

O DDO contém algumas opções:

- _Restrict_: Podem ser combinados (`AE`)
  - `A`: Attribute, define um novo atributo
  - `E`: Element, define um novo elemento HTML
- _Scope_: É o contexto na qual a diretiva está inserida, aqui é aonde passaremos atributos da mesma, por exemplo, se receberemos um atributo chamado "Titulo", vamos defini-lo aqui.
- _Template_: Aqui definiremos o html que será substituido na diretiva.

```js
(function () {
  angular.module('allDirectives', [])
    .directive('painelFotos', function () {
    
      return {
        restrict: "AE",
        scope: {
          titulo: '@'
        },
        template:
        '<div class="panel panel-default">'+
        '        <div class="panel-heading">'+
        '           <h3 class="panel-title text-center">{{titulo}}</h3>'+
        '       </div>'+
        '       <div class="panel-body">'+
        '       </div>'+
        '</div>'
      };

    })
})()
```

> Note que a propriedade titulo está com um `@`, isso significa que o valor passado será literal, ou seja, uma string.

> Para passar outros valores podemos utilizar, por exemplo, `&` que significa que vamos passar uma __expressão__ que será executada no escopo do controller

> Para passar o valor _as-is_, ou seja, com o mesmo tipo de dados que está na diretiva, utilizamos o `=`

> É importante notar que a diretiva precisa ser em camelCasing, mas quando virar uma tag HTML, a mesma se transforma em hífen. Logo, `painelFotos` viraria `painel-fotos`. Sendo chamado da seguinte maneira:

```html
<painel-fotos ng-repeat="foto in fotos" titulo="{{foto.titulo}}">
    <img class="img-responsive center-block" src="{{foto.url}}" alt="{{foto.titulo}}">
</painel-fotos>
```

Neste caso, os elementos filhos não serão mantidos, ou seja, a imagem não vai ser exibida. Para isso vamos utilizar o _transclude_. O transclude vai manter os elementos marcados com a diretiva `ng-transclude`, vamos alterar a nossa diretiva:

```js
(function () {
  angular.module('allDirectives', [])
    .directive('painelFotos', function () {
    
      return {
        restrict: "AE",
        transclude: true, //Veja o Transclude
        scope: {
          titulo: '@'
        },
        template:
        '<div class="panel panel-default">'+
        '        <div class="panel-heading">'+
        '           <h3 class="panel-title text-center">{{titulo}}</h3>'+
        '       </div>'+
        '       <div class="panel-body" ng-transclude>'+ //Criamos a marcação que será mantida
        '       </div>'+
        '</div>'
      };

    })
})()
```

### Manipulação de DOM

No mundo Angular, manipular o DOM utilizando uma biblioteca externa (como o jQuery) é uma grande heresia, porque quebramos as regras de que o controller deve ser completamente _agnóstico_ a todo o tipo de DOM da ferramenta. Para isto usamos as diretivas do angular.

Vamos imaginar a seguinte situação: Temos um formulário que, ao ser submetido, deve passar o foco para um botão específico.

Podemos criar uma diretiva chamada `focus` para tratar essa situação:

```js
angular.module('minhasDiretivas')
  .directive('focus', () => {
    let ddo = {};

    ddo.restrict = "A";
    ddo.scope = {
      focado: '='
    };

    return ddo;
  });
```

Então no nosso botão podemos utilizar assim:

```html
<button focus focado='focado'>Texto</button>
```

Veja que o atributo `focado` é um elemento de `$scope.focado` no controller, ou seja, a propriedade se transforma em um valor interno de uma propriedade do controller, para que possamos verificar e alterar esses valores.

Porém ainda não temos acesso ao DOM e nem ao elemento do DOM que ele foi adicionado. Após a criação do DOM pelo angular, o mesmo retorna um _link_, o link é uma função que informa o escopo e o elemento de uma diretiva, e podemos utilizar ela na definição:

```js
angular.module('minhasDiretivas')
  .directive('focus', () => {
    let ddo = {};

    ddo.restrict = "A";
    ddo.scope = {
      focado: '='
    };
    ddo.link = (scope, element) => {

    };

    return ddo;
  });
```

Para podermos ter acesso à propriedade em tempo real e executar o two-way-databinding, utilizamos os _watchers_, que são responsáveis por adicionar listeners no elemento para um determinado atributo da diretiva:

```js
angular.module('minhasDiretivas')
  .directive('focus', () => {
    let ddo = {};

    ddo.restrict = "A";
    ddo.scope = {
      focado: '='
    };
    ddo.link = (scope, element) => {
      scope.$watch('focado', () => {
        alert('mudou');
      });
    };

    return ddo;
  });
```

O que estamos realizando aqui é que __sempre__ que o valor de `focado` for alterado, será exibido um alert. Vamos agora executar agora o código de manipulação de DOM utilizando a biblioteca jQLite (que é padrão do angular).

```js
angular.module('minhasDiretivas')
  .directive('focus', () => {
    let ddo = {};

    ddo.restrict = "A";
    ddo.scope = {
      focado: '='
    };
    ddo.link = (scope, element) => {
      scope.$watch('focado', () => {
        if(scope.focado) {
          element[0].focus(); //Element é um objeto jQLite, portanto ele é um array com o elemento do DOM como temos apenas um elemento, o array só tem um valor e então podemos acessar a função focus() do vanilla mesmo para focar
          scope.focado = false;
        }
      });
    };

    return ddo;
  });
```

> Note que, o watch é um pouco caro para a SPA, porque o poder computacional está sendo gasto com o monitoramento do atributo.

#### $broadcast

O Angular também trabalha com o EventBus, que é similar ao EventEmitter do NodeJS. Este é o outro modo de criarmos watchers para eventos específicos.

```js
cadastroDeFotos.cadastrar(foto)
  .then((retorno) => {
    $scope.mensagem = retorno.mensagem;
    if (retorno.inclusao) $scope.foto = {};
    $scope.$broadcast('fotoCadastrada'); //Emitimos um evento
  })
  .catch((erro) => {
    $scope.mensagem = erro;
  });
```

E na nossa diretiva podemos utilizar o listener `on`, presente no angular:

```js
angular.module('minhasDiretivas')
  .directive('focus', () => {
    let ddo = {};

    ddo.restrict = "A";
    
    ddo.link = (scope, element) => {
      scope.$on('fotoCadastrada', () => { //Criamos um listener para o foto cadastrada
        element[0].focus();
      });
    };
    return ddo;
  });
```

> Note que também removemos o atributo `focado` porque ele já não é mais necessário.

### RootScope

Podemos remover o evento fotoCadastrada do controller e levar diretamente ao serviço para que ele seja disparado sempre que o serviço executou a gravação ou a atualização. Mas não podemos realizar essa ação porque o `$scope` não é acessível dentro de um serviço.

O RootScope é o escopo principal que todos os escopos herdam, então podemos emitir um evento global. Precisamos apenas injetar o `rootScope`, imaginamos o serviço:

```js
angular.module('meuModulo', ['ngResource'])
  .factory('serviço', (recurso, $q, $rootScope) => {
    $rootScope.$broadcast('fotoCadastrada');
  });
```

### Templates externos

Podemos remover aquela marcação de concatenação de strings na diretiva, vamos criar um novo arquivo com o template:

```html
<div class="panel panel-default">
  <div class="panel-heading">
    <h3 class="panel-title text-center">{{titulo}}</h3>
  </div>
  <div class="panel-body" ng-transclude>
  </div>
</div>
```

Agora vamos trocar a propriedade `template` por `templateUrl`:

```js
(function () {
  angular.module('allDirectives', [])
    .directive('painelFotos', function () {
    
      return {
        restrict: "AE",
        transclude: true,
        scope: {
          titulo: '@'
        },
        templateUrl: "../partials/painel-fotos.html"
      };

    })
})()
```

### Diretivas que buscam dados

Vamos criar uma diretiva chamada meusTitulos. Essa diretiva buscará fotos do servidor e montará uma lista com apenas os títulos dessas fotos. Vamos alterar public/js/directives/minhas-diretivas.js:

```js
angular.module('minhasDiretivas', [])
    // diretivas anteriores omitidas
    .directive('meusTitulos', function() {
        var ddo = {};
        ddo.restrict = 'E';
        ddo.template = '<ul><li ng-repeat="titulo in titulos">{{titulo}}</li></ul>';
        return ddo;
    });
```

Até aqui, nenhuma novidade. Precisamos agora elaborar o código que busca as fotos do servidor. Para isso, precisaremos de recursoFoto, mas como? Sabemos que ele é um artefato injetável em controllers em serviços, mas em diretivas? A solução mora na propriedade controller do nosso ddo:

```js
angular.module('minhasDiretivas', [])
    // diretivas anteriores comentadas
    .directive('meusTitulos', function() {
        var ddo = {};
        ddo.restrict = 'E';
        ddo.template = '<ul><li ng-repeat="titulo in titulos">{{titulo}}</li></ul>';
        ddo.controller = function($scope, recursoFoto) {
        };
        return ddo;
    });
```

A propriedade controller permite passarmos uma função que permite termos acesso aos injetáveis do Angular, como `$scope` e `recursoFoto`. Há outros elementos exclusivos que não abordaremos aqui. Você deve estar se perguntando: ok, você me convenceu, mas como recursoFoto foi injetado se não temos o módulo meusServicos como dependência de minhasDiretivas? Resposta: nosso módulo principal da aplicação já carrega o módulo meusServicos, inclusive o módulo minhasDiretivas, por isso recursoFoto é injetável. Porém, fica mais bonito declarar explicitamente essa dependência em nosso módulo, sem efeito colateral algum.

Agora, basta buscarmos nossas fotos e adicionarmos o resultado em $scope.titulos. Veja que acessamos esta propriedade através da diretiva ng-repeat do nosso template:

```js
// explicitei a dependência do módulo `meusServicos`
angular.module('minhasDiretivas', ['meusServicos'])
    // diretivas anteriores comentadas
    .directive('meusTitulos', function() {
        var ddo = {};
        ddo.restrict = 'E';
        ddo.template = '<ul><li ng-repeat="titulo in titulos">{{titulo}}</li></ul>';
        ddo.controller = function($scope, recursoFoto) {
            recursoFoto.query(function(fotos) {
                $scope.titulos = fotos; // ainda não é isso que queremos!
            });
        };
        return ddo;
    });
```

Espere um pouco, `$scope.titulos` está recebendo a lista de fotos, não queremos isso! Queremos é uma lista de títulos. Que tal um pouquinho de JavaScript do "bem" para nos ajudar na tarefa de criar uma nova lista a partir de outra? Vamos usar a função .map:

```js
angular.module('minhasDiretivas', ['meusServicos'])
    // diretivas anteriores comentadas
    .directive('meusTitulos', function() {
        var ddo = {};
        ddo.restrict = 'E';
        ddo.template = '<ul><li ng-repeat="titulo in titulos">{{titulo}}</li></ul>';
        ddo.controller = function($scope, recursoFoto) {
            recursoFoto.query(function(fotos) {
                $scope.titulos = fotos.map(function(foto) {
                    return foto.titulo;
                });    
            });
        };
        return ddo;
    });
```

A função map itera sobre nossa lista fornecendo acesso ao elemento da iteração no seu parâmetro. Poderia ser qualquer nome, mas nada mais justo chamarmos de foto, já que estamos iterando sobre uma lista de fotos. Para cada foto retornamos seu titulo, isto é, no final da iteração teremos uma nova lista, mas de títulos apenas.

Muito bem, agora é só utilizarmos nossa diretiva. Para não bagunçar nosso projeto, vamos adicioná-la como último elemento da parcial 'principal.html', assim:

```html
<meus-titulos></meus-titulos>
```

E nela colocamos a url do template, sempre partindo da pasta raiz.

## Filtrando dados

Podemos fazer com que os usuários possam filtrar os dados de suas pesquisas, para isto vamos utilizar uma nova marcação para criar um formulário de envio:

```html
<div class="row">
  <div class="col-md-12">
    <form action="">
      <input type="text" class="form-control" placeholder="filtrar">
    </form>
  </div>
</div>
```

Assim temos uma nova linha com um formulário de filtro.

### Utilizando Models

Para filtrar estes dados, temos que ter a capacidade de utilizar uma string (capturada em tempo real) em uma variável JavaScript. Não podemos utilizar uma angular expression, porque as mesmas são apenas leitura, vamos utilizar então a diretiva `ng-model`.

Primeiramente, criamos uma variável dentro do nosso controller chamada filtro com `$scope.filtro`, e utilizaremos a diretiva no nosso input:

```html
<div class="row">
  <div class="col-md-12">
    <form action="">
      <input type="text" class="form-control" placeholder="filtrar" ng-model="filtro">
    </form>
  </div>
</div>
```

Podemos filtrar uma lista dentro do `ng-repeat`, usamos especificamente uma diretiva do angular que podemos passar um valor e este valor ser reduzido em um array:

```html
<painel-fotos ng-repeat="foto in fotos | filter: filtro" titulo="{{foto.titulo}}">
    <img class="img-responsive center-block" src="{{foto.url}}" alt="{{foto.titulo}}">
</painel-fotos>
```

Utilizando o `| filter: <string>` estamos dizendo que o array deve possuir **em qualquer propriedade** a string passada. Desta forma podemos filtrar os arrays de acordo com um filtro built-in do angular.

Para podermos filtrar uma propriedade especifica, podemos passar um objeto `{propriedade: valor}`

Um dos problemas é que a atualização do `$scope` é instantanea, para podermos criar um _delay_ entre  a digitação e a atualização do dado, podemos utilizar uma diretiva chamada `ng-model-options` com o valor `debounce`

```html
<div class="row">
  <div class="col-md-12">
    <form action="">
      <input type="text" class="form-control" placeholder="filtrar" ng-model="filtro" ng-model-options="{debounce: 500}">
    </form>
  </div>
</div>
```

#### Pseudo-classes

Quando utilizado em conjunto com o ngAnimate, o angular coloca algumas classes a medida que algumas ações são tomadas. Assim como o browser coloca classes como `:before`, `:first-line` e etc, também o angular tem a capacidade de incluir algumas classes no seu DOM.

- `ng-leave`: É uma classe colocada pelo angular no `ng-repeat` quando um elemento deixa a lista
- `ng-leave-active`: É uma classe do angular para o `ng-repeat` quando um elemento está para deixar a lista

Para mais classes, veja na página de ajuda do ngAnimate.

## Views, rotas e partials

O modelo SPA não necessariamente significa que toda a aplicação vai ter apenas uma página, mas sim que ela vai ser carregada uma unica vez e nunca mais após disso.

Para podermos criar outras views e ouras telas para nossas aplicações, podemos utilizar das views do próprio angular.

Para definir um local de visualização, utilizamos a tag `<ng-view>` para definir que aquele local será o local aonde os conteúdos serão colocados.

```html
<html>
  <head>
  ...
  </head>
  <body>
    <ng-view></ng-view>
  </body>
</html>
```

Para utilizarmos o modelo de rotas que iremos implementar, vamos utilizar o script `ngRoute` que o próprio angular provém. Primeiramente vamos importar e definir como uma dependencia no nosso `main.js`.

O Router vai, por meio de requisições AJAX, iniciar o download dos recursos e importar seu conteúdo dentro do `ng-view`.

> __Importante__: Temos que remover todas as referências aos controllers que utilizamos previamente, pois quando trocarmos de rota, esses controllers não estarão mais disponíveis, o próprio router do angular vai injetar essas dependencias no partial carregado.

```js
angular.module("alurapic", ["minhasDiretivas", "ngAnimate", "ngRoute"])
  .config(function($routeProvider) {
    $routeProvider.when("/fotos", {
      templateUrl: "partials/principal.html",
      controller: "FotosController"
    });
  });
```

No código acima estamos dizendo que quando o endereço for `/fotos`, então vamos carregar o partial `principa.html` e aplicar seu controller.

> Note que a URL que o Router enxerga deve seguir o padrão `/#/<caminho>`, ou seja, neste caso acima seria `/#/fotos`

Da mesma forma vamos cadastrar o nosso partial de fotos:

```js
angular.module("alurapic", ["minhasDiretivas", "ngAnimate", "ngRoute"])
  .config(function($routeProvider) {

    $routeProvider.when("/fotos", {
      templateUrl: "partials/principal.html",
      controller: "FotosController"
    });

    $routeProvider.when("/fotos/new", {
      templateUrl: "partials/cadastro.html",
      controller: "CadastroController"
    });
  });
```

Quando não temos nenhuma rota que bata com as rotas existentes, podemos definir uma rota padrão:

```js
angular.module("alurapic", ["minhasDiretivas", "ngAnimate", "ngRoute"])
  .config(function($routeProvider) {

    $routeProvider.when("/fotos", {
      templateUrl: "partials/principal.html",
      controller: "FotosController"
    });

    $routeProvider.when("/fotos/new", {
      templateUrl: "partials/cadastro.html",
      controller: "CadastroController"
    });

    $routeProvider.otherwise({ redirectTo: "/fotos" });
  });
```

Desta forma o método `otherwise` diz que, se não pudermos encontrar a rota selecionada, vamos direcionar para uma rota padrão, neste caso `/#/fotos`.

### LocationProvider e modo HTML5

O angular possui uma capacidade de trabalhar com rotas sem a `#`, mas para isso tanto o front quando o back-end precisam estar preparados para aceitar este tipo de rotas.

uma vez que ambos os lados estejam preparados para isto, podemos simplesmente injetar uma nova dependencia e configura-la de forma adequada.

```js
angular.module("alurapic", ["minhasDiretivas", "ngAnimate", "ngRoute"])
  .config(function($routeProvider, $locationProvider) {

    $locationProvider.html5Mode(true);

    $routeProvider.when("/fotos", {
      templateUrl: "partials/principal.html",
      controller: "FotosController"
    });

    $routeProvider.when("/fotos/new", {
      templateUrl: "partials/cadastro.html",
      controller: "CadastroController"
    });

    $routeProvider.otherwise({ redirectTo: "/fotos" });
  });
```

E no nosso HTML precisamos utilizar a tag `<base href="/">` no topo do head para que isso funcione.

> Não se esqueça de que é importante que o back-end precisa estar configurado para isso.

### Wildcards

É possível definir wildcards ou parâmetros de rota para as rotas do angular da mesma forma que definimos em qualquer outro framework:

```js
angular.module("alurapic", ["minhasDiretivas", "ngAnimate", "ngRoute"])
  .config(function($routeProvider, $locationProvider) {

    $locationProvider.html5Mode(true);

    $routeProvider.when("/fotos", {
      templateUrl: "partials/principal.html",
      controller: "FotosController"
    });

    $routeProvider.when("/fotos/new", {
      templateUrl: "partials/cadastro.html",
      controller: "CadastroController"
    });

    $routeProvider.when("/fotos/edit/:id", {
      templateUrl: "partials/cadastro.html",
      controller: "CadastroController"
    });

    $routeProvider.otherwise({ redirectTo: "/fotos" });
  });
```

Tudo que segue após o `:<nome>` se transforma em um atributo do objeto `$routeParams`. Desta forma podemos acessar o id pelo controller através do seguinte código:

```js
angular.module('alurapic').controller('CadastroController', function ($scope, $http, $routeParams) {
  console.log($routeParams.id);
});
```

## Formulários com AngularJS

Tendo um formulário, podemos usar as diretivas `ng-model` para poder atribuir uma variável no angular para cada input do formulário.

Assim como também podemos usar diretivas de eventos do angular para ligar o evento submit do form para um método do angular.

```html 
<form action="" ng-submit="submeter()"></form>
```

E então no modelo do Angular fazemos:

```js
$scope.submeter = function() {

}
```

E colocamos nosso código internamente.

### Validação de Formulários

Primeiramente temos de desativar todas as validações do HTML5 usando o atributo `novalidate` dentro da tag form:

```html
<form novalidate name="formulario" class="row" ng-submit="submeter()">
    <div class="col-md-6">
        <div class="form-group">
            <label>Título</label>
            <input name="titulo" class="form-control" ng-model="foto.titulo">    
        </div>
        <div class="form-group">
            <label>URL</label>
            <input name="url" class="form-control" ng-model="foto.url">
        </div>
        <div class="form-group">
            <label>Descrição</label>
            <textarea name="descricao" class="form-control" ng-model="foto.descricao"></textarea>
        </div>

        <button type="submit" class="btn btn-primary">
            Salvar
        </button>
         <a href="/" class="btn btn-primary">Voltar</a>
    </div>
    <div class="col-md-6">
        <minha-foto></minha-foto>
    </div>
</form>
```

Então vamos deixar os campos obrigatórios e definir mensagens de erro:

```html
<form novalidate name="formulario" class="row" ng-submit="submeter()">
    <div class="col-md-6">
        <div class="form-group">
            <label>Título</label>
            <input name="titulo" class="form-control" ng-model="foto.titulo" required>
            <span class="form-control alert-danger">Título obrigatório</span>
        </div>
        <div class="form-group">
            <label>URL</label>
            <input name="url" class="form-control" ng-model="foto.url" required>
            <span class="form-control alert-danger">URL obrigatória</span>
        </div>
        <div class="form-group">
            <label>Descrição</label>
            <textarea name="descricao" class="form-control" ng-model="foto.descricao"></textarea>
        </div>

        <button type="submit" class="btn btn-primary">
            Salvar
        </button>
         <a href="/" class="btn btn-primary">Voltar</a>
    </div>
    <div class="col-md-6">
        <minha-foto></minha-foto>
    </div>
</form>
```

Veja que utilizamos o atributo `required` do próprio HTML5, porém desta vez será tratada pelo angular.

Para as mensagens de erro, criamos um span logo abaixo do campo, porém no momento ele fica sempre exibido. Vamos condicionar ele a exibir apenas quando o formulário estiver inválido. Para isto, primeiramente vamos utilizar a diretiva `ng-show`:

```html
<form novalidate name="formulario" class="row" ng-submit="submeter()">
    <div class="col-md-6">
        <div class="form-group">
            <label>Título</label>
            <input name="titulo" class="form-control" ng-model="foto.titulo" required>
            <span class="form-control alert-danger" ng-show="formulario.$submitted && formulario.titulo.$error.required">Título obrigatório</span>
        </div>
        <div class="form-group">
            <label>URL</label>
            <input name="url" class="form-control" ng-model="foto.url" required>
            <span class="form-control alert-danger" ng-show="formulario.$submitted && formulario.titulo.$error.required">URL obrigatória</span>
        </div>
        <div class="form-group">
            <label>Descrição</label>
            <textarea name="descricao" class="form-control" ng-model="foto.descricao"></textarea>
        </div>

        <button type="submit" class="btn btn-primary">
            Salvar
        </button>
         <a href="/" class="btn btn-primary">Voltar</a>
    </div>
    <div class="col-md-6">
        <minha-foto></minha-foto>
    </div>
</form>
```

Estamos utilizando uma validação interna do angular que identifica qual é o tipo de erro que obtivemos de acordo com o campo que precisamos. Também pedimos para que esta validação seja feita apenas quando o formulário for submetido.

Podemos mudar o erro para limitar o tamanho de caracteres por exemplo.

```html
<form novalidate name="formulario" class="row" ng-submit="submeter()">
    <div class="col-md-6">
        <div class="form-group">
            <label>Título</label>
            <input name="titulo" class="form-control" ng-model="foto.titulo" required ng-maxlength="20">
            <span class="form-control alert-danger" ng-show="formulario.$submitted && formulario.titulo.$error.required">Título obrigatório</span>
            <span class="form-control alert-danger" ng-show="formulario.$submitted && formulario.titulo.$error.maxlength">O título só pode ter 20 letras</span>
        </div>
        <div class="form-group">
            <label>URL</label>
            <input name="url" class="form-control" ng-model="foto.url" required>
            <span class="form-control alert-danger" ng-show="formulario.$submitted && formulario.titulo.$error.required">URL obrigatória</span>
        </div>
        <div class="form-group">
            <label>Descrição</label>
            <textarea name="descricao" class="form-control" ng-model="foto.descricao"></textarea>
        </div>

        <button type="submit" class="btn btn-primary">
            Salvar
        </button>
         <a href="/" class="btn btn-primary">Voltar</a>
    </div>
    <div class="col-md-6">
        <minha-foto></minha-foto>
    </div>
</form>
```

#### ng-options

`ng-options` é uma diretiva do angular que identifica uma opção para uma outra diretiva. Como no caso da exibição de uma tag `select` que identifica o value de forma diferente do nome. Podemos usar o `ng-options` como: `grupo._id as grupo.nome for grupo in grupos`. Desta forma o nome do select vai ser exibido e o ID será o valor.

```html
<!-- código anterior omitido -->

        <div class="form-group">
            <label>Grupo</label>
            <select name="grupo" 
                    ng-model="foto.grupo" class="form-control" required
                    ng-controller="GruposController"
                    ng-options="grupo._id as grupo.nome for grupo in grupos">
                <option value="">Escolha um grupo</option>
            </select>
            <span ng-show="formulario.$submitted && formulario.grupo.$error.required" class="form-control alert-danger">
                Grupo obrigatório
            </span>
        </div>

<!-- código posterior omitido -->
```

a diretiva ng-options, que possui comportamento parecido com `ng-repeat`, porém a sintaxe `"grupo._id as grupo.nome"` indica que o valor do elemento será o ID do grupo e o que será exibido para seleção será seu nome. O restante `"for grupo in grupos"` percorrerá a lista de grupos disponibilizada no escopo do controller, construindo cada item de nossa lista.
 
#### Outras diretivas

Podemos usar a diretiva `ng-disabled` para definir quando um campo ou elemento estará desativado:

```html
<button type="submit" class="btn btn-primary" ng-disabled="formulario.$invalid">Salvar</button>
```

Desta forma o botão salvar será apenas habilitado quando o formulário estiver válido.

## ngResource

> Se o back-end mudar, o que acontece?

Temos que realizar todas as mudanças em todas as URL's que chamamos em nossos endpoints dentro do nosso código.

Quando importamos o script `ngResource` dentro da nossa chamada de scripts e no módulo, temos acesso a uma classe chamada `$resource`.  Podemos então criar recursos com nossos endpoints.

```js
let recursoFoto = $resource('v1/fotos/:fotoId');
```

### GET

E então podemos substituir nosso `$http.get` por:

```js
recursoFoto.query(
  (fotos) => {
  $scope.fotos = fotos;
},
  (erro) => {
    console.log(erro);
  });
```

### DELETE

Isso resolve o nosso problema com o `$http.get`. Podemos agora alterar o nosso botão de remoção utilizando o verbo `delete` do HTTP através do resource:

```js
resucroFoto.delete({fotoId: foto._id}, 
  () => {
    //sucesso
  }, 
  (erro) => { 
    //erro
});
```

> Note que no `query`, não precisamos nos preocupar com o parâmetro `fotoId`, mas no `delete` temos o primeiro parâmetro que é justamente o parâmetro.

### PUT

Para usarmos o PUT, podemos simplesmente alterar a definição do nosso recurso. Passaremos um segundo parâmetro como `null`, que será a _query string_ da URL, enquanto o terceiro parâmetro será um objeto nomeado com a nossa função:

```js
let recursoFoto = $resource('v1/fotos/:fotoId', null, {
  update: {
    method: "PUT"
  }
});
```

O que estamos fazendo aqui é criar um método update, que vai fazer uma requisição para essa URL com o método PUT:

```js
recursoFoto.update({fotoId: $scope.foto._id}, $scope.foto, 
() => {
    //sucesso
},
(erro) => {
    //erro
});
```

> O primeiro parâmetro é o parâmetro da URL, enquanto o segundo é a body da request

## Serviços

Serviços são códigos que podem ser reutilizados ao longo de todo o app.

Vamos criar um novo arquivo (podemos criar esse serviço dentro de um módulo separado)

Existem vários modos de serviços, um deles é o `factory` que sempre retorna um objeto ou função:

```js
angular.module('meusServicos', ['ngResource'])
  .factory('recursoFoto', ($resource) => {

    return $resource('v1/fotos/:fotoId', null, {
                        update: {
                          method: "PUT"
                        }
                      });
  });
```

Estamos retornando um objeto já configurado, o primeiro parâmetro é o nome do serviço. Agora podemos injetar este módulo como injetamos qualquer outro:

```js
angular.module('alurapic').controller('fotosController', ($scope, recursoFoto) ...)
```

E poderemos usar como uma variável normal

### Utilizando promisses com serviços

Para utilizarmos promisses dentro dos nossos serviços do Angular, utilizamos um serviço injetável chamado `$queue` (ou `$q`). Este serviço leva dois parâmetros, um deles é o `resolve` e o outro é o `reject`:

```js
angular.module('meusServicos', ['ngResource'])
  .factory('cadastroDeFotos', (recursoFoto, $q) => {
    let servico = {};

    servico.cadastrar = (foto) => {
      return $q((resolve, reject) => {
        //Implementação
      });
    };

    return servico;
  });
```

O `resolve` é uma função que devolve valores, assim como o `reject`, desta forma, dentro do `then` e do `catch` respectivamente.

Podemos implementar da seguinte maneira:

```js
angular.module('meusServicos', ['ngResource'])
  .factory('cadastroDeFotos', (recursoFoto, $q) => {
    let servico = {};

    servico.cadastrar = (foto) => {
      return $q((resolve, reject) => {
        if(foto._id) {
          recursoFoto.update({fotoId: foto._id}, () => {
            resolve({
              mensagem: "Foto " + foto.titulo + " atualizada com sucesso",
              inclusao: false
            });
          },
          (erro) => {
            console.log(erro);
            reject("Não foi possível alterar a foto " + foto.titulo);
          });
        } else {
          recursoFoto.save(foto, () => {
            resolve({
              mensagem: 'Foto ' + foto.titulo + ' incluida com sucesso',
              inclusao: true
            });
          },
          (erro) => {
            console.log(erro);
            reject('Não foi possível incluir a foto ' + foto.titulo);
          });
        }
      });
    };

    return servico;
  });
```

Assim todos os dados seriam utilizados da seguinte forma:

```js
cadastroDeFotos.cadastrar(foto)
  .then((retorno) => {
    $scope.mensagem = retorno.mensagem;
    if retorno.inclusao
  })
  .catch((erro) => {
    $scope.mensagem = erro;
  });
```

## Blindagem de minificação

É extremamente comum a minificação de scripts para reduzir o tamanho dos arquivos e por conseguinte diminuir o uso de banda por parte do cliente, ainda mais se ele estiver em uma rede móvel como a 3G.

O problema é que o processo de minificação altera o nome dos parâmetros das funções (processo chamado de _mingle_). Não há problema algum nisso, contanto que o novo nome seja trocado em todos os lugares em que é usado, porém o sistema de injeção de dependências do Angular é baseado no nome dos parâmetros. A conclusão disso é que nada mais funcionará no Angular após a minificação, já que os parâmetros das funções serão trocados por outros nomes aleatórios e menores que não tem nada a ver.

Para solucionar este problema, o Angular possui o annotation system, um sistema de anotação que permite dizer o que deve ser injetado para cada parâmetro do controller, mesmo que seu nome seja trocado. Veja a solução:

Este controller :

```js
angular.module('alurapic')
    .controller('FotoController', function($scope, recursoFoto, $routeParams, cadastroDeFotos) {
            // código omitido
    });
```

Vira:

```js
angular.module('alurapic')
    .controller('FotoController', ['$scope', 'recursoFoto', '$routeParams', 'cadastroDeFotos', function($scope, recursoFoto, $routeParams, cadastroDeFotos) {
            // código omitido
    }]);
```

Veja que o segundo parâmetro do controller é um array que recebe primeiro todos os artefatos que o controller do Angular receberá e por último a função que define o controller. O processo de minificação jamais tocará nos dados do array e o Angular segue a convenção que o primeiro parâmetro do array será injetado como primeiro parâmetro da função do controller. Se o nome do parâmetro da função do controller muda, tudo continuará funcionando.