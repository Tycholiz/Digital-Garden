
### Problem with Jest mocks and Typescript
Mocking functions happens at runtime. At compile-time however, the Typescript compiler doesn't know that you have mocked those functions. During compilation, the TSC will do a type-check and it will complain that `mockReturnValueOnce` does not exist on that function.
- To solve this, we have to extend our function so it both have the original types and the type of a Jest Mock function:
```ts
jest.mock("../models/events");
import { getEvents } from "../models/events";
const mockedGetEvents = getEvents as jest.MockedFunction<typeof getEvents>
```
