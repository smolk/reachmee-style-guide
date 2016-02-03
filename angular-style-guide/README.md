# AngularJS Style Guide

## Table of contents
1. [Basic rules](#basic-rules)
2. [File structure](#file-structure)
3. Routing
4. [Controllers](#controllers)
5. Services
6. Directives
7. [Logging](#logging)
8. 3d-part components


# Basic rules
### Single responsibility
**Define 1 component per file.**

The following example defines the app module, defines a controller, and defines a directive all in the same file.
```javascript
// avoid
angular.module('rm')
 .controller('FirstPageCtrl', [function(){...}])
 .directive('rmSomeList', [function(){...}]);
```
Same components now located in separate files

```javascript
// recommended
// first-page.controller.js
angular.module('rm')
 .controller('FirstPageCtrl', [function(){...}]);


// some-list.directive.js
angular.module('rm')
 .directive('rmSomeList', [function(){...}]);
```

### Avoid one-line dependency injection
```javascript
// avoid
angular.module('rm')
 .controller('FirstPageCtrl', ['$http', '$state', '$modal', '$log', function(){...}]);
```

```javascript
// recommended
angular.module('rm')
 .controller('FirstPageCtrl', [
 '$http', 
 '$state', 
 '$modal', 
 '$log', 
function(){...}]);
```


[back to top](#angularjs-style-guide)

# File structure
 * Create folder named for the page or sub-page they represented. 
 * Place controller, template and all the components like services, directives, filters related to this page in the Folder. 
### Folder-by-Page structure
```javascript
// recommended
/site
    /app-admin
    /app-recruitment
        app.module.js
        app.routing.js
        /commission-list
        /candidate-list
        /commission
            /commission.controller.js
            /commission.service.js
            /commission-sidebar.directive.js
            /commission-sidebar.html
            /commission.html
            /advertise
            /candidate-list
                /candidate-list.controller.js
                /candidate-list.html
                /candidate.service.js
                /extended-view.directive.js
                /extended-view.directive.html
                /table-view.directive.js
                /table-view.directive.html
                /add-candidate.modal.js
                /add-candidate.modal.html
                /advanced-filter.modal.js
                /advanced-filter.modal.html
            /desciption
            /participants
            ...
    /app-reports
    /app-extview
    /app-login
    /directives
    /services
    /vendor
```

[back to top](#angularjs-style-guide)

# Controllers
 * **ControllerAs notation.** Use `ControllerAs` syntax
```javascript
$stateProvider.state('commission.candidate-list', {
  templateUrl: 'app-recruitment/commission/candidate-list/candidate-list.html',
  controller: 'CandidateListController',
  controllerAs: 'ctrl' // ctrl_[name] - for controllers that will have subviews to make it accessible
})
```

* In the DOM we get a variable per controller, which aids nested controller methods, avoiding any $parent calls
* Use `$scope` only for creating watchers `$scope.$watch`otherwise use `this` captured by `self`.
* `this` gets bound to `$scope` always
```javascript
// avoid
angular.module('rm')
 .controller('CandidateListController', [
 '$scope',
 '$log',
 controller
]);

function controller($scope, $log){
 $scope.bntClick = function(){};
}
```

```javascript
// recommended
angular.module('rm')
 .controller('CandidateListController', [
 '$log',
 controller
]);

function controller($log){
 var self = this;
 self.bntClick = function(){};
};
```

* **Methods.** Pass functions into module methods rather than assign as a callback
```javascript
// avoid
angular.module('rm')
 .controller('CandidateListController', [
 '$scope',
 '$log',
function($scope, $log){...}]);
```

```javascript
// recommended
angular.module('rm')
 .controller('CandidateListController', [
 '$log',
 controller
]);
function controller($log){...};
```

* **Size** It is time to refactor your controller if it's bigger than `300+` lines. Business logic should be wrapped in Service or Directive.

* **Bindable Members Up Top**
* Place bindable members at the top of the controller, alphabetized, and not spread through the controller code.
  * Placing bindable members at the top makes it easy to read and helps you instantly identify which members of the controller can be bound and used in the View.
  * Setting anonymous functions in-line can be easy, but when those functions are more than 1 line of code they can reduce the readability. Defining the functions below the bindable members (the functions will be hoisted) moves the implementation details down, keeps the bindable members up top, and makes it easier to read.

```javascript
// avoid
angular.module('rm')
 .controller('CandidateListController', [
 '$log',
 controller
]);
function controller($log){
 var self = this;
 
 this.moveCandidate = function(){};
 this.candidates = [];
 this.showFilterSection = function(){};
 
};
```

```javascript
// recommended
angular.module('rm')
 .controller('CandidateListController', [
 '$log',
 controller
]);
function controller($log){
 var self = this;
 
 self.moveCandidate = moveCandidate;
 self.candidates = [];
 self.showFilterSection = showFilterSection;
 
 function moveCandidate(){...}
 function showFilterSection(){...}
};
```
[back to top](#angularjs-style-guide)

# Directives
 * **ControllerAs notation.** Use `ControllerAs` syntax
```javascript
angular.module('rm')
    .directive('rmProcessFoldersHorizontal', function(){
        return directive = {
            restrict: 'A',
            templateUrl: 'template.directive.html',
            controllerAs: 'vm',
            controller: [
                '$log',
                controller
            ]
        };
    });
```

[back to top](#angularjs-style-guide)

# Credits
Working on this Style Guide I was inspired by following resources, trying to get parts that in my opinion are usefull in ReachMee project:
* [johnpapa](https://github.com/johnpapa/angular-styleguide#controllers)
* [toddmotto](https://github.com/toddmotto/angularjs-styleguide#controllers)
* [airbnb](https://github.com/airbnb/javascript#naming-conventions)
* [gocardless](https://github.com/gocardless/angularjs-style-guide#general-patterns-and-anti-patterns)
