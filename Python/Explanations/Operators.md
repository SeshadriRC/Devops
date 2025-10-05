## Operator Precendence - Example 1

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

## Operator Precendence - Example 2

Let's carefully evaluate this Python expression step by step:  

```python
print(10 - 6 ** 2 / 9 * 10 + 1)
```

---

### Step 1: Operator precedence
Python follows **PEMDAS/BODMAS**:

1. `**` → exponentiation  
2. `*` and `/` → multiplication/division (left to right)  
3. `+` and `-` → addition/subtraction (left to right)  

---

### Step 2: Exponentiation
```python
6 ** 2 = 36
```

Expression becomes:  
```
10 - 36 / 9 * 10 + 1
```

---

### Step 3: Division
```python
36 / 9 = 4.0
```

Expression becomes:  
```
10 - 4.0 * 10 + 1
```

---

### Step 4: Multiplication
```python
4.0 * 10 = 40.0
```

Expression becomes:  
```
10 - 40.0 + 1
```

---

### Step 5: Addition/Subtraction (left to right)
1. `10 - 40.0 = -30.0`  
2. `-30.0 + 1 = -29.0`

---

### ✅ Step 6: Output
```
-29.0
```  

---

If you want, I can also explain **how using parentheses would change this calculation**, which often confuses beginners.
