---
title: >-
  AngularJS Routing: Using Otherwise without redirectTo (or how to make a 404
  page)
tags:
  - Software
date: 2014-04-24 00:08:00
---

Every example I see of the otherwise statement in the built in AngularJS routing configuration looks like this.
  <pre class="prettyprint">var app = angular.module('myApp', ['ngRoute']);

app.config(function ($routeProvider) {
    $routeProvider
    .when('/', {
        controller: 'HomeController',
        templateUrl: 'Views/home.html'
    })
    //other whens removed
    .otherwise({ redirectTo: '/' });
});</pre>

That’s nice, but what if I want to have a 404 page? At this moment the angular docs don't give much of a hint to what is possible. 

[![image](http://www.michaelware.net/image.axd?picture=image_thumb_10.png "image")](http://www.michaelware.net/image.axd?picture=image_10.png)

I really wanted a default page that let the user know something went wrong instead of just sending them to the home page.&nbsp; This turns out to be quite easy.&nbsp; The example below loads it own Controller and View when no match is found.

<pre class="prettyprint">var app = angular.module('myApp', ['ngRoute']);

app.config(function ($routeProvider) {
    $routeProvider
    .when('/', {
        controller: 'HomeController',
        templateUrl: 'Views/home.html'
    })
    //other whens removed
    .otherwise({
        controller: 'Error404Controller',
        templateUrl: 'Views/Error404.html'
    });
});</pre>

I suspect all the options for the [$routeProvider](https://code.angularjs.org/1.2.16/docs/api/ngRoute/provider/$routeProvider) work but these are all I have confirmation for.&nbsp; Also, I am sure someone is thinking “this is not a real 404 page since the response code is not a 404.”&nbsp; That’s ok with me.&nbsp; I am still going to think of it as “Page Not Found”.