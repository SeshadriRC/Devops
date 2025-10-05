- Operator Precendence

Let’s carefully evaluate the Python expression:

```python
print(2 * 3 ** 3 * 4)
```

---

### Step 1: Operator precedence

Python follows **order of operations (PEMDAS/BODMAS)**:

1. `**` → exponentiation
2. `*` and `/` → multiplication/division (left to right)
3. `+` and `-` → addition/subtraction

---

### Step 2: Evaluate exponentiation

```python
3 ** 3 = 27
```

---

### Step 3: Evaluate multiplications from left to right

```python
2 * 27 = 54
54 * 4 = 216
```

---

### ✅ Step 4: Output

```
216
```

---

So, running `print(2 * 3 ** 3 * 4)` will display:

```
216
```

If you want, I can also explain **why parentheses would change the result** with a quick example.
