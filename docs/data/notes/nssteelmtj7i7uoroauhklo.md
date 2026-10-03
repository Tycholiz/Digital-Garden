
Namespaces provide a mechanism for organizing code and declarations in hierarchies of named containers.

Namespaces have named members that each denote a value, a type, or a namespace, or some combination thereof, and those members may be local or exported.

The body of a namespace corresponds to a function that is executed once, thereby providing a mechanism for maintaining local state with assured isolation. 
- Namespaces can be thought of as a formalization of an [[IIFE|js.lang.functions.IIFE]]

Namespaces are either instantiated or non-instantiated. 
- A non-instantiated namespace is a namespace containing only interface types, type aliases, and other non-instantiated namespace. 
- An instantiated namespace is a namespace that doesn't meet this definition. 
  - In intuitive terms, an instantiated namespace is one for which a namespace instance is created, whereas a non-instantiated namespace is one for which no code is generated.

A namespace identifier can be referenced in one of two ways:
- as a NamespaceName, in which it denotes a container of namespace and type names
- as a PrimaryExpression, in which it denotes the singleton namespace instance

```ts
namespace M {  
  export interface P { x: number; y: number; }  
  export var a = 1;  
}

var p: M.P;             // M used as NamespaceName  
var m = M;              // M used as PrimaryExpression  
var x1 = M.a;           // M used as PrimaryExpression  
var x2 = m.a;           // Same as M.a  
var q: m.P;             // Error
```

Above, when `M` is used as a PrimaryExpression it denotes an object instance with a single member `a` and when `M` is used as a NamespaceName it denotes a container with a single type member `P`. The final line in the example is an error because `m` is a variable which cannot be referenced in a type name.
- If the declaration of `M` above had excluded the exported variable `a`, `M` would be a non-instantiated namespace and it would be an error to reference `M` as a PrimaryExpression.
