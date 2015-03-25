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

This is what happens with `<input type="text" ng-model="name">`:

### `$apply` and `$digest` in Action

#### Introducing the `$timeout` Service

## Broadcasting & Emitting Events

### `$scope.$emit(name, args)` for Emitting Events

### `$scope.$broadcast(name, args)` for Broadcasting Events

### `$scope.$on(name, handlerFunction)` for Registering Listeners

### Events in Action

### The `$destroy` Event

## Conclusion