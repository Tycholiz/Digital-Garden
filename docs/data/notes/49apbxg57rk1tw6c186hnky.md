
### Mocking a named import
```ts
import { getTime } from './time';

jest.mock('./time', () => ({
  getTime: () => '1:11PM',
}));
```

### Mocking only the named import (and leaving other imports unmocked)
```ts
import { getTime, isMorning } from './time';

jest.mock('./time', () => ({
  ...jest.requireActual('./time'), 
  getTime: () => '1:11PM',
  // isMorning will return its true value
}));
```

### Mocking a default import
```ts
import getDayOfWeek from './time';

jest.mock('./time', () => () => 'Monday');
```

### Mocking default and named imports
If you want to mock default and named imports, you’ll need to remember to use `__esModule: true`

```ts
import getDayOfWeek, { getTime } from './time';

jest.mock('./time', () => ({
  __esModule: true,
  default: () => 'Thursday'
  getTime: () => '1:11PM',
}));
```

### Changing what the mock returns per test
If you wanted to have `getDayOfWeek` to return a different value per test, you can use `mockReturnValue` in each of your tests
- If you only wanted to change what the mocked function returned for just one test, you need to use `mockReturnValueOnce`, otherwise you will change the mock for all other subsequent tests.
```ts
import getDayOfWeek from './time';
jest.mock('./time', () => jest.fn());

test('App renders Monday', () => {
  getDayOfWeek.mockReturnValue('Monday');
});
```

### Mocking Third Party library (e.g. Axios)
```ts
import * as axios from 'axios'

jest.mock('axios')

test('good response', () => {
  axios.get.mockResolvedValue({ status: 200, data: {...} })
})
test("bad response", () => {
  axios.get.mockRejectedValue({ ... });
})
```