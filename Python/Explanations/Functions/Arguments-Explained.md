## Question 1

What is the output of the following snippet:

def my_function(*argv):  
    for arg in argv:  
        print(arg) 

my_function('Hello', 'World!')

Hello
World!


## Question 2

What is the output of the following snippet:

def my_function(arg1, *argv): 
    print ("First argument:", arg1) 
    for arg in argv: 
        print("Next argument:", arg) 

my_function('Welcome', 'to', 'Python!')

The output of the given code is:

```
First argument: Welcome
Next argument: to
Next argument: Python!
```

✅ **Explanation:**

* `arg1` captures the **first** argument: `'Welcome'`.
* `*argv` collects the **remaining** arguments into a tuple: `('to', 'Python!')`.
* The function prints the first argument, then iterates through `argv` and prints each remaining argument.

## Question 3

What is the output of the following snippet:

def sum(*args):
    for arg in args:
        result += arg
    return result 

print(sum(2,3,1))


This code will raise an **error**.

### ❌ **Output:**

```
UnboundLocalError: local variable 'result' referenced before assignment
```

### ⚙️ **Explanation:**

* The variable `result` is used inside the loop before it has been assigned a value.
* You must **initialize** `result` before using it.

---

### ✅ **Correct Version:**

```python
def sum(*args):
    result = 0
    for arg in args:
        result += arg
    return result

print(sum(2, 3, 1))
```

**Output:**

```
6
```

## Question 4

What is the output of the following snippet: def my_function(*argv): print(argv) my_function('Hello', 'World!')

The output of the given code is:

```
('Hello', 'World!')
```

✅ **Explanation:**

* The `*argv` parameter collects all positional arguments into a **tuple**.
* Since `'Hello'` and `'World!'` are passed, `argv` becomes `('Hello', 'World!')`.
* Printing `argv` directly displays the tuple.





