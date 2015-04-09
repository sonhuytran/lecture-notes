[TOC]

**Services**, **factories** and **providers** are three components that

- hold some logic required at multiple points within your app
- are all used to encapsulate repetitive logic, but subtly differ from each other
- are all *injectable types* (can be *injected* into controllers and other services through Dependency Injection)

# Services

A service encapsulates some specific business logic and exposes an API, to be used by other components, to invoke this logic.

> - AngularJS services are always **singletons**: there are *NEVER* two service instances.
> - Services are **lazily instantiated**: AngularJS instantiates the service only when it encounters a component that declares dependency on the service.
> - A service can also depends on other services.
> - In practice, you should use services to encapsulate the repeatable logic and share data among difference components of your app, e.g. REST interactions

## Declaration

```JavaScript
angular.module('myApp').service('helloService', function() {
	this.sayHello = function() {
		alert('Hello! Welcome to AngularJS service!');
	}
});
```

## Usage

```JavaScript
angular.module('myApp').controller('TestController', function($scope, helloService) {
	helloService.sayHello();
});
```

## Eager Loading of a Service

A service can be eagerly loaded in the `module.run()` function:

```JavaScript
angular.module('myApp').run(function(helloService) {
	// helloService has been instantiated & can be used here
})
```

# Factory

Effectively, a factory is the same as a service, but more *verbose* and *configurable*:

> - Factories give you the freedom to determine what to instantiate and return based on certain parameters.
> - Factories are also singletons and can depend on each other & other components.
> - The function given to the `factory()` method is *NOT* a constructor like services. It will be invoken each time the factory is *injected* to another component.

## Declaration

```JavaScript
angular.module('myApp').factory('helloService', function() {
	return {
		sayHello: function(name) {
			alert('Hello' + name);
		},
		
		echo: function(message) {
			alert(message);
		}
	};
});
```
## Usage

Used like a service.

# Provider

> - Provider is *the most verbose & configurable version* of services because it's based on *priority settings or logic*.
> - Every service created is automatically associated with a provider by AngularJS.
> - A provider can be, and can ONLY be injected into our module's `config()` block. But you don't necessarily need to call `config()` to instantiate the provider. It is instantiated when the module is loaded.
> - Providers are instantiated ONLY ONCE. Later, other components just need to ask for the service provided by the provider. 

## Declaration

```JavaScript
angular.module('myApp').provider('greet', function() {

	// Configuration
	this.greeting = 'Hello';

	this.setGreeting = function(greeting) {
		this.greeting = greeting;
	}
	
	// This will be called to obtain the service
	this.$get() = function() {
		var greeting = this.greeting;

		return function(name) {
			alert(greeting + ', ' + name);
		}
	}
});
```

## Usage

> - When we passed the name `greet` to `provider()`, the actual provider's name is `greetProvider`.
> - When we declares a dependency on the `greet` service, AngularJS calls `$get()` on the `greetProvider` instance and *inject whatever it returns*.

```JavaScript
// Configuration of the greetProvider (can ONLY be configured here)
angular.module('myApp').config(function(greetProvider) {
	greetProvider.setGreeting('Hola');
});

// Usage of the greet service provided by the greetProvider
angular.module('myApp').controller('TestController', function(greet) {
	greet('Sandeep');
});
```

# Value

## Caracteristics

**A value:**

- is an *injectable type*
- is used to register a simple service (a string, number, array, function, or an object) 
- cannot be injected in `module.config()`

## Example

```JavaScript
angular.module('myApp').value('appVersion', '1.0');
```

# Constant

## Caracteristics

A **constant** is the same as a **value**, EXCEPT that it CAN be injected into `module.config()`.

## Example

```JavaScript
angular.module('myApp').constant('DATA_SOURCE', 'a string here');
```

# Using Decorators

> - Sometimes, you'll need to extend the functionality of a 3rd-party service, but don't want, or cannot modify its source code. A **decorator** intercepts the creation of a service and modifies its default behavior on the fly.
> - A decoration may return *an object* which wraps the original service and delegate tasks to it, or attach *new functions/properties* to the original service.
> - You should check for the existence of functions/properties before attaching them by using the `Object.hasOwnProperty()` function and throwing an exception on that case.

```JavaScript
angular.module('myApp').config(function($provide) {
	
	// $delegate is the original $log service, can inject other services here
	$provide.decorator('$log', function($delegate) { 
		$delegate.postInfoToURI = function(message) {			
			
			// send data to server
			$delegate.log('Data to post: ' + message);
			$delegate.log('Sending data to server');
		}

		return $delegate; // the modified $log service
	});
});
```