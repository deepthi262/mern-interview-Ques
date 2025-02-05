### **Promises `.then()` vs Callbacks: How & When to Use Which?**

#### **1. Callbacks**
- A **callback** is a function passed as an argument to another function, which is executed once an operation completes.
- Example:
  ```js
  function fetchData(callback) {
    setTimeout(() => {
      callback(null, "Data received");
    }, 1000);
  }

  fetchData((err, data) => {
    if (err) console.error(err);
    else console.log(data);
  });
  ```
- **Issues with Callbacks:**
  - **Callback Hell**: Nesting multiple callbacks leads to unreadable and hard-to-maintain code.
  - **Error Handling**: Error propagation requires checking `err` manually in each callback.

#### **2. Promises (`.then()`)**
- A **Promise** is an object that represents the eventual completion (or failure) of an asynchronous operation.
- Example:
  ```js
  function fetchData() {
    return new Promise((resolve, reject) => {
      setTimeout(() => resolve("Data received"), 1000);
    });
  }

  fetchData()
    .then((data) => console.log(data))
    .catch((err) => console.error(err));
  ```
- **Advantages of Promises:**
  - **Chaining**: Multiple `.then()` calls avoid deep nesting (solves callback hell).
  - **Error Handling**: Errors are automatically passed down the chain and caught with `.catch()`.

---

### **Which is Better & When to Use Which?**
| Feature          | Callbacks | Promises (`.then()`) |
|-----------------|-----------|----------------------|
| Readability     | Harder to read (nested) | Easier to read (chained) |
| Error Handling  | Requires manual checks | Centralized `.catch()` |
| Composability   | Hard to chain | `.then()` allows chaining |
| Debugging       | Harder | Easier with stack traces |
| Performance     | Slightly better (less overhead) | Slightly more memory usage |

- **Use Callbacks** when:
  - Working with legacy Node.js APIs (e.g., `fs.readFile`).
  - Performance is critical, and you don’t want extra overhead.

- **Use Promises** when:
  - Writing new code, as they offer better readability and error handling.
  - You need multiple asynchronous operations to execute sequentially (e.g., API calls).

---

### **Memory Management in Callbacks vs Promises**
- **Callbacks:**
  - Each callback reference stays in memory until the function completes.
  - If callbacks are nested, it may lead to **memory leaks** if closures hold references to outer scopes.

- **Promises:**
  - Promises use the **microtask queue** for execution, keeping them efficient.
  - A resolved or rejected Promise **does not hold memory references indefinitely**.
  - However, **unhandled promises (not calling `.catch()`) may cause memory leaks**.

**Best Practice for Memory Management:**
- Always call `.catch()` on Promises to handle rejections.
- Avoid deep nesting of callbacks (use Promises or `async/await` instead).
- Use weak references where necessary to prevent closures from holding unused memory.

---

### **Final Verdict**
- **Use Promises (`.then()`) over Callbacks** for new code due to better readability, error handling, and composability.
- **Callbacks may be faster in some cases** (lower memory overhead), but they introduce complexity in larger applications.
- **For even better syntax**, use `async/await`, which is built on top of Promises and offers cleaner code.

