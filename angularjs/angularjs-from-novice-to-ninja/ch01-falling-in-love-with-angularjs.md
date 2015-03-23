## Falling In Love With AngularJS ##

[TOC]

Definition++
=======
AngularJS is a **framework that extends HTML** by teaching it **new syntax**, making it suitable for developing really great **web applications**.

With AngularJS, you can introduce **new HTML elements** and **custom attributes**.

AngularJS has been designed from ground up with **TDD** (Test Driven Development) in mind.

The Power Features of AngularJS
=======

1. Two-way Data Binding
-----------------------
In AngularJS we create templates and bind different components with specific models.

2. Structure front end code
---------------------------
With AngularJS you're going to build *solid*, *well-structured*, and *fully testable* apps. This saves you from maintenance nightmares and make your project much easier.

AngularJS is an **MVW** (Model-View-Whatever) framework where **Whatever** means **Whatever Works for You**. The reason is that AngularJS can be used both as **MVC** and **MVVM** framework.

3. Routing Support
------------------
Single Page Apps (SPAs) load the content **asynchronously** on the same page and just change the URL in the browser to reflect it. It makes users feel as if they are interacting with desktop apps.

AngularJS support **routing** with **multiple views** for different URLs: it will load the appropriate view in the main page when a specific URL is requested. We are dividing our app into different parts and thereby making it more maintainable.

4. Templating
-------------
AngularJS uses **plain old HTML** as the templating language, which makes designers and developers' jobs independent to each others.

5. Form validation
------------------
AngularJS forms incorporate **real-time form validations**, **custom validators**, **formatters**, etc. with several **CSS control state classes**.

6. Directives
-------------
Directives trick HTML into doing **new things** that are not supported natively. This is done by introducing **new elements/attributes** and teaching the **new syntax** to HTML.

7. Embeddable, testable and injectable
--------------------------------------
You can easily embed an AngularJS app within **another app**, or use it along with **other technologies**.

AngularJS favors **TDD**, supports **unit** and **End-to-End testing**.

AngularJS provides full support for **Dependency Injection**.

8. Powered by Google and an active community
--------------------------------------------
AngularJS is **open-source**, maintained by **Google** developers, released under **MIT** license and available for **download at GitHub**. The **documentation** is also pretty good. The number of developers using it is also increasing with great **tutorials** all over the web.

Download and Installation
=========================
Beside normal locally downloading & importing method, you can import it into the HTML via CDN by putting this inside the `<head/>` tag of the document:

    <script type="text/javascript" src="http://ajax.googleapis.com/ajax/libs/angularjs/1.2.16/angular.js"></script>
or minified version:

    <script type="text/javascript" src="http://ajax.googleapis.com/ajax/libs/angularjs/1.2.16/angular.min.js"></script>

Notes:

- Even-numbered versions are **stable** releases, while odd-numbered ones are for *developers only*.
- There are several intereting extra AngularJS scripts:


| Script | Description |
| ------- | ----- |
| angular-route.js | routing support |
| angular-animate.js | CSS3 animations |
| angular-mocks.js & angular-scenarios.js | support testing |
| angular-resource.js| better support for interacting with REST APIs |


Required Tools
==============
Some IDEs:

 - JetBrains Webstorm
 - Sublime Text
 - TextMate (MacOSX only)
 - NetBeans

Debugging Tools:

 - AngularJS Baratang plug-in for Chrome

The Angular Seed Project
========================
The [Angular Seed project](https://github.com/angular/angular-seed) provides a good skeleton structure for AngularJS apps so that you can quicky boostrap the app and start developing.

The Anatomy of an AngularJS app
===============================

Component           | Description
:------------------ | -----------------------------------------------------------------
1) Model            | Data shown to users, in **POJOs** (Plain Old JavaScript Objects)
2) View             | What users see after the raw HTML template involving directives & expressions is compiled and linked with corrent scope.
3) Controller       | The business logic driving the app
4) Scope            | A context holding data models & functions. A controller usually sets these models & functions in the scope.
5) Directives       | Something teaches HTML new syntax by extending HTML with custom elements & attributes.
6) Expressions      | `( {{ }} )` used for accessing scope models & functions.
7) Template         | HTML with additional markup in the form of directives & expressions.

What is MVW
-----------
AngularJS can be used to develop apps based on both the MVC or MVVM patterns.

### [MVC](https://alexatnet.com/articles/model-view-controller-mvc-javascript)
MVC improves code organization by promoting **separation of concerns**: The UI (**View**) is separated from business data (**Model**) of the app through a **Controller** which handles inputs, delegates the tasks to business logic and coordinates with the model and view.

### [MVVM](http://addyosmani.com/blog/understanding-mvvm-a-guide-for-javascript-developers/)
MVVM is a design pattern to build declarative UIs. A **ViewModel** exposes the **Model** to the **View** in a way the **View** can understand it. In other words, the **ViewModel** is a pure code representation of the **Model**.

Structuring Our Code with MVW
-----------------------------
### Benefits

 * The business logic (model) is decoupled from the UI (view).
 * Maintenance is easier because everything has its own place.
 * Modules can be loaded and tested independently.

### Components

 1. **Controller**: handles inputs, calls the business code and shares data with view via `$scope`. In AngularJS, the business logic is performed inside a service and injected into the controller. The controller uses the services to obtain data and sets it on the `$scope`, so it is just aware of the `$scope` and not the view.
 2. **Model**: represents business data. The UI is a projection of the model at any given time.
 3. **View**: is only concerned with displaying data & decoupled from the business logic. It should update itself whenever the underlying data model changes. In AngularJS the view reads model data from the `$scope`. This helps the front end development to progress in parallel with the back end activity.