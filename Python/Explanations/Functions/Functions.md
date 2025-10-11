## Question 1

What is the error in the following snippet code:

def multi_func():
  result = int(input()) * 5
  return result     

print(result)

Good question 👇

Let’s look carefully at your code:

```python
def multi_func():
    result = int(input()) * 5
    return result     

print(result)
```

---

### ❌ **Error:**

```
NameError: name 'result' is not defined
```

---

### 🧠 **Reason:**

* The variable **`result`** is defined **inside the function** `multi_func()`.
* That means it’s a **local variable** — it only exists **within** that function.
* When you try to access `print(result)` **outside** the function, Python doesn’t know what `result` is.

---

### ✅ **Correct Code:**

You need to **call the function** and print its **returned value**:

```python
def multi_func():
    result = int(input()) * 5
    return result     

print(multi_func())
```

Now:

* The function runs.
* It takes input, multiplies it by 5.
* Returns the result.
* `print()` displays the returned value.

---

### 💡 **Summary for Interview**

> The error is `NameError: name 'result' is not defined` because `result` is a local variable inside the function and not accessible in the global scope.
> To fix it, you should call the function and print its return value using `print(multi_func())`.

## Question 2

What is the output of the following snippet:

def my_function(*students):
  print("The tallest student is " + students[2])

my_function("James", "Ella", "Jackson")


Let’s analyze the code step by step 👇

```python
def my_function(*students):
    print("The tallest student is " + students[2])

my_function("James", "Ella", "Jackson")
```

---

### **Explanation:**

1. `*students` means the function accepts **any number of positional arguments**, and they are stored as a **tuple** named `students`.

   * Here: `students = ("James", "Ella", "Jackson")`

2. `students[2]` accesses the **third element** of the tuple (indexing starts at 0).

   * `students[0]` → `"James"`
   * `students[1]` → `"Ella"`
   * `students[2]` → `"Jackson"`

3. `print("The tallest student is " + students[2])` concatenates the string.

---

### **Output:**

```
The tallest student is Jackson
```

✅ Correct.

---

If you want, I can also explain **what happens if fewer than 3 students are passed**, which is a common interview follow-up.

## Question 3

What will be the result of calling the print_info function with the arguments 'john' and 19?

def print_info(name, age=18):
    print(name, age)

print_info('john', 19)


Let’s go through this step by step 👇

```python
def print_info(name, age=18):
    print(name, age)

print_info('john', 19)
```

---

### **Explanation:**

1. The function `print_info` has **two parameters**:

   * `name` (required)
   * `age` (optional, default = 18)

2. When you call:

```python
print_info('john', 19)
```

* `'john'` is assigned to `name`.
* `19` is explicitly passed for `age` (overrides the default 18).

3. The function executes:

```python
print(name, age)
```

Which prints the values of `name` and `age` separated by a space.

---

### **Output:**

```
john 19
```

✅ Correct.

> **Note for interview:**
> Default parameters are only used **if no argument is passed**. If you pass an argument, it overrides the default.


## Question 4

Regarding the definition of the function and the sample input, Choose the correct value of the output.

Function :  def multi_func(num1,num2):
                      return num1 *num2 
Sample input: print ( multi_func(5 , num1= 10) )

Let’s carefully analyze this code snippet:

```python
def multi_func(num1, num2):
    return num1 * num2

print(multi_func(5, num1=10))
```

---

### **Step-by-Step Explanation**

1. **Function definition:**

   ```python
   def multi_func(num1, num2):
       return num1 * num2
   ```

   * `num1` → first parameter
   * `num2` → second parameter

2. **Function call:**

   ```python
   multi_func(5, num1=10)
   ```

   * `5` is passed as the **first positional argument** → Python tries to assign `5` to `num1`.
   * `num1=10` is a **keyword argument** specifying `num1` again.

3. **Conflict:**
   Python **does not allow the same argument to be passed both positionally and by keyword**.

   * Here, `num1` is getting **two values** (5 from position, 10 from keyword).

---

### ❌ **Result**

This will raise a **TypeError**:

```
TypeError: multi_func() got multiple values for argument 'num1'
```

---

### ✅ **Correct Way to Call**

You could either:

1. Use all positional:

```python
multi_func(5, 10)
```

2. Use keyword arguments only:

```python
multi_func(num1=5, num2=10)
```

Both will return `50`.

---

**Answer:**

```
TypeError: multi_func() got multiple values for argument 'num1'
```

## Question 5

Define a function that gets the user input and multiply it by a number we passed to the function.

Sample input:  6
Sample output: 

print( multi_num(5) )
30

## Question 6

Define a function that gets the user input and multiply it by a number we passed to the function.

Sample input:  6
Sample output: 

print( multi_num(5) )
30

Yes, you **can use** this one-liner version:

```python
def multi_num(num): 
    return int(input()) * num
```

---

### **How it works**

1. `num` → the multiplier passed to the function.
2. `input()` → waits for the user to type a value.
3. `int(input())` → converts the typed string to an integer.
4. `int(input()) * num` → multiplies the user input by `num`.
5. `return` → gives the result back to the caller.

---

### **Example Usage**

```python
print(multi_num(5))
```

**User enters:** `6`

**Output:**

```
30
```

---

✅ It works exactly like the previous multi-line version, but in a **compact one-liner**.

> Note: For readability, multi-line functions are usually preferred in production code.


## Question 7
