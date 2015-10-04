# The Javascript Code Review Checklist

The ultimate code review guide and checklist on JavaScript. I hope so!.

These are compiled based on my experiences, observations, best practices I have been a part of. Besides serving these as a self guide, I am offering this as a suggestive checklist on code reviewing JavaScript. Hope this helps you.

This will be a work in progress since code review is an art and you just cannot hava a checklist that is long enough.

Table of Contents
- [Plain O' JavaScript](#plain-o'-javascript)
- [Angular 1x](#angular-1x)
- [CoffeeScript](#coffeescript)
- [JQuery](#jquery)
- [Backbone](#backbone)

## Plain o' JavaScript

* Always use `===` instead of `==`. 
Best to use [jshint](http://jshint.com/) or other tools to catch this automatically.

* Actively replace Prototype.js code with jQuery, where possible, while working on new features or refactoring.

* Even though variables are hoisted up in JS, make sure to declare vars on top of method. This promotes readability

Intead of..

      function() {
      	console.log(myVar);
      	var myVar = "Hi";
      }

Do...

      function() {
      	var myVar = "Hi";
      	console.log(myVar);
      }

* Make sure strings are localized if the framework is in place

* Make sure exceptions that are caught and show to the user are also localized and make sense.

* When checking on Object properties depend on dot notation. ie.
Instead of

```
MyController['myMethod'] !== undefined
```

do

```
MyController.myMethod !== undefined
```

* Prefer initial assignments to 'undefined' instead of 'null'. Then the following checks become useful

```
var myFlag = undefined;
...
if (myFlag) {
..
}
```

* Prefer (not not) operator instead of checking for undefined. Then you don't have to do direct comparision

Instead of

```
return myFlag !== undefined
```

do

```
return !!myFlag
```

* Prefer (not not) operator instead of checking for undefined. Then you don't have to do direct comparision

* To use JSDoc annotations to enfore type safety and method contract instead of doing a lot of check in the code.

i.e avoid...

```
if (check1 && check2 && check3) ...
```

* Avoid large anonymous functions. Instead create a private function and use that.

### JSDoc
* For private methods user @private annotations

* If you are using annotations, best to run it through Google Closure Compiler. That will make sure if you are any unsupported annotations but virtue of typos etc.

## Angular 1x

### General
* Instantiate $scope variables to null or empty array etc instead of leaving them undefined

* `$boardcast` only those events on $rootScope that are used systemwide or belong on a system event bus. 

### DOM in directives

Limit DOM manipulation to directives only. Rely on jqLite as much as possible or jQuery if need be.

### Promise chains

* Prefer promise syntax rather than callback syntax. i.e Instead of passing error handler as the second argument in the $q or a resource call, prefer chaining successes in `then`s followed by `catch` and the finally clause in the last for cleanup etc.

Make sure _catch_ clause is the the last one.

Instead of:
```
    MyResource.query({
        ....
      }).$promise.
        catch(function(error) {
          showError($i18next('Failed to get part.'), error);
        }).
        then(function(response) {
        ...
        }).
```

Do..

```
    MyResource.query({
        ....
      }).$promise.
        then(function(response) {
        ...
        }).
        catch(function(error) {
          showError($i18next('Failed to get part.'), error);
        }).
        finally(...) ;
```

Also notice the punctuations at the start and end of your then and catches. This helps readability.

* Depend on `finally` block as much as possble for cleanups.

* Return promises and not deferred

Instead of... 

```
  function resolveQuery(...) {
      var deferred = $q.defer();
      ...
      return deferred;
  }
```
Do..

```
  function resolveQuery(...) {
      var deferred = $q.defer();
      ...
      return {$promise: deferred.promise};
  }
```  

### $resource

Prefer `$resource` over `$http`

### $http

Limit use `$http` to writing unit tests or for places where you cannot use `$http`. Avoid direct use of `$http` in Angular controllers. Instead extract the requests to service objects instead. 

Either use

Controller -calling on-> providers using $resources
Or
Controllers -calling on-> Services -calling on-> providers using $resource or $http . This is Adapter pattern. 

### Controller As syntax and Google Closure Compiler

Instead of using $scope every where in your controller,
- setup google closure compiler for your builds.
- write controller, services and directives as classes by using Google Closure Compiler
- Depend on JSDoc annoations which GCC uses well to ensure type safety.
- Use <Controller_Class> as <ControllerName> syntax in the code and templates.

This reduces stupid development time regressions and makes JS more strongly types language.

The pain is shortlived but helps in the future.

Readings...

[Angular Google Style](http://google.github.io/styleguide/angularjs-google-style.html)

[JSDoc Annotations with Closure Compiler](https://github.com/google/closure-compiler/wiki/Using-JSDoc-Annotations-with-Closure-compiler)

_Angular 2.0 is based on this pattern, so the more you depend on this the better it is._

### Anonymous functions

Instead of this...
      function forgetDevice() {...}
      
      $scope.forgetDevice = function(device) {	
        return forgetDevice(device);
      };

Do
      function forgetDevice() {...}
      
      $scope.forgetDevice = forgetDevice;

i.e No need for these anonymous wrappers when you are putting private functions into 

## Digest cycles
Instead of this anti pattern.
```js
if(!$scope.$$phase) {
  //$digest or $apply
}
```

Do..
```js
$timeout(function() {
  // anything you want can go here and will safely be run on the next digest.
})
```

## CoffeeScript

* Avoid conditional modifiers (lines that end with conditionals).
* Break up long lines with trailing dot-notation

Intead of 

```js
$http.get("api/route", params).success(successHandler).error(errorHandler)
```

Do

```js
$http.get("/api/route", params).
  success(successHandler).
  error(errorHandler)
```

* Initialize arrays using `[]`.
* Initialize empty objects and hashes using `{}`.
* Use hyphen-separated filenames, such as `coffee-script.coffee`.
* Use `PascalCase` for classes, `lowerCamelCase` for variables and functions,
  `SCREAMING_SNAKE_CASE` for constants, `_single_leading_underscore` for
  private variables and functions.
* Prefer `==` and `!=` to `is` and `isnt`.
* Prefer `||` and `&&` to `or` and `and`.
* Prefer `!` over `not`.
* Prefer `@` over `this` for referencing instance properties.
* Prefer double quotes.

## JQuery

* Simplify selectors
Instead of ..
```
$("#topContainer).find(".childA .childB").doSomething();
```

Do...
```
$("#topContainer .childA .childB").doSomething();
```

## Backbone

Coming soon..
