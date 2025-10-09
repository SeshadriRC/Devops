Let’s analyze it 👇

```python
list1 = [1, 66, "python", [11, 55, "cat"], [ ], 2.22, True]
print(list1[0:4])
```

---

### 🧩 Explanation:

`list1[0:4]` means:

* Start from index **0**
* Go **up to (but not including)** index **4**

So it will include elements at indices:
`0`, `1`, `2`, and `3`.

---

### Elements:

| Index | Value             |
| ----- | ----------------- |
| 0     | `1`               |
| 1     | `66`              |
| 2     | `"python"`        |
| 3     | `[11, 55, "cat"]` |

---

### ✅ Output:

```python

[1, 66, 'python', [11, 55, 'cat']]
```

Let’s break it down 👇

## Question 2

```python
my_list = [0, 1, 2, 3, 4]
print(my_list[-1])
```

---

### 🧩 Explanation:

* In Python, **negative indices** count from the **end of the list**:

  * `-1` → last element
  * `-2` → second last element, and so on

* So `my_list[-1]` refers to the **last element** of `my_list`.

---

### List Elements:

```
Index:  0   1   2   3   4
Value:  0   1   2   3   4
```

* `my_list[-1]` → `4`

---

### ✅ Output:

```python
4
```

