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
    }
and in the HTML view:

    <button ng-click="getRandomHelloMessage()">Random Hello Message</button>

#### What a Controller Should ***Not*** Do

* No DOM manipulation. Use `directives` instead.
* No formatting model values. Use `filters` instead.
* No repeatable code. Encapsulate them in `services` instead.

#### What a Controller Should Do

* Set the initial state of a scope by attaching models on it.
* Set functions on the scope that perform some tasks.

### Adding Instance Functions and Properties to Controllers

Instead of the `$scope` object, we can attach functions and properties directly to the controller (using `this` object). Then in the HTML, use for example `ng-controller="GreetingController as greetingController"` and `{{greetingController.greeting}}` as expression.

* Pros
	* When nested controllers have models with the same name.
* Cons
	* The `$scope` object exists for clear separation of concern between `controller` and `view`.
	* The approach is not mainstream and leads to more typing.

### Dependency Injection in Controllers With Minification

    angular.module('myApp',[])
    .controller('DemoController', ['$rootScope', '$scope', '$http', function ($rootScope, $scope, $http){
	    // controller goes here
    }]);

## Overview of Two-Way Data Binding

AngularJS guarantees that your view is always stays updated with the latest model data.

    <!doctype html>
    <html lang="en" ng-app>
    <head>
    	<meta charset="utf-8">
    	<title>Two way data binding</title>
    </head>
    <body ng-init="name='AngularJS'">
    
    <input type="text" ng-model="name"/>
    <div><h2>Hello, {{ name }}</div>
    <script src="lib/angular/angular.js"></script>
    
    </body>
    </html>

What happens step by step:

1. The `ng-app` directive bootstraps the app. AngularJS creates a `$rootScope` for the HTML page.
2. The `ng-init` directive creates a model called `name`, keeps it in the root scope and initialize it with the *AngularJS* value.
3. The `ng-model` directive attaches the model `name` to the input field. This is the basis of two-way binding.
4. The `{{ name }}` expression binds the model data to view unidirectionally. It watchs for the model value and update the DOM whenever the value changes.
5. Whenever we type into the input field, the view is updated immediately with the latest value.

### Doing Something Cool

    <!doctype html>
    <html lang="en" ng-app>
    <head>
	    <meta charset="utf-8">
	    <title>Two way data binding</title>
    </head>
    <body ng-init="fbID='sandeep.panda92'">

    <input type="text" ng-model="fbID" />
    <br/>
    
    <span><img ng-src="https://graph.facebook.com/{{fbID}}/picture?type=normal"/></span>
    
    <script src="lib/angular/angular.js"></script>
    </body>
    </html>

## Conclusion