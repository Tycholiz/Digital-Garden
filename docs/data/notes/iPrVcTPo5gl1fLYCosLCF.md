
Layouts are useful when there are elements that should be visible on every page, such as top-level navigation or a footer.
- The only requirement is that the component includes a `<slot>` for the page content.

### Nested Layouts
Suppose we don't just have a single /settings page, but instead have nested pages like /settings/profile and /settings/notifications with a shared submenu (for a real-life example, see [github.com/settings](github.com/settings)).

We can create a layout that only applies to pages below /settings (while inheriting the root layout with the top-level nav):

```html
<!-- src/routes/settings/__layout.svelte -->
<h1>Settings</h1>

<div class="submenu">
	<a href="/settings/profile">Profile</a>
	<a href="/settings/notifications">Notifications</a>
</div>

<slot></slot>
```

#### Resetting the layout stack
To reset the layout stack, create a __layout.reset.svelte file instead of a __layout.svelte file. For example, if you want your `/admin/*` pages to not inherit the root layout, create a file called `src/routes/admin/__layout.reset.svelte`.

### Error pages
Sveltekit renders a default error page, but if we supply a `__error.svelte` alongside the component, then that will be shown to the user.