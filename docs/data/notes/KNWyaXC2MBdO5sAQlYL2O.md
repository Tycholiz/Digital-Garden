
### `useState` types
```tsx
type Props = {
  setFormSubmitted: React.Dispatch<React.SetStateAction<boolean>>;
}
```

### Constrain component props
```tsx
// helper
export const propertyOf = <TObj>(name: keyof TObj) => name;

<Component
    id={propertyOf<OperationalDetailsValues>('providerCount')}
/>
```

# Resources
- [React+Typescript Cheatsheet](https://github.com/typescript-cheatsheets/react)
