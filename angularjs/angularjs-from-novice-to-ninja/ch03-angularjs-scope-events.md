# AngularJS Scope & Events

[TOC]

## Scope Demystified

### Writing Access with Prototypes

#### Writing a Primitive to an Object

#### Writing a Reference Type to an Object

### Objects Can Extend Objects

## Prototypal Inheritance in AngularJS Scope

## Advanced Scope Concepts

### The Watchers in AngularJS

A **watcher** monitors model changes and takes action in response. The `$scope` object has a function `$watch()` used to register a watcher.

```JavaScript
$scope.$watch('wishListCount', function(newValue, oldValue){
    console.log('called ' + newValue + ' times');
    if (newValue == 2) {
	    alert('Great! You have 2 items in your wish list. Time to buy what you love. ');
	}
});
```

By default, **watchers** compare objects by reference. The listener has also an optional third parameter *objectEquality* (boolean, by default *false*), we can use it to get notification whenever any of the object's properties changes.

The return value of `$scope.$watch()` is a function which can be called to **unbind** the watcher. It also clears the memory allocated to the watcher.

### The `$watchCollection()` Function

We can use this function to watch a collection or an object. We will be notified when a new item is added to the array, or an existing one is removed, updated or reordered. It's the same in case of an object and its properties.
```JavaScript
$scope.$watchCollection('myCollection', function (newCollection, oldCollection) {
    // handle the change
    console.log(newCollection); //print new collection
});
```

### The `$apply()` Function and the `$digest` Loop

The **expressions** are a special type of directive that set up a **watcher** on models or functions. Their purpose is to get notified when the value of the model changes and update the DOM accordingly.

A **scope** has a function called `$apply()` which takes a function as an argument. AngularJS knows about model mutations ***only if*** that mutation is done inside `$apply()`. Then AngularJS knows that some model changes might have occurred. It starts a **digest circle** by calling `$rootScope.$digest()` - which propagates to all child scopes. Then ALL the watchers are called to check if the model value has changed.

The digest circle doesn't run only once after the `$apply()` call. It starts all over again and fires each watcher to check if any of the models have been mutated in the last loop, till NONE of the models have changed OR it reaches the maximum 10 loop counts.

> **-->** It's good to refrain from model mutation in the listener functions.

The `$digest` cycle always runs at least twice. This is called **dirty checking**. If you want a function is called whenever **$digest()** is called, pass it as the first and *only* argument of the `$watch()` function:

```JavaScript
$scope.$watch(function() {
    // do something here
    console.log('called in a digest cycle');
    return;
});
```

> Internally, `ng-click` and `ng-model` wrap the code that changes the models inside an `$apply()` call.

This is what happens when you write:
```html
<input type="text" ng-model="name"/>
```

1. The directive `ng-model` register a `keydown` listener with the input field.
2. Inside the `keydown` listener the directive assigns the new value to the scope's model. This code is wrapped inside `$apply()` call.
3. The digest cycle starts in which the watchers are called. If there is an expression `{{ name }}` in the view, a watcher is already registered. It will get called and update the DOM using `innerHTML`.
4. You can observe the result.

> If you use Angular's `$http` built-in service to make XHRs, the model mutation code is implicitly wrapped within the `$apply()` call. But with plain JavaScript, you need to mutate the model inside the `$apply()`.
> 
> If a function which changes models is passed as an argument to `$apply()`, it is evaluated and then `$rootScope.$digest()` is fired. This method should ALWAYS be used, because this function will be wrapped in a `try/catch` with the `$exceptionHandler` service.

### `$apply` and `$digest` in Action

#### Introducing the `$timeout` Service

AngularJS' `$timeout` service is just like `setTimeout()` function but it wraps your code already in the `$apply()` function. Example:

```JavaScript
angular.module('myApp', []).controller('TimeoutController', function($scope, $timeout) {
	$scope.fetchMessage = function() {
		$timeout(function() {
			$scope.message = 'Fetched after 3 seconds'; // no need for $apply()
			console.log('message=' + $scope.message);
		}, 3000);
	}
});
```

## Broadcasting & Emitting Events

AngularJS support 2 types of event propagation:

1. **Emitting** the event upwards in the scope hierarchy.
2. **Broadcasting** the event downwards in the scope hierarchy.

### `$scope.$emit(name, args)` for Emitting Events

The event life cycle starts with the `$scope` on which `$emit()` was called and is dispatched upwards in the scope hierarchy to all the registered listeners.

The `$scope` gets notified via `$on` when the event is emitted.

### `$scope.$broadcast(name, args)` for Broadcasting Events

This function is the same as `$emit()` except that the event propagates downwards in the scope hierarchy to all the child scopes.

### `$scope.$on(name, handlerFunction)` for Registering Listeners

`handlerFunction` is a `callback(event, args)`, while `event` has these properties and functions:

1. `name`
2. `targetScope`: the scope that emitted/broadcasted the event
3. `currentScope`: the scope that is handling the event
4. `stopPropagation()`: available for **emitted** events only

> The `$rootScope` can be used as a Central Message Bus to notify all child scopes.

### The `$destroy` Event

When a scope is being destroyed, a `$destroy` event is broadcast on the scope. You can use it to cancel the timers from the `$timeout` service, cancel the watchers or any custom event listeners.

Any digest cycle won't touch this and its child scopes anymore.

```JavaScript
$scope.$on('$destroy', function () {
	// clean up code
});
```
