---
title: Functional Programming Concepts
---

For those delving into the functional programming world, there are a number of concepts that are fundamental to understanding. These concepts include:
* First-class functions
* Higher-order functions
* Closures
* Pure-functions
* Partial Application
* Currying

These concepts can forever change how one approaches software and can give new inspiration to their own development. I think it is worthwhile for everyone to learn a functional language. I will be showing examples in javascript as well as in clojure.

#### First-class functions
A programming language is considered to have **first-class functions** if it treats functions as like any other variable. In other words, if a language treats functions as a **first-class citizens**(can be refered to as first-class objects), it can be passed to functions, return from functions, assigned to variables, placed in data structures, etc. It has the same flexiblity as a variable.


{% highlight js linenos %}
  // js
  // A function can be passed as a
  // parameter to another function.
  function myFunc(func, value) {
    return func(value);
  }
  // A function can be assigned to a variable
  var func = myfunc;
  console.log(func(x => x * 4), 5);
  // 20
{% endhighlight %}

{% highlight clojure linenos %}
  ;; clojure
  ;; A function can be passed as a
  ;; parameter to another function.
  (defn myFunc [func value] (func value))

  ;; A function can be assigned to a variable
  (let [f (myFunc)] (f #(* 4 %) 5))
  ;; 20

  ;; A function can be stored in a data structure
  (let [f [myFunc myFunc]] )
{% endhighlight %}

A function that takes a function as a parameter or returns a function is termed **higher-order functions**. One of the most common higher-order functions is map. Lets take a look at an example in clojure:

{% highlight clj linenos %}
  ; clojure example of map
  ; define a function that doubles a number
  (defn double [x] (* x 2))
  ; map applies a function over elements in a collection
  (map double [1 2 3 4 5])
  ; [2 4 6 8 10]
{% endhighlight %}

**first-class functions** are fundemental to functional programming which are part of its standard practice.

If a language supports treating functions as values, it is considered a **functional language**.
