

### Use Prop type to pass to StyledComponent
```ts
interface Props {
  onPress: any;
  src: any;
  width: string;
  height: string;
}

const Icon = styled.Image<Props>`
  width: ${p => p.width};
  height: ${p => p.height};
`;
```

and if you want to be more precise and ignore the `onPress`, effectively giving us a subset of `Props`:
```ts
const Icon = styled.Image<Pick<Props, 'src' | 'width' | 'height'>>`
  width: ${p => p.width};
  height: ${p => p.height};
`;
```
