# Modules, Controllers & Data Binding #

[TOC]

With AngularJS, we divide front-end source code into reusable components, called **modules**. A module is a *container* that groups together different components under a common name.
Modules can depend on other modules.

## Create Our First Module ##

`angular.module()` is as a global API for creating & retrieving modules and registering components.

    // define a module with no dependencies
    angular.module('firstModule', []);
    
    // define a module with 2 dependencies
    angular.module('firstModule', ['moduleA', 'moduleB']);
    
    // register a controller
    angular.module('firstModule').controller('FirstController', ['$scope', function ($scope) {
	    // your rocking controller
    }]);
    
    // register a directive
    angular.module('firstModule').directive('FirstDirective', function() {
	    return {
		    // directive goes here
	    }
    }]);

## Modular Programming Best Pratice

We can modularize our code:

### 1. By Layers

Each type of component goes into a particular module: *app.js*, *controllers.js*, *directives.js*, *filters.js*, *services.js*. Then we just refer to the main module by the *ng-app* directive.

### 2. By Features

For example, we can have *login*, *comment* & *posts* modules which exist independently and are loosely coupled. Usually, each module and its relevant JS files are placed in a separate directory.

Modularization by features clearly keeps your app maintainable and testable in the long run.

## Controllers

### The Role of a Controller

The task of a controller is to augment the scope by attaching models and functions to it.

A controller is a constructor function which is instantiated by AngularJS when it encounters `ng-controller` directive in HTML.

### Attaching Properties and Functions to Scope

    angular.module('myApp', []).controller('GreetingController', ['$scope', '$rootScope', 'customService', function($scope, $rootScope, customService) {
	    $scope.now = new Date(); // set the model 'now' on scope
	    $scope.greeting = 'Hello!'; // set the name model on scope
    }]);

A controller is associated with a `$scope` object and can depend on other components. An application has only one `$rootScope`.

Then we can use it in the HTML view. Note: *medium* is a built-in filter which formats the *date*.

    <!DOCTYPE html>
    <html ng-app="myApp">
    <head>
	    <script type='text/javascript' src="https://ajax.googleapis.com/ajax/libs/angularjs/1.2.16/angular.js"></script>
	    <script src="app.js"></script>
    </head>
    <body ng-controller="GreetingController">
	    {{greeting}} User! The current date/time is <span>{{ now | date: 'medium' }}</span>.
    </body>
    </html>

### Adding Logic to the Controller

It's just like adding model object to `$scope`. For example;

    $scope.helloMessages = ['Hello', 'Bonjour', 'Hola', 'Ciao', 'Hallo'];
    $scope.greeting = $scope.helloMessages[0];
    $scope.getRandomHelloMessage = function() {
    	$scope.greeting = $scope.helloMessages[parseInt((Math.random() * $scope.helloMessages.length))];

### Adding Instance Functions and Properties to Controllers

### Dependency Injection in Controllers With Minification