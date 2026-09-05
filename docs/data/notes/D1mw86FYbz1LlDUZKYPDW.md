
### Prevent render of component until data arrives
For example, a header that must show either a login button or a profile dropdown depending on if the user is logged in or not. Until we know, we should render nothing:

because the HTML file is built at compile-time in Next, every single user gets an identical copy of that HTML, regardless of whether they're logged in or not. Only once the js bundle is parsed and executed can we render the proper header state.
```js
const Header = () => {
    if (typeof window === 'undefined') {
        return null;
    }

    const user = getUser();
    if (user) {
        return (
        <AuthenticatedNav
            user={user}
        />
        );
    }
    return (
        <nav>
            <a href="/login">Login</a>
        </nav>
    );
};
```
