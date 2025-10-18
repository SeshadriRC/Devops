What is the output of the following command if the tuple1 has values (1,2,3,4,5) :

print(tuple1.append(6))

**answer**
Error

## Question 2

What is the output of the following snippet:

a = (10, 20, 30, 40, 50)
a = a[::-1]
print(a)

✅ **Output:**

```
(50, 40, 30, 20, 10)
```

---

### **Explanation:**

* `a[::-1]` uses **slicing** with a step of `-1`, which means **reverse the tuple**.
* So the original tuple `(10, 20, 30, 40, 50)` becomes `(50, 40, 30, 20, 10)`.

## Question 3

❌ That code will raise an **error**.

---

### **Output:**

```
TypeError: 'int' object is not iterable
```
## Question 4

What is the output of the following snippet:

x = tuple(3)
print(x)

---

### **Explanation:**

* The `tuple()` constructor needs an **iterable** (like a list, string, or another tuple) to convert into a tuple.
* You passed an **integer (`3`)**, which is **not iterable**, so Python raises a `TypeError`.

✅ Correct examples:

```python
tuple([3])      # → (3,)
tuple("3")      # → ('3',)
tuple(range(3)) # → (0, 1, 2)
```

