
## Layout-isolated component
- def - A component that is unaffected by the parent it is placed within, and does not itself affect the size and position of its siblings.
    - this only applies to the root element of a reusable component
![](/assets/images/2021-03-28-19-49-24.png)
- avoid any properties on the root element of a component that affect, or are affected by elements outside of the bounds of that component.
    - ex. margin, because it acts on elements outside of the component's scope
    - ex. align-self, as it will stretch the width or height of the component depending on the flex-direction of its parent
    - by contrast, padding is fine, as it is confined to the scope of the component
    - Basically, if a property depends on, or impacts other components outside of its scope, discourage its use.
```js
// Does NOT conform to layout isolation principals
function MyComponent() {
  return (
    <div style={{alignSelf: 'center'}}>
      <div />
    </div>
  
}

// This component is layout isolated, since the potentially dangerous property is not on the root element
function MyComponent() {
  return (
    <div>
      <div style={{alignSelf: 'center'}}/>
    </div>
  )
}
```

# E Resources
https://visly.app/blogposts/layout-isolated-components
