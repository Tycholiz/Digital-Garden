

### Problem with Jest mocks and Typescript
Mocking functions happens at runtime. At compile-time however, the Typescript compiler doesn't know that you have mocked those functions. During compilation, the TSC will do a type-check and it will complain that `mockReturnValueOnce` does not exist on that function.
- To solve this, we have to extend our function so it both have the original types and the type of a Jest Mock function:
```ts
jest.mock("../models/events");
import { getEvents } from "../models/events";
const mockedGetEvents = getEvents as jest.MockedFunction<typeof getEvents>
```

# ts-jest
ts-jest is a jest transformer so that we can transform our source Typescript code into Javascript code for the purposes of our test.

## API
### `mocked(module, deep)`
The purpose of this function is to resolve typing errors
- when we declare:
```ts
const mockedFoo = mocked(foo, true)
```

all of the types within the `mockedFoo` module will be known to our jest test, and we will get no typing errors as a result.

## Resources
- [mocking classes](https://kulshekhar.github.io/ts-jest/docs/guides/mock-es6-class/)
