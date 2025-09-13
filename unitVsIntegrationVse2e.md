# Unit vs Integration vs e2e tests

## **1. Unit Testing 🧩**

* **Scope:** Smallest piece of code (usually a function, method, or class).
* **Goal:** Verify that a single "unit" of logic works correctly in isolation.
* **Dependencies:** Mocked or stubbed (DB, APIs, services replaced with fakes).
* **Example:**

  ```ts
  // function
  function add(a: number, b: number) {
    return a + b;
  }

  // test
  expect(add(2, 3)).toBe(5);
  ```
* ✅ Fast, cheap, easy to pinpoint failures.
* ❌ Doesn’t guarantee components work together.

---

## **2. Integration Testing 🔗**

* **Scope:** How multiple units/components work together.
* **Goal:** Verify interaction between modules, services, or layers.
* **Dependencies:** Some are real (e.g., actual DB, real API call), others mocked depending on scope.
* **Example:**

  * Test that calling your **UserService** actually saves a new user in the DB.
  * Testing an API route `/register` that creates a user and sends back the response.
* ✅ Catches mismatches between components (schema, contracts, APIs).
* ❌ Slower, more complex setup than unit tests.

---

## **3. End-to-End (E2E) Testing 🌍**

* **Scope:** Full system, from start to finish, as a user would.
* **Goal:** Verify that the whole app works correctly across all layers (frontend + backend + DB + 3rd-party services).
* **Dependencies:** All real (database, API, UI).
* **Example:**

  * Open browser → go to signup page → fill in form → submit → check that confirmation email arrives and new user appears in the database.
* ✅ Most realistic, best at catching production-like failures.
* ❌ Slowest, hardest to maintain, prone to flakiness.

# Nest.js tests

If using a **`PostgreSqlContainer`** (like from [Testcontainers](https://testcontainers.com/modules/postgresql/?language=nodejs)) is generally considered **integration testing**, not full E2E.

---

### **Why it’s integration testing**

* You are **testing how your code interacts with a real database**, not a mock.
* Scope: usually **one service or module** that depends on the database (e.g., `UserService` saving/fetching users).
* You **aren’t testing the full system** (like UI, API gateway, external services, frontend interactions).
* The container is just a **stand-in for the real DB**, but it’s still limited to a specific integration layer.

---

### **When it could become E2E**

* If your tests use the PostgreSQL container **as part of a full system flow**, including frontend, API, other services, etc., then it leans toward **E2E**.
* Example of E2E:

  1. Start PostgreSQL container.
  2. Start your backend service.
  3. Start frontend or API client.
  4. Run a test that goes through the full user journey (signup → DB insert → confirmation email).

## Why the official NestJS starter template includes only unit and e2e tests

### **1. Official NestJS starter template**

* **Includes:**

  * `*.spec.ts` for **unit tests** (controller/service)
  * `*.e2e-spec.ts` for a **demo E2E test**
* **Does NOT include:** integration tests by default.

  * Reason: Integration tests usually require a **real database or services**. The starter is meant to be **simple and lightweight**.

---

### **2. What people do in real projects**

* Many developers **don’t separate “integration tests” explicitly** in NestJS starter projects.
* Instead, they **expand the E2E tests** to cover integration scenarios:

  * Start Nest application with `Test.createTestingModule()`
  * Use **real database (PostgreSQL container / SQLite / in-memory DB)**
  * Call services or endpoints and assert data persisted
* This effectively **merges integration and E2E testing** in practice.

✅ Pros:

* Simpler setup, fewer test categories to maintain.

❌ Cons:

* Slower tests (because you spin up modules or DB)
* Less clear separation between **unit logic** and **integration scenarios**

---

### **3. Alternative: Full testing template**

* Some projects (or third-party templates) separate **unit / integration / E2E** tests like this:

```
/test
 ├─ unit/
 │   └─ user.service.spec.ts
 ├─ integration/
 │   └─ app.int-spec.ts
 └─ e2e/
     └─ app.e2e-spec.ts
```

* **Unit:** Mocked dependencies, fast

* **Integration:** Real DB / services, module-level

* **E2E:** Full application flow via HTTP, including multiple modules

* But **NestJS CLI doesn’t generate this automatically** — you set it up manually.

---

### **4. TL;DR**

* **Official template:** unit + example E2E only
* **Integration tests:** usually added manually
* Many teams **repurpose E2E tests for integration**, especially if the app is small or simplicity is valued.
