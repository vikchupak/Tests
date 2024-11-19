- jest.fn
- jest.spyOn
- jest.mock

Sugar syntax over the .mock property
- https://jestjs.io/docs/mock-functions#custom-matchers
- https://jestjs.io/docs/mock-functions#mock-property

# Function mocks

### Mock func

```javascript
const mockFn = jest.fn();
```

### Mock object func

```javascript
const spyMockFn = jest.spyOn(object, 'greet').mockReturnValue('Hi!');
```
- Spies on an existing function, keeping the original implementation unless overridden.
- All other methods and properties remain unchanged.

# Async function mocks

### Mock async func

```javascript
const asyncMockFn = jest.fn().mockResolvedValue(43);

asyncSpyMockFn.mockClear();
asyncSpyMockFn.mockReset();
```

### Mock object async func

```javascript
const asyncSpyMockFn = jest.spyOn(object, 'greet').mockResolvedValue('Hi!');

asyncSpyMockFn.mockClear();
asyncSpyMockFn.mockReset();
asyncSpyMockFn.mockRestore(); // Supported only by jest.spyOn
```

# Module mocks

- https://jestjs.io/docs/es6-class-mocks#the-4-ways-to-create-an-es6-class-mock

### Mock entire module
```
import axios from 'axios';

jest.mock('axios');

axios.get.mockResolvedValue({ mockedResult: 'ok' });
```
Or
```
import math from "./math";

jest.mock('./math.js')
```
- Module methods are automatically mocked with `jest.fn()`.
- Module properties and their values are removed. The properties must be explicitly defined or mocked if needed.
  - ```javascript
    // Example of moking the property with custom data
    axios.defaults = {
      baseURL: 'https://mockapi.example.com',
    };
    ```
  - ```javascript
    // Example of moking the property with the module original data
    axios.defaults = jest.requireActual('axios').defaults;
    ```
### Mock module partionaly

https://jestjs.io/docs/mock-functions#mocking-partials

# Manual module mocks

- Provide your own custom implementation of a module in a __mocks__ directory.

Example `__mocks__/math.js` file
```javascript
module.exports = { add: jest.fn(() => 5) };
```

Usage
```javascript
jest.mock('./math'); // Automatically uses the manual mock
const { add } = require('./math');
expect(add()).toBe(5);
```

# Class mocks

- The same as module mocks, but with classes
```javascript
jest.mock('./User');
const User = require('./User');

const mockUserInstance = new User();
mockUserInstance.sayHello.mockReturnValue('Hi!');
expect(mockUserInstance.sayHello()).toBe('Hi!');
```
