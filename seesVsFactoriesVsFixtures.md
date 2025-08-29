Let’s complete the trio: **fixtures, seeds, factories**. They’re related, but each plays a slightly different role in testing and DB setup.

---

## 🌱 **Seeds**

* **DB CONTEXT ONLY**
* **What:** Predefined, static data inserted into a DB.
* **Purpose:** Ensure the system starts with known values.
* **Example:** Insert admin user, default roles, countries list.

```ts
// seed-users.ts
await userRepo.insert([
  { name: 'Admin', email: 'admin@example.com' },
  { name: 'Guest', email: 'guest@example.com' },
]);
```

* Typically **the same every time** (so devs/tests have a predictable base).

---

## 🏭 **Factories**

* **ANY CONTEXT, BUT USUALLY DB**
* **What:** Code that generates **test entities dynamically**, often randomized.
* **Purpose:** Quickly produce test data without writing it by hand.
* **Example:** Create 10 random users for testing.

```ts
import { faker } from '@faker-js/faker';

export function userFactory(overrides = {}) {
  return {
    name: faker.person.fullName(),
    email: faker.internet.email(),
    ...overrides,
  };
}
```

* Can be used to:

  * Generate a single object (`userFactory()`)
  * Generate arrays of entities (`Array.from({ length: 10 }, () => userFactory())`)
* Helps avoid brittle tests where the same hardcoded data is reused everywhere.

---

## 🔧 **Fixtures**

* **ANY CONTEXT**
* **What:** The **test setup environment** — may include seeds, factories, or even external services.
* **Purpose:** Put the system into a **known state** before running a test.
* **Example in NestJS:**

  * Spin up `PostgreSqlContainer`
  * Initialize `app = moduleRef.createNestApplication()`
  * Use seeds to insert default data
  * Use factories to insert random test data

```ts
beforeEach(async () => {
  await userRepo.clear();

  // seed: known admin user
  await userRepo.save({ id: 1, name: 'Admin' });

  // factory: 10 random users
  const users = Array.from({ length: 10 }, () => userFactory());
  await userRepo.insert(users);
});
```

---

### 📌 Quick analogy

* **Seeds** = "plant fixed test data" 🌱
* **Factories** = "machine that makes test data on demand" 🏭
* **Fixtures** = "whole test setup" (may use seeds + factories + environment) 🧰
