## Question 1

What is the output of the following snippet:

def get_odd_func(numbers):
    odd_numbers = [num for num in numbers if num % 2]
    return odd_numbers

print(get_odd_func([1, 2, 3, 4, 5, 6]))

Let's go step by step 👇

### Code:

```python
def get_odd_func(numbers):
    odd_numbers = [num for num in numbers if num % 2]
    return odd_numbers

print(get_odd_func([1, 2, 3, 4, 5, 6]))
```

### Explanation:

* The list comprehension `[num for num in numbers if num % 2]`
  checks each number in the list:

  * `num % 2` is **True (non-zero)** for **odd numbers**.
  * `num % 2` is **False (0)** for **even numbers**.
* So it filters out only the odd numbers.

### Step-by-step evaluation:

Input list → `[1, 2, 3, 4, 5, 6]`

| num | num % 2 | Include? |
| --- | ------- | -------- |
| 1   | 1       | ✅        |
| 2   | 0       | ❌        |
| 3   | 1       | ✅        |
| 4   | 0       | ❌        |
| 5   | 1       | ✅        |
| 6   | 0       | ❌        |

### Output:

````python
[1, 3, 5]
``` ✅
````

## Question 2

What is the output of the following snippet:

def my_function(names):
  for i in names:
    print(i, end=' ')

names = ["john", "mark", "emmy"]
my_function(names)

Let's break it down 👇

### Code:

```python
def my_function(names):
  for i in names:
    print(i, end=' ')

names = ["john", "mark", "emmy"]
my_function(names)
```

### Explanation:

* The function `my_function` takes a list called `names`.
* It loops through each element (`i`) in that list.
* `print(i, end=' ')` prints each name **on the same line**, separated by a space (because of `end=' '`).

### Step-by-step:

* Iteration 1 → prints `"john "`
* Iteration 2 → prints `"mark "`
* Iteration 3 → prints `"emmy "`

### ✅ Output:

```
john mark emmy 
```

## Question 3

What is the output of the following snippet:

def double_list(numbers):
  return 2 * numbers

numbers = [1, 2, 3]
print(double_list(numbers))

Let’s analyze 👇

### Code:

```python
def double_list(numbers):
  return 2 * numbers

numbers = [1, 2, 3]
print(double_list(numbers))
```

### Step-by-step:

* The function receives a **list**: `[1, 2, 3]`
* In Python, multiplying a list by an integer (`2 * numbers`) **repeats** the list — it does **not** multiply each element.

  * So, `2 * [1, 2, 3]` → `[1, 2, 3, 1, 2, 3]`

### ✅ Output:

```
[1, 2, 3, 1, 2, 3]
```

## Question 3

What is the output of the following snippet:

def get_odd_func(numbers):
    odd_numbers = [num for num in numbers if num % 2]
    return odd_numbers

print(get_odd_func([7, 4, 5, 6, 9, 8, 12]))

Let's go step by step 👇

### Code:

```python
def get_odd_func(numbers):
    odd_numbers = [num for num in numbers if num % 2]
    return odd_numbers

print(get_odd_func([7, 4, 5, 6, 9, 8, 12]))
```

### Explanation:

* The list comprehension `[num for num in numbers if num % 2]`
  keeps only **odd numbers**, since `num % 2` is **True** (non-zero) for odds and **False** (zero) for evens.

### Step-by-step filtering:

| num | num % 2 | Include? |
| --- | ------- | -------- |
| 7   | 1       | ✅        |
| 4   | 0       | ❌        |
| 5   | 1       | ✅        |
| 6   | 0       | ❌        |
| 9   | 1       | ✅        |
| 8   | 0       | ❌        |
| 12  | 0       | ❌        |

### ✅ Output:

```
[7, 5, 9]
```
