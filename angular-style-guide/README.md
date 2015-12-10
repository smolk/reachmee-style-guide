# AngularJS Style Guide

## Table of contents
1. [Basic rules](#basic-rules)
2. [File structure](#file-structure)
3. Routing
4. Controllers
 * ControllerAs notation
 * 
5. Services
6. Directives
7. 3d-part components


# Basic rules
### Single responsibility
Define 1 component per file.

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

Avoid one-line dependency injection
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

