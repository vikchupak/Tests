Differ mainly by how much internal knowledge of the system the tester has.

### **1. Black Box Testing**

* **Knowledge:** Tester has **no knowledge of internal code/structure**. They only see the system from the outside, like a user.
* **Focus:** Inputs and outputs.
* **Examples:**

  * Testing a login form by entering valid/invalid credentials.
  * Checking if API endpoints return correct responses for different inputs.

---

### **2. White Box Testing**

* **Knowledge:** Tester has **full knowledge of the internal code** (like a developer).
* **Focus:** Internal logic, code paths, and implementation details.
* **Examples:**

  * Writing unit tests for functions/methods.
  * Verifying that all branches of an `if-else` statement are tested.

---

### **3. Grey Box Testing**

* **Knowledge:** Tester has **partial knowledge** of the internal structure.
* **Focus:** Combination of external functionality and some internal workings.
* **Examples:**

  * Testing a web app with **access to database schemas** but not full source code.
  * Using knowledge of algorithms or caching mechanisms to design smarter test cases.
