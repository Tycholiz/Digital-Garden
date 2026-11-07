

### Pass props to StyledComponent
```ts
const Icon = styled.Image<{ width: number, height: number }>`
  width: ${p => p.width};
  height: ${p => p.height};
`;

// later
<Icon width={50} height={100}>
```

and if you want to be more precise and ignore the `onPress`, effectively giving us a subset of `Props`:
```ts
const Icon = styled.Image<Pick<Props, 'src' | 'width' | 'height'>>`
  width: ${p => p.width};
  height: ${p => p.height};
`;
```

# UE Resources
- https://blog.agney.dev/styled-components-%26-typescript/