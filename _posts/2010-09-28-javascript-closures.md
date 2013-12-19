---
title: "Javascript Closures"
layout: single
summary: "Demonstrating the behavior of JavaScript closures"
---
> "Javascript functions are a combination of code to be executed and the scope in which to execute them. This combination of code and scope is known as a closure..."

> Excerpt from *Javascript: The Definitive Guide* by David Flanagan

The basic definition of a closure is a function defined within a function. The behavior of these nested functions differs between programming languages based on their handling of scope. JavaScript employs a Lexical (static) scope, which means functions execute in the same context in which they are defined. Unlike dynamically scoped languages, JavaScript not only contains a pointer to the nested function, but a reference to its call object as well.

# Call object: forget me not

JavaScript's Lexical scope allows closures to behave in seemingly counter-intuitive ways. Normally when a function exits, the interpreter releases its useless call object for garbage collection. However, if we set a nested function as the return value of a parent function, the interpreter must keep a reference to the parent's call object. This is required in order to provide the nested function with the scope in which it was defined. We can't access the stored object directly, but it will persist in memory until needed.

I have demonstrated the behavior of closures in the priceless flash card application below.

	{% highlight javascript %}
	// Define the outer function
	function flashCard(question, answer) {

	    // Define our closure to ask the question
	    function ask() {
	        alert("Question: " + question);
	        alert("Answer: " + answer);
	    }

	    // Return the closure
	    return ask();

	}

	// Create a couple flash cards
	var cards = [
	  flashCard("Who was the 2nd president of the United States?", "John Adams"),
	  flashCard("What time is it?", "It's Business Time")
	];

	// Call the closures
	cards[0](); // "Who was the 2nd... "
	cards[1](); // "What time... "
	{% endhighlight %}

If you've been following along you might correctly guess that the first closure call will display the first question/answer (the second call the second question/answer). Again, this is because `ask()` is executing in the same scope in which it was defined. In this case, its scope includes the question and answer variables that we provided while defining its parent function.

Closures probably won't change your life, but they have some interesting uses.

For more information about practical application, read [Douglas Crockford's piece on Private Members in JavaScript](http://www.crockford.com/javascript/private.html).