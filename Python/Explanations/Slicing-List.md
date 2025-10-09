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

## Question 3

What will be the output of below Python code?

my_list = [0, 1, 2, 3, 4]
my_list.append("python")
b = my_list[1:]
print(b)

Let’s analyze step by step 👇

```python
my_list = [0, 1, 2, 3, 4]
my_list.append("python")
b = my_list[1:]
print(b)
```

---

### Step 1️⃣ — Initial list

```python
my_list = [0, 1, 2, 3, 4]
```

---

### Step 2️⃣ — Append `"python"`

```python
my_list.append("python")
```

Now:

```python
my_list = [0, 1, 2, 3, 4, "python"]
```

---

### Step 3️⃣ — Slice `[1:]`

```python
b = my_list[1:]
```

* `[1:]` means **start from index 1 till the end**
* Index 1 → `1`, Index 2 → `2`, Index 3 → `3`, Index 4 → `4`, Index 5 → `"python"`

So:

```python
b = [1, 2, 3, 4, "python"]
```

---

### ✅ Step 4️⃣ — Print result

```python
print(b)
```

**Output:**

```python
[1, 2, 3, 4, 'python']
```


## Question 4

What will be the output of below Python code?

my_list = [0, 1, 2, 3, 4]
print(my_list[::-1])

Let’s break it down 👇

```python
my_list = [0, 1, 2, 3, 4]
print(my_list[::-1])
```

---

### 🧩 Explanation:

* `my_list[::-1]` is **list slicing** with a **step of -1**.
* This means: **start from the end and move backwards**, effectively reversing the list.

---

### Step 1 — Original list

```
[0, 1, 2, 3, 4]
```

### Step 2 — Reverse using `[::-1]`

```
[4, 3, 2, 1, 0]
```

---

### ✅ Output:

```python
[4, 3, 2, 1, 0]
```

