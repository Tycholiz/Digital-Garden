
### `<script />`
Javascript loaded through script tags should be located before the closing `</body>` tag.
- This is to stop JavaScript loading from blocking content loading. So, the content is loaded and the scripts are loaded next.

An alternative to this is to use async in the script tag, like so:
```html
<script src=”main.js” async></script>
```
this way it will not block the main thread and hence will not block the content from loading.

### `head`
put links to CSS style sheets in `<head></head>`
- this way, the browser styles the HTML as it loads. If style sheets are put at the bottom of the HTML, the browser will have to restyle and render the whole page from the top, which can cause a performance bottleneck.

### `<nav>`
The `<nav>` element represents a section of a page whose purpose is to provide navigation links, either within the current document or to other documents.
ex. menus, tables of contents

### `<iframe>`
Sites like Facebook and Twitter use iframes to display content (plugins) on third party websites.
- think: sometimes you see tweets on other people's website, which has the same styling that you'd find on Twitter.com.
