# afterEach scopes

In Jest, the scope of `afterEach` depends on where it is defined in your test file. Here's how the scoping works and affects its execution:

---

### 1. **Global Scope**
When `afterEach` is defined outside of any `describe` block, it applies to **all test cases** in the file, regardless of the `describe` block they belong to.

**Example:**
```javascript
afterEach(() => {
  console.log('Runs after every test in the file');
});

describe('Group 1', () => {
  test('Test 1', () => {
    console.log('Test 1');
  });
  
  test('Test 2', () => {
    console.log('Test 2');
  });
});

describe('Group 2', () => {
  test('Test 3', () => {
    console.log('Test 3');
  });
});
```

**Output:**
```
Test 1
Runs after every test in the file
Test 2
Runs after every test in the file
Test 3
Runs after every test in the file
```

---

### 2. **`describe` Block Scope**
When `afterEach` is defined inside a `describe` block, it only applies to the tests within that `describe` block.

**Example:**
```javascript
describe('Group 1', () => {
  afterEach(() => {
    console.log('Runs after tests in Group 1');
  });

  test('Test 1', () => {
    console.log('Test 1');
  });

  test('Test 2', () => {
    console.log('Test 2');
  });
});

describe('Group 2', () => {
  test('Test 3', () => {
    console.log('Test 3');
  });
});
```

**Output:**
```
Test 1
Runs after tests in Group 1
Test 2
Runs after tests in Group 1
Test 3
```

---

### 3. **Nested `describe` Blocks**
When `afterEach` is defined in nested `describe` blocks, the innermost `afterEach` runs after the corresponding test, followed by the outer `afterEach` in reverse nesting order.

**Example:**
```javascript
afterEach(() => {
  console.log('Global afterEach');
});

describe('Outer Group', () => {
  afterEach(() => {
    console.log('Outer afterEach');
  });

  describe('Inner Group', () => {
    afterEach(() => {
      console.log('Inner afterEach');
    });

    test('Test 1', () => {
      console.log('Test 1');
    });
  });
});
```

**Output:**
```
Test 1
Inner afterEach
Outer afterEach
Global afterEach
```

---

### 4. **Multiple `afterEach` in the Same Scope**
If multiple `afterEach` functions are defined in the same scope, they execute in the order they are defined.

**Example:**
```javascript
afterEach(() => {
  console.log('First afterEach');
});

afterEach(() => {
  console.log('Second afterEach');
});

test('Test 1', () => {
  console.log('Test 1');
});
```

**Output:**
```
Test 1
First afterEach
Second afterEach
```

---

### Summary of Scoping Rules
1. `afterEach` in the **global scope** applies to all tests in the file.
2. `afterEach` in a `describe` block applies only to the tests in that block.
3. Nested `afterEach` functions execute in reverse nesting order after a test completes.
4. Multiple `afterEach` functions in the same scope run in the order they are defined. 
