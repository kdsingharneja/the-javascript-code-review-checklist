# The Javascript Code Review Checklist

The ultimate code review guide and checklist on JavaScript. I hope so!.

These are compiled based on my experiences, observations, best practices I have been a part of. Besides serving these as a self guide, I am offering this as a suggestive checklist on code reviewing JavaScript. Hope this helps you.

This will be a work in progress since code review is an art and you just cannot hava a checklist that is long enough.

Table of Contents
- [Plain O' JavaScript](#plain-o'-javascript)
- [Angular 1x](#angular-1x)
- [CoffeeScript](#coffeescript)
- [Backbone](#backbone)

## Plain o' JavaScript

* Actively replace Prototype.js code with jQuery, where possible, while working on new features or refactoring.

8 Variables are hoisted up in JS. But make sure to declare vars on top of method. This promotes readability

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

## Angular 1x

### DOM in directives

Limit DOM manipulation to directives only. Rely on jqLite as much as possible or jQuery if need be.

### $resource

Prefer `$resource` over `$http` when in Angular code.

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

## Backbone

Coming soon..
