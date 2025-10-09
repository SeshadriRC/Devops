## Question 1

Choose the correct answer if the following list contains the element 'A'. Check if you get "True" in the output.

Li = ['A','C','b', 1, 3, 4]

'A' in Li → True, because 'A' is present in the list.

## Question 2

What will be the output of below Python code?

my_list = [0, 3, 4, 1, 2]
print(my_list.index(1))


Answer: 3

## Question 3

What will be the output of the following Python code?

list1 = [0, 3, 4, 1, 2]
list1[1]=[8,9]
print(list1)

Answer:

[0, [8, 9], 4, 1, 2]

## Question 4

What will be the output of below Python code?

countries = ["USA", "Canada", "India"]
countries[0], countries[1] = countries[1], countries[0]
print(countries)

Answer:

Let’s analyze step by step 👇

```python
countries = ["USA", "Canada", "India"]
countries[0], countries[1] = countries[1], countries[0]
print(countries)
```

---

### Step 1 — Original list

```
countries = ["USA", "Canada", "India"]
```

---

### Step 2 — Swap first two elements

```python
countries[0], countries[1] = countries[1], countries[0]
```

* `countries[0]` (USA) is replaced by `countries[1]` (Canada)
* `countries[1]` (Canada) is replaced by `countries[0]` (USA) **simultaneously**
* Resulting list:

```
["Canada", "USA", "India"]
```

---

### ✅ Output:

```python
['Canada', 'USA', 'India']
```

## Question 5

What will be the output of the following Python code?

list1=[3,4,6,1,2]
list2=list1
list1[1]=9
print(list2)

Let’s analyze carefully 👇

```python
list1 = [3, 4, 6, 1, 2]
list2 = list1
list1[1] = 9
print(list2)
```

---

### Step 1 — Original list

```
list1 = [3, 4, 6, 1, 2]
```

---

### Step 2 — Assign `list2 = list1`

* This **does not create a new list**, it makes `list2` **reference the same list** as `list1`.
* Now both `list1` and `list2` point to the same memory location.

---

### Step 3 — Modify `list1[1]`

```python
list1[1] = 9
```

* The element at **index 1** changes from `4` → `9`
* Since `list2` references the same list, it **also sees this change**.

---

### Step 4 — Print `list2`

```
[3, 9, 6, 1, 2]
```

---

### ✅ Output:

```python
[3, 9, 6, 1, 2]
```

## Question 6

What will be the output of the following Python code?

list1 = [0, 3, 4, 1, 2]
list1[2:5]=[8,9]
print(list1)

Let’s analyze step by step 👇

```python
list1 = [0, 3, 4, 1, 2]
list1[2:5] = [8, 9]
print(list1)
```

---

### Step 1 — Original list

```
list1 = [0, 3, 4, 1, 2]
```

---

### Step 2 — Slice assignment

```python
list1[2:5] = [8, 9]
```

* `[2:5]` refers to elements at indices 2, 3, 4 → `[4, 1, 2]`
* Replacing this slice with `[8, 9]`
* The length of the new slice **does not need to match** the old slice
* Resulting list:

```
[0, 3, 8, 9]
```

---

### ✅ Output:

```python
[0, 3, 8, 9]
```


## Question 7

Let’s analyze step by step 👇

```python
list1 = [0, 3, 4, 1, 2]
list1[2:4] = [1, 2]
print(list1)
```

---

### Step 1 — Original list

```
list1 = [0, 3, 4, 1, 2]
```

---

### Step 2 — Slice assignment

```python
list1[2:4] = [1, 2]
```

* `[2:4]` refers to elements at **indices 2 and 3** → `[4, 1]`
* Replacing this slice with `[1, 2]`
* The resulting list becomes:

```
[0, 3, 1, 2, 2]
```

✅ Notice the last element `2` from the original list (index 4) **remains at the end**.

---

### ✅ Output:

```python
[0, 3, 1, 2, 2]
```

## Question 8

What is the output of the following code: (4, 6) not in [(4, 7), (5, 6), "hello"]

Let’s analyze step by step 👇

```python
(4, 6) not in [(4, 7), (5, 6), "hello"]
```

---

### Step 1 — Understand the expression

* `not in` checks whether the **element on the left** exists in the **collection on the right**.
* Left: `(4, 6)` → a tuple
* Right: `[(4, 7), (5, 6), "hello"]` → list containing 2 tuples and a string

---

### Step 2 — Check for membership

* Compare `(4, 6)` with each element in the list:

  1. `(4, 7)` → Not equal
  2. `(5, 6)` → Not equal
  3. `"hello"` → Not equal (different type)

* `(4, 6)` **does not exist** in the list.

---

### Step 3 — Apply `not in`

* Since `(4, 6)` is **not in** the list → `True`

---

### ✅ Output:

```python
True
```








