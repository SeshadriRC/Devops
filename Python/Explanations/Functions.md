In Python, a **function** is a **block of reusable code** that performs a specific task. Functions allow you to **organize code**, **avoid repetition**, and **make programs easier to read and maintain**.

---

### 1. **Defining a function**

Use the `def` keyword:

```python
def greet(name):
    print(f"Hello, {name}!")
```

* `def` → defines a function
* `greet` → function name
* `(name)` → parameter (input to the function)
* The indented block → code that runs when the function is called

---

### 2. **Calling a function**

```python
greet("Seshadri")  # Output: Hello, Seshadri!
```

---

### 3. **Functions with return values**

Functions can also **return a value** using `return`:

```python
def add(a, b):
    return a + b

result = add(5, 3)
print(result)  # Output: 8
```

---

### 4. **Key points**

* Functions **can take input parameters** (optional)
* Functions **can return output** (optional)
* Functions help **avoid repeating code**
* Python has **built-in functions** (like `print()`, `len()`)
* You can also write **your own custom functions**

---

### 5. **Example**

```python
def square(x):
    return x * x

print(square(4))  # Output: 16
```

---

If you want, I can also explain **the difference between built-in functions, user-defined functions, and lambda functions** in Python—they’re all slightly different. Do you want me to?
