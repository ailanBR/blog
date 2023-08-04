---
title: "Clojure Functional and Fun"
date: 2023-07-06T21:20:00+03:00
description: "Let's talk about ."
tags: [Programming, Clojure]
---

> If you can't fly then run, if you can't run then walk, if you can't walk then crawl, but whatever you do you have to keep moving forward. 
> 
> [Martin Luther King Jr](https://pt.wikipedia.org/wiki/Martin_Luther_King_Jr.)

# Resume

Let's talk about `Clojure` my favorite programming language (not only because is what pay my bill's xD), the first look was not the best (I need to be realist) but from the beginning was my intent to enjoy the learning journey and be proficient enough to swim.

Here I will explain what I understood as the main thing about `Clojure` - Functions !  

# First of all

#### [How to install](https://clojure.org/guides/install_clojure) 
#### [Getting Start](https://clojure.org/guides/getting_started)

Wait a little there, Clojure ?!

```Clojure
(print "Hello world") ;Hello World
```

Here are what you want, a clojure `hello world`. what the * is that ? Hold on.

Parentheses `()` is the Clojure way to call a function, and what we did was call the native function `print` with one parameter that was `"hello world"`. 

# The creator

In the last step we call a predefined function, to create you own function one of the ways is call the `def`, that is the way that we create vars in clojure and functions! like that :


```Clojure
(def hello-person (fn [name] (str "Hello world " name)))
```

> `def` function receive important parameters :
>
>`hello-person` => There we are defining the name that we want;
>
> `(fn)` => We use that to create a kind of anonymous function
> 
>`[name]` => An array of parameters, in this case our function have only one parameter `name`;
>
>`(str "Hello world" name)` => after the parameters array is where we play, we decide to call the function `str` using `"Hello world" name` as parameters.

But that is not the common way to create functions in clojure, for that we have the `defn`, that do the same, but more sophisticated.

```Clojure
(defn hello-person [name] (str "Hello world " name))
```

**And calling** 

```Clojure
(hello-person "Lob") ;Hello world Lob
```

# Another beginning info about def

We see **one** of the ways to create functions in clojure using `def`, `fn` and `defn` function, that is what we will do at most.

I mentioned that `defn` is the most used way to create functions and is important to know that `def` if the common way to create vars, like that :

```Clojure
(def hello-lob "Hello world Lob")
```

**Calling**

```Clojure
hello-lob ;"Hello world Lob"
```

here we don't need the parentheses to get the `hello-lob` value, another important information is that vars in clojure are immutable, to change the value of our var we need to recreate there.

# Exercise Exercise Exercise

Let's try to use our `hello-person` function to another thing, we will create a function to get the quantity of letter from the phrase returned.

```Clojure
(defn get-letters-quantity 
  [hello-phrase]
      (count hello-phrase))
```

**Calling...**

```Clojure
(get-letters-quantity "Hello world Lob") ;15
```

hmmm... we can do that different

```Clojure
(get-letters-quantity (hello-person "Lob")) ;15
```

But the demand changed, now we only need to now if the quantity of letter of the phrase is more than 10, easy !

```Clojure
(defn more-than-ten? 
  [letters-quantity]
  (> letters-quantity 10))
```

> The `>` function already return a boolean

**Calling**

```Clojure
(more-than-ten? 15) ;true
```

Or 

```Clojure
(more-than-ten? (get-letters-quantity (hello-person "Lob"))) ;true
```

Can be better ? maybe...

```Clojure
(-> "Lob" 
    hello-person 
    get-letters-quantity 
    more-than-ten?) ;true
```

What is the magic ? We have no magic here, the `->` is a macro named as "thread-first", that get the first parameter and pass as first parameter of you second parameter and continues doing that at the last parameter.

1. `"Lob"` will be sent to `hello-person` as first parameter, turning in this `(hello-person "Lob")`
2. The result of `(hello-person "Lob")` will be sent to `get-letters-quantity`, turning in this `(get-letters-quantity (hello-person "Lob"))`
3. The result of `(get-letters-quantity (hello-person "Lob"))` will be sent to `more-than-ten?`, turning in this `(more-than-ten? (get-letters-quantity (hello-person "Lob")))` 

Important information here is that we are using specific `->` "thread-first" macro
 
We have you brother `->>` "thread-last" macro, the difference follow the name, the result of the first will be sent as the last parameter of the second

It means that if we have a sequence of functions that we want to pass throw the last parameter we need to use the `->>` "thread-last" macro, example :

if we change the functions `hello-person` and create a new function `more-than-target?` more flexible than our `more-than-ten?`, like that :

```Clojure
(defn hello-person [hello name] (str hello " " name))

(defn more-than-target?
      [target letters-quantity]
      (> letters-quantity target))
```

We will need to change our thread-first to thread-last function AND pass the first parameters of the changed functions

```Clojure
(->> "Lob" 
     (hello-person "Hello world") 
     get-letters-quantity
     (more-than-target? 10)) ;true
```

