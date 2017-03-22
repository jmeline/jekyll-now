---
title: Higher order functions in clojure
---


A function that takes a function as a parameter or returns a function is termed **higher-order functions**.

If a language supports treating functions as values, it is considered a **functional language**.

{% highlight clojure linenos %}
 (defn logger [level]
  (condp = level
    :warning  (fn [x] "warning: " x)
    :fatal    (fn [x] "fatal: " x)))

 (def warn-logger (partial logger :warning))

 (warn-logger "something went wrong")
 ; => "warning: something went wrong"
{% endhighlight %}
