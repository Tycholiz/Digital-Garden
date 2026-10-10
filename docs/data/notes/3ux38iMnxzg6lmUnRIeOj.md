
Interfaces support overriding (correct term?), allowing us to have multiple methods by the same name, so long as their signature differs:
```ts
export interface Cache {
  set<T>(key: string, value: T, options?: CachingConfig): Promise<T>;
  set<T>(key: string, value: T, ttl: number): Promise<T>;
}
```

```ts
interface SquareConfig {
  color?: string;
  width?: number;
}
```

