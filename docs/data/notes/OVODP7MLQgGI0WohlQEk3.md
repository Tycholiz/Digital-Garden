
The DOM is just a big nested object that can be represented as a tree:
![](./assets/images/2021-11-06-09-53-30.png)

Another way to view the tree is like this, which is a more accurate representation of what it actually looks like:
![](./assets/images/2021-11-06-09-54-44.png)

HTML becomes the DOM after the browser parses it.

DOM is an API for HTML or XML documents and it creates a logical structure which can be accessed and manipulated.

The DOM was created so we could have a standard API that could be used with any programming language.

This object would include everything that's inside the scope of your browser, including `window`, `navigator`, `document`, global variables

The inspector tab looks like HTML, but in fact it is just an HTML-like representation of the DOM. The reason it looks like HTML is because someone thought it would be a good idea to show the DOM as HTML tags instead of objects. This makes sense, since the developer writes HTML, which is parsed into the DOM, and it looks similar to how the HTML was authored.
