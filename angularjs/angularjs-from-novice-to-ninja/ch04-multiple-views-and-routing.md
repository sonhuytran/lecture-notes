# Multiple Views and Routing

[TOC]

As we're developing Single Page Apps, mapping each route to a particular view will logically divides our app into *manageable* parts and keeps it *maintainable*.

AngularJS routing API enables us to **map URLs with specific functionalities**, by associating **a template and controller with a particular URL**. So, we can say:

> **An AngularJS route** = a URL + a template + a controller.

When user navigates to a different URL, the route changes and content of the route are *dynamically* loaded into the page via *an AJAX call*.

> **Benefits of Single Page Apps:**
> - much less network traffic
> - faster view rendering

## Creating Multiple Views

We need to include the `angular-route.js`, then create a separate HTML file for each view. Example of a route config:

```JavaScript
angular.module('myApp').config(function ($routeProvider) {
	$routeProvider
		.when('/view1', {
			controller: 'Controller1',
			templateUrl: 'partials/view1.html'
		})
		.when('/view2', {
			controller: 'Controller2'
			templateUrl: 'partials/view2.html'
		});
});
```

## Using `$routeParams` in the Controller
## Using `ng-template`
## The `resolve` Property in the Route Config Object
## Exploring the `$location` Service
### The API
#### Writable Parts
#### Non-Writable Parts
#### Adding a New History Entry Versus Replacing One
## Events in Routing
### `$location` Related Events
### `$route` Related Events
## The `ng-include` Directive
## Introducing the Angular UI Router
### Getting Started With UI Router
#### Requirements
### Defining States