The statement is **True**. ✅

---

### **Explanation:**

* In Python, a **method** is a function that is **associated with an object** (usually a class instance).
* You **call it using the object**, e.g., `obj.method()`.
* Although it is called like a regular function, it automatically receives the object (`self`) as its first argument.

---

### **Example:**

```python
class Person:
    def greet(self):
        print("Hello!")

p = Person()
p.greet()  # method is called by its name, but it's associated with object 'p'
```

* Here, `greet` is a **method** of the object `p`.
* You call it like a function, but it’s tied to the object.

---

So the statement is **True**.
