# Multiple Views and Routing



[TOC]



As we're developing Single Page Apps, mapping each route to a particular view will logically divides our app into *manageable* parts and keeps it *maintainable*.



AngularJS routing API enables us to **map URLs with specific functionalities**, by associating **a template and controller with a particular URL**. So, we can say:



> **An AngularJS route** = a URL + a template + a controller.



When user navigates to a different URL, the route changes and content of the route are *dynamically* loaded into the page via *an AJAX call*.



> **Benefits of Single Page Apps:**



> - much less network traffic

> - faster view rendering



# Creating Multiple Views



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



# Using `$routeParams` in the Controller



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



# Using `ng-template`



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



# The `resolve` Property in the Route Config Object



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



# Exploring the `$location` Service



The AngularJS `$location` service:



* exposes the current browser URL (obtained from `window.location`) through a well-defined API. --> We can change the URL to navigate to a different route.

* keeps itself and the URL in sync --> We can watch for changes in URL.



> Updating browser URL with `$location` just loads a new route and does **NOT** refresh a full page. For a full page refresh or a redirection to a different URL, use `$window.location.href`.



## The API

`$location` service offers getters & setters to read and update different parts of a URL:

* read-only access: `host`, `protocol`, `port`, etc.
* read-write access: `path`, `search`, `hash`, etc.

> `<protocol>https://<host>foo.com:<port>8080/#!<path>/bar?<search>baz=23#<hash>bag`

**Note:**

* $`location.url()` returns the part of the URL after hostname and protocol, i.e. only `path + search + hash`.
* `$location.absUrl()` returns the whole URL
* `$location` integrates with the AngularJS scope life cycle by updating itself and calling `$apply()` whenever the browser URL changes. You can set up watchers and receives notifications when it changes.
* A path name always starts with a `/`. **DON'T** prefix the path name with a `/` while passing it to `$location.path()`. AngularJS adds it automatically.
* `search()` is an object containing key/value pairs.
* the `hash()` is a REAL anchor and has nothing to do with routes.

### Adding a New History Entry Versus Replacing One

Call `$location.replace()` right AFTER modifying the URL to replace the last entry in the browser's history instead of creating a new one. Example:

```JavaScript
$location.path('/view2').search({key1:value1}).hash('div1'); // change the parts
$location.replace(); // change the URL, but don't create a new history entry
```

# Events in Routing

All these events are broadcast in the `$rootScope`.

## `$location` Related Events

* `$locationChangeStart`: broadcast just before the browser URL changes as a result of `$location`'s setters.
* `$locationChangeSuccess`: broadcast after the browser URL changes, but won't be fired if `preventDefault` is called in `$locationChangeStart`.

## `$route` Related Events

* `$routeChangeStart`: broadcast just before AngularJS starts changing the route.
* `$routeChangeSuccess`: broadcast after the `$route` service is able to successfully resolve all the dependencies needed to load the new route.
* `$routeChangeError`: broadcast if at least one of the dependencies cannot be resolved to change the route.

# The `ng-include` Directive

The `ng-include` directive can be used to fetch, compile and include an external HTML fragment in the main page. Example:

```html
<div ng-include="'/partials/fragments/selected-user.html'"/>
```

or

```html
<ng-include src="'/partials/fragments/selected-user.html'" />
```

**Note:**

* The template path string must be wrapped inside single quotes.
* The `src` attribute can be an `ng-expression`.

# Introducing the Angular UI Router

## Getting Started With UI Router

### Requirements

## Defining States