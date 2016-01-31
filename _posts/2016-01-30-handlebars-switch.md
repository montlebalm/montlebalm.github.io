---
title: "Handlebars \"switch\" and alternatives"
layout: single
summary: "Creating a \"switch\" helper for Handlebars and why you might not want to use it."
---

Handlebars is a self-described logicless templating language for JavaScript. It comes with a minimal set of built-in tools and includes a plug-in system for supporting user-defined helpers. Control flow statements are particularily anemic with only `if` and `else` included out of the box. A `switch` helper [was proposed and rejected][switch-proposal] in favor of keeping the standard library lightweight. I thought a `switch` might be useful, so I created one using the Handlebars Helper API.

Handlebars includes the [registerHelper][register-helper] function, which allows you to create your own inline and block template helpers. Here's what it takes to create `switch` and `case`:

```javascript
Handlebars.registerHelper("switch", function(value, options) {
  this._switch_value_ = value;
  var html = options.fn(this); // Process the body of the switch block
  delete this._switch_value_;
  return html;
});

Handlebars.registerHelper("case", function(value, options) {
  if (value == this._switch_value_) {
    return options.fn(this);
  }
});
```

Here's how it's used inside the template:

```
&gt;&gt;#switch letter}}
  &gt;&gt;#case "a"}}
    A is for alpaca
  &gt;&gt;/case}}
  &gt;&gt;#case "b"}}
    B is for bluebird
  &gt;&gt;/case}}
&gt;&gt;/switch}}
```

Great, but why isn't this included out of the box? Given the talented team behind Handlebars you can rest assured it wasn't an oversight. No, Handlebars eschews this logic because it wants you to determine your view logic outside the template.

We can still get the conditional behavior we want and we can do it without betraying the spirit of logicless templates. Here's what it might look like if you created a specialized helper:

```javascript
Handlebars.registerHelper("letterText", function(letter, options) {
  switch (letter) {
    case "a":
      return "A is for alpaca";
    case "b":
      return "B is for bluebird";
  }
});
```

Here's how the `letterText` helper would be used in the template:

```
&gt;&gt;letterText letter}}
```

I think it's clear that the latter example makes for simpler templates and puts the branching logic back into JavaScript where it belongs. The "downside" to this approach is that you'll need to make a new handler for each use case. I put "downside" in quotes because I could make an argument that the explicit codification of the use case actually clarifies the code.

Feel free to use the `switch` helper if you feel that it's right for you though I hope I've made the case for why you might not need it.



[handlebars-repo]: https://github.com/wycats/handlebars.js	"Handlebars.js on GitHub"
[switch-proposal]: https://github.com/wycats/handlebars.js/issues/927 "Proposed switch helper"
[register-helper]: http://handlebarsjs.com/reference.html#base-registerHelper "Handlebars API: registerHelper"
