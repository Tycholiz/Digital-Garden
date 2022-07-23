
```ts
class HeaderComponent {
    // with `strictPropertyInitialization` enabled, we must set properties in the constructor
    constructor(private header: string) {}

    render() {
        return `<h1>${this.header.toUpperCase()}</h1>`;
    
    constructor(private header: string) {}

    render() {
        return `<h1>${this.header.toUpperCase()}</h1>`;
    }
}
```
