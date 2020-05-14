---
layout: post
title: "HOW TO PASS A VARIABLE BY VALUE TO ASYNC CALLBACKS IN JAVASCRIPT (NODEJS)"
date:   2018-07-21 14:57:42 +0530
categories: Programming Javascript
---

One of the hurdles you would have come across as a javascript developer is how to pass a variable by value to async callbacks in Javascript when the variable is changing inside a loop. When a variable is declared as `var`var , it gets hoisted to the top of the scope and the same variable gets shared inside the callback function as well. This becomes a problem because if the variable is getting incremented or changed in some way.

Wrong code
----------

The value of the variable which is passed to callback won’t be the same when the callback is actually getting executed.  
An example for this is,

{% highlight javascript %}
for (var i = 0; i < 3; i++) {
    setInterval(function() {
        console.log(i)
    }, 500)
}
#=> The output of the above code is 3, 3, 3 which is not what you would expect.
{% endhighlight %}

 When I first faced this problem, I was expecting it to print `0, 1, 2`0, 1, 2 but instead, I got the final value of the variable printed out. After a lot of digging around, I managed to find a solution which is binding variable to the function’s scope. This way the variable is passed as value instead of referring to the parent variable.

The solution?
-------------

There are multiple ways to fix this issue- one is declaring the variable as `let`let instead of `var`var like so,

{% highlight javascript %}
for (var i \= 0; i < 3; i++) {
    setInterval(function() {
        console.log(i)
    }, 500)
}
{% endhighlight %}

There is another approach to solve this problem. There may be cases where you can not use `let`. This happened to me in my job where we have a legacy application with an old custom code minification library which couldn't process the newer keywords like `let` I had to find another solution because changing/updating is not an option and requires multiple levels of approvals, so I came up with another solution of binding variables to the function scope. Here is an example,

{% highlight javascript %}
for (var i = 0; i < 3; i++) {
    setTimeout(function() {
        console.log(this.i)
    }.bind({ i: i}), 500)
}
#=> Now the output is 0, 1, 2 which is exactly what we wanted.
{% endhighlight %}

In the above example, we are binding the global var to function scope using the bind keyword.

This is one of the most commonly asked javascript interview questions also. Do let me know your thoughts in the comments.