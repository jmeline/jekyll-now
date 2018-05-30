---
title: Tagged Template literals in JS
---

I decided that I would write about tagged template literals as this is a concept that I recently discovered in the library [emotion](https://emotion.sh/) that I found pretty neat.

Before jumping into what tagged template literals are, it is valuable to grasp the concept of a template literal.

# Template Literals

A template literal are strings that allow expressions to be embedded in them. Template Literals are surrounded by back-ticks (` `).

{% highlight js linenos %}
const age = 25;
const string = `sample text`;
const string2 = `I am ${25}`; // "I am 25"
{% endhighlight %}

template literals have placeholders indicated by the ${} syntax and can execute code within them.

tagged template literals is a more advanced form of this and allows the developer to parse the literals from a function.

{% highlight js linenos %}
  const foodEvaluator = (strings, food, tastyLevel) => {
    // strings is an array
    // ["this", " is "]

    if (food === 'sushi') {
      return food + ' is the pinnicale of the food experience';
    }
    return strings[0]
      + food
      + strings[1]
      + tastyLevel;
  };

  const food = 'sushi';
  const tasty = 'delicious';

  console.log(foodEvaluator`this ${food} is ${tasty}); // "sushi is the pinnacle of the food experience"

{% endhighlight %}

That is pretty much it. I wanted to demonstrate the cool things you could do with it.

