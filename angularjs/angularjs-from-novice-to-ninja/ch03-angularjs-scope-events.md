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

    $scope.$watch('wishListCount', function(newValue, oldValue){
	    console.log('called ' + newValue + ' times');
	    if (newValue == 2) {
		    alert('Great! You have 2 items in your wish list. Time to buy what you love. ');
		}
    });

By default, **watchers** compare objects by reference. The listener has also an optional third parameter *objectEquality* (boolean, by default *false*), we can use it to get notification whenever any of the object's properties changes.

The return value of `$scope.$watch()` is a function which can be called to **unbind** the watcher. It also clears the memory allocated to the watcher.

### The `$watchCollection()` Function

### The `$apply()` Function and the `$digest` Loop

### `$apply` and `$digest` in Action

#### Introducing the `$timeout` Service

## Broadcasting & Emitting Events

### `$scope.$emit(name, args)` for Emitting Events

### `$scope.$broadcast(name, args)` for Broadcasting Events

### `$scope.$on(name, handlerFunction)` for Registering Listeners

### Events in Action

### The `$destroy` Event

## Conclusion