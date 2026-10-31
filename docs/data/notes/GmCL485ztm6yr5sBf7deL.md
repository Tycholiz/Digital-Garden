
### Making optional code more strict

Before
```ts
interface Address {
  street: string;
  city?: string;
}

interface User {
  name: string;
  address: Address;
  meta: Record<string, string>;
}

interface SuperUser extends User {
  permissions: string[];
}

class UserRepository {
  private users: User[];

  constructor() {
    this.users = [
      // Do not change the data. Let's assume it comes from the backend.
      {
        name: "John",
        address: undefined,
        meta: { created: "2019/01/03" }
      },
      {
        name: "Anne",
        address: { street: "Warsaw" },
        meta: {
          created: "2019/01/05",
          modified: "2019/04/02"
        }
      }
    ];
  }

  getUser(id: number): User | undefined {
    return this.users[id];
  }

  getCities() {
    return this.users
      .filter(user => user.address?.city)
      .map(user => user.address.city);
  }

  forEachUser(action: (user: User) => void) {
    this.users.forEach(user => action(user));
  }
}

const userRepository = new UserRepository();

console.log(userRepository.getUser(1).address.city.toLowerCase());

console.log(
  userRepository
    .getCities()
    .map(city => city.toUpperCase())
    .join(", ")
);

console.log(new Date(userRepository.getUser(0).meta.modfified).getFullYear());
```

After
```ts
interface Address {
  street: string;
  city?: string;
}

interface User {
  name: string;
  address: Address | undefined; /* 1 */
  meta: Record<string, string>;
}

interface SuperUser extends User {
  permissions: string[];
}

class SafeUserRepository {
  private users: User[];

    /* 2 */
  constructor() {
    this.users = [
      // Do not change the data. Let's assume it comes from the backend.
      {
        name: "John",
        address: undefined,
        meta: { created: "2019/01/03" }
      },
      {
        name: "Anne",
        address: { street: "Warsaw" },
        meta: {
          created: "2019/01/05",
          modified: "2019/04/02"
        }
      }
    ];
  }

  // `user` with given `id` might not exist, so marking the return type as possibly undefined
  getUser(id: number /* 3 */): User | undefined /* 4 */ {
    return this.users[id];
  }

  getCities() {
    return this.users
        /* 5 */
      .map(user => user.address?.city)
      .filter(city => city !== undefined);
  }

  forEachUser(action: (user: User) => void) {
    this.users.forEach(user => action(user));
  }
}

const safeUserRepository = new SafeUserRepository();

/* 6 */
console.log(safeUserRepository.getUser(1)?.address?.city?.toLowerCase());

console.log(
  safeUserRepository
    .getCities()
    /* 7 */
    .map(city => city?.toUpperCase())
    .join(", ")
);

/* 8 */
console.log(new Date(safeUserRepository?.getUser(0)?.meta.modfified ?? 0).getFullYear());
```
