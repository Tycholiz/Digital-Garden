
Have your state management system keep track of whether the drawer is open or not. Use that state to determine the transform position of the drawer:
```
transform: ${({ isOpen }) => isOpen ? 'translateX(0)' : 'translateX(-100%)'};
```
