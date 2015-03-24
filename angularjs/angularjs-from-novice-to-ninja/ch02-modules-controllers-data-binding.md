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