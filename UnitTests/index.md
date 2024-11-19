# Sync function mocks

### Mock func

```javascript
const mockFn = jest.fn();
```

### Mock object func

```javascript
const spyMockFn = jest.spyOn(object, 'greet').mockReturnValue('Hi!');
```
- Spies on an existing function, keeping the original implementation unless overridden.

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
