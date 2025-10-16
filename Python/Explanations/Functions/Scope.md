## Question 1

What is the output of the following snippet:

def my_function():
  x = 20
  def my_inner_function():
    print(x)
  my_inner_function()
my_function()


Let’s break it down step-by-step 👇

```python
def my_function():
  x = 20                      # x is defined in the outer function
  def my_inner_function():
    print(x)                  # inner function can access x from outer scope
  my_inner_function()         # inner function is called here
my_function()
```

### 🔹 Execution Flow:

1. `my_function()` is called.
2. Inside it, `x = 20` is created.
3. Then `my_inner_function()` is defined (it can access `x` due to closure).
4. `my_inner_function()` is called — it executes `print(x)`.
5. The value of `x` from the outer function (`20`) is printed.

### ✅ **Output:**

```
20
```


## Question 2

def my_function():
  def my_inner_function():
    x = 20
    print(x)
  my_inner_function()

my_function()


Let’s go through it step by step 👇

```python
def my_function():
  def my_inner_function():
    x = 20
    print(x)
  my_inner_function()

my_function()
```

### 🔹 Execution Flow:

1. `my_function()` is called.
2. Inside it, `my_inner_function()` is defined.
3. `my_inner_function()` is then called.
4. Inside `my_inner_function()`, `x = 20` is assigned, and `print(x)` prints `20`.

### ✅ **Output:**

```
20
```

## Question 3

What is the output of the following snippet:

def my_function():
  def my_inner_function():
    x = 20
  print(x)
  my_inner_function()

my_function()

Let’s analyze the code carefully 👇

```python
def my_function():
  def my_inner_function():
    x = 20
  print(x)
  my_inner_function()

my_function()
```

### 🔹 Step-by-step:

1. `my_function()` is called.
2. Inside `my_function`, the function `my_inner_function()` is defined (but not yet called).
3. Then, Python reaches `print(x)` — **but `x` is not defined** in this scope.

   * `x` exists only *inside* `my_inner_function`, not outside it.
4. Since `x` is not found in the local or global scope, Python raises an error.

### ❌ **Output:**

```
NameError: name 'x' is not defined
```


## Question 4

What is the output of the following snippet:

x = 30
def my_function():
  global x
  x = 20

my_function()
print(x)

Let’s go through it step by step 👇

```python
x = 30
def my_function():
  global x      # refers to the global variable x
  x = 20        # modifies the global x

my_function()
print(x)
```

### 🔹 Execution Flow:

1. Initially, `x = 30` (global variable).
2. `my_function()` is called.
3. Inside the function, `global x` tells Python that `x` refers to the **global** variable, not a new local one.
4. Then `x = 20` updates the global variable.
5. Finally, `print(x)` prints the updated global value.

### ✅ **Output:**

```
20
```

## Question 5
