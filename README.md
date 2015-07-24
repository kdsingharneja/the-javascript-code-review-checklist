# The Javascript Code Review Checklist

The ultimate code review guide and checklist on JavaScript. I hope so!.

These are compiled based on my experiences, observations, best practices I have been a part of. Besides serving these as a self guide, I am offering this as a suggestive checklist on code reviewing JavaScript. Hope this helps you.

This will be a work in progress since code review is an art and you just cannot hava a checklist that is long enough.

## Plain o' JavaScript

## Angular 1.x

### Controller As syntax and Google Closure Compiler

Instead of using $scope every where in your controller,
- setup google closure compiler for your builds.
- write controller, services and directives as classes by using Google Closure Compiler
- Depend on JSDoc annoations which GCC uses well to ensure type safety.
- Use <Controller_Class> as <ControllerName> syntax in the code and templates.

This reduces stupid development time regressions and makes JS more strongly types language.

The pain is shortlived but helps in the future.

REadings...
http://google.github.io/styleguide/angularjs-google-style.html
https://github.com/google/closure-compiler/wiki/Using-JSDoc-Annotations-with-Closure-compiler

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


## Backbone
