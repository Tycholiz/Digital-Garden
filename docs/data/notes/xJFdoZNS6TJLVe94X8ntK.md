
Discriminated unions are a concept closely related to [[state machines|general.patterns.state-machine]].

Discriminated unions allow us to compose types and enforce some business logic rules *at compile-time*.
For example, we can implement the following logic:
> Customer needs to provide either email or phone number.
An obvious (and naive) solution to this is to just make both the `email` and `phone` properties optional; but then we end up with the possibility that neither is provided.

Instead, we can create a new type `Contact` which is the union of 2 interfaces: 1 has a `phone` property, and the other has a `email` property:
```ts
interface EmailContact {
    kind: 'email';
    email: string;
}

interface PhoneContact {
    kind: 'phone';
    phone: number;
}
  
type Contact = EmailContact | PhoneContact;

interface Customer {
    name: string;
    contact: Contact;
}

function printCustomerContact({ contact }: Customer) {
    // Here comes the power of discriminated unions. TypeScript analyses the code and sees an if statement that checks whether contact.kind is equal to "email". If that’s true, it can be certain that the type of contact is EmailContact, so it narrows the type inside the first if branch. The only other option for contact is to be a PhoneContact, so the type is narrowed to PhoneContact in the second if branch.
    if (contact.kind === 'email') {
        // Type of `contact` is `EmailContact`!
        console.log(contact.email);
    } else {
        // Type of `contact` is `PhoneContact`!
        console.log(contact.phone);
    }
}
```

Each union member follows a special convention of having a `kind` property. This property is called a *discriminator*, and it encodes type information into the object so that it is available at runtime. Now, TypeScript can figure out which union member you’re dealing with by looking at the `kind` property.
- `kind` is just a convention, and it can be any word.


### Example
Users place orders for products. Users have contact information, email or postal addresses, and at least one is required. Orders should include price, product name, quantity, payment date, paid amount, sending date, and delivery date.
```ts
type Customer = {
  name: string;
  contactInfo: ContactInfo;
};

type ContactInfo =
  | { kind: "emailOnly"; email: string }
  | { kind: "postalOnly"; address: string }
  | { kind: "emailAndPostal"; email: string; address: string };

type PaidOrderData = { paymentDate: Date; amount: number };
type SentOrderData = { sendingDate: Date };
type DeliveredOrderData = { deliveryDate: Date };

type OrderState =
  | { kind: "new" }
  | { kind: "paid"; paidData: PaidOrderData }
  | { kind: "sent"; paidData: PaidOrderData; sentData: SentOrderData }
  | {
      kind: "delivered";
      data: PaidOrderData;
      sentData: SentOrderData;
      deliveredData: DeliveredOrderData;
    };

type Order = {
  customer: Customer;
  state: OrderState;
  productName: string;
  price: number;
  quantity: number;
};
```

### Multi-value Discriminator
a discriminator does not have to be a single property. A group of literal properties can also act as a discriminator! In such a case, every combination of values marks a different member of the union type.
```ts
type Foo =
  | { kind: 'A', type: 'X', abc: string }
  | { kind: 'A', type: 'Y', xyz: string }
  | { kind: 'B', type: 'X', rty: string }

declare const foo: Foo;

if (foo.kind === 'A' && foo.type === 'X') {
    // here, the intellisense on `foo` would show how it's type is narrowed to the only type it could be, namely the first one shown in `Foo`
  console.log(foo.abc);
}
```

