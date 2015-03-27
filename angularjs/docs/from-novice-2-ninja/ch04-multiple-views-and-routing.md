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
		})
		.otherwise({
			redirectTo:'/view1'
		});
});
```

The `view()` function has two arguments: the path (shown in browser's url) of the view, and a **route config object**.

## Using `$routeParams` in the Controller

If we config the router with an URL like `/view1/:param1/:param2`, we can retrieve it in the controller using the `$routeParams`:

```JavaScript
function($scope, $routeParams) {
	$scope.var1 = $routeParams.param1;
	$scope.var2 = $routeParams.param2;
}
```

We can also change the URL of the browser and load the view:

```JavaScript
function ($scope, $location) {
	$scope.loadView2 = function() {
		$location.path('/view2/:' + $scope.firstname + '/:' + $scope.lastname);
	}
}
```

`$routeParams` can parse also the query string. If the URL contains `?key1=value1`, the key/value pair is added to `$routeParams`.

## Using `ng-template`

We have an option to define templates inline with the `ng-template` directive. Firstly, we create the scripts:

```HTML
<head>
	<script type='text/ng-template' id='/view1.tpl'>
		<-- HTML template goes here !-->
	</script>
	<script type='text/ng-template' id='/view2.tpl'>
		<-- HTML template goes here !-->
	</script>
</head>
```

Then in the route configuration:

```JavaScript
$routeProvider
	.when('/view1', {
		controller: 'Controller1',
		templateUrl: '/view1.tpl' // The ng-template id
	}).when('/view2', {
		controller: 'Controller2',
		templateUrl: '/view2.tpl' // The ng-template id
	});
```

## The `resolve` Property in the Route Config Object

In the route config object, we can add the property `resolve`, which can be a *string* or a *function* used to pass additional dependencies to the controller.

* If it's a string, AngularJS will try to find ***a service*** with that name.
* If it's a function, its ***return value*** will be used.

> Imagine you can use the `resolve` as a function with an AJAX call to retrieve data. This function will be executed by the time the controller is instantiated. So the return value is a **deferred type** (or **promise**).

If all the dependencies are resolved, a `$routeChangeSuccess` event is broadcast, otherwise a `$routeChangeError` is broadcast. Example:

```JavaScript
$routeProvider.when('/view2/:firstname/:lastname', {
	controller: 'Controller2',
	templateUrl: 'partials/view2.html',
	resolve: {
		names: function() {
			return ['Misko', 'Vojta', 'Brad'];
		}
	}
});
```

And then in the controller:

```JavaScript
function ($scope, $routeParams, names) {
	$scope.firstname = $routeParams.firstname;
	$scope.lastname = $routeParams.lastname;
	$scope.names = names;
}
```

At last, in the view template:

```HTML
<li ng-repeat="name in names">{{ name }}</li>
```

## Exploring the `$location` Service

The AngularJS `$location` service:

* exposes the current browser URL (obtained from `window.location`) through a well-defined API. --> We can change the URL to navigate to a different route.
* keeps itself and the URL in sync --> We can watch for changes in URL.



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