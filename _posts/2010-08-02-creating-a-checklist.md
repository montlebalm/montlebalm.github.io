---
title: "Creating a Checklist"
layout: single
summary: "Developing a todo app for no reason"
---
Lots of sites have them. No matter the name, "Add to WishList", "Save for later", and "Queue", all refer to the same thing: storing a piece of information for the future. These lists work great for tracking items on an specific site, but fail to provide anything outside their domain.

The desire to track things across sites led me to come up with my Checklist. The concept is to create an online application that's agnostic to the origin of its list's items. The feature set can fit on a small napkin (I have proof) and includes:

- Creating lists
- Adding list items
- Specifying a relevant url and descripiton for each list item
- Completing, editing, and deleting lists and items

Almost everything is accomplished through AJAX so it feels pretty zippy. The only page refresh occurs when you click to view the items inside a list.

# Examples

Say you're looking to upgrade a Home Theater system and want to keep track of some competing products. We can add each system to a list, provide a link to the site with the best price, and jot down some reasons why we like it over others. We can add and remove items when needed and delete the entire set once we've made our final purchasing decision.

In another example, we want to make a checklist for our upcoming camping trip. We can specify items we already have but need to pack like matches, clothes, and water. In addition, we can list things we need to buy like backpacks, sleeping bags, and tents. Each item left to purchase can contain a link to it's page on [REI.com](http://www.rei.com/) where we can order it. We can check off items as we pack so our list will accurately reflect what's left to be done.

# Code

The code base is a mix of PHP, MySQL, and Javascript (+YUI 3). All of the user interaction is handled through AJAX, which means the Javascript is doing most of the heavy lifting. Luckily, YUI 3 makes it easy to implement async calls using the IO module. Here's an example of deleting an item with an id of 10:

{% highlight javascript %}
// load the io module
YUI().use("io", function(Y) {
    // configure the async call
    var cfg = {
        method: "POST",
        data: "type=delete&id=10",    // call arguments
        headers: { "X-Transaction": "Post Data" }
    }
    // make the call
    Y.io("handleAjax.php", cfg);
}
{% endhighlight %}

Of course, in real code you'd abstract this out to a much simpler form. I only left it in the verbose form to show the full call signature.

This was my first project using YUI 3 and it's obvious the Yahoo team has come a long way since version 2. The syntax is much more succinct and results in more readable code.

# Future

This project screams for a mobile version. Syncing across all your internet-aware devices is low hanging fruit. I haven't created any mobile sites and this would be a great first candidate so you may see another version of this in the future.

I've also been thinking about shared lists with other users. It would be useful in a variety of situations but requires much more infrastructure.