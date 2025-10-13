## Question 1

Regarding the definition of the function and the sample input, Choose the correct value of the output.

Function :  def add_func(num1,num2):
            	  	return num1 + num2 
		
Sample input: print ( add_func(5 , 5) )


**Output**

10

## Question 2

The output of the following snippet of the code will be:

a = 0
def add_three(a):
	return a+3

result = add_three(3)
print(result)

**Output**
Let’s break it down 👇

```python
a = 0

def add_three(a):
    return a + 3

result = add_three(3)
print(result)
```

### 🧩 Explanation:

* The global variable `a = 0` is **not used** inside the function because the function parameter `a` **shadows** it.
* The function receives the argument `3`, so inside the function `a = 3`.
* It returns `a + 3`, i.e., `3 + 3 = 6`.

### ✅ **Output:**

```
6
```

## Question 3

The output of the following snippet of the code will be:

a = 0
def add_three(a):
	return a+3

result = add_three(a)
print(result)

Let’s go through it carefully 👇

```python
a = 0

def add_three(a):
    return a + 3

result = add_three(a)
print(result)
```

### 🧩 Explanation:

* The global variable `a` is **0**.
* The function `add_three(a)` is called with that value (`a = 0`).
* Inside the function, it returns `a + 3 → 0 + 3 = 3`.

### ✅ **Output:**

```
3
```
## Question 4

What will be the result of calling the fullname_func function with the argument 'John'?

def fullname_func(fname):
  print(fname + " Mark")

fullname_func("John")

### ✅ **Output:**

John Mark


## Question 5

The output of the following snippet of the code will be:

nums= [7,4,1]
def change_third_item(list):
	list[2] = 5

change_third_item(nums)
print(nums)


Analyzing the code step by step:

```python
nums = [7, 4, 1]

def change_third_item(list):
    list[2] = 5

change_third_item(nums)
print(nums)
```

### Explanation:

* `nums` is `[7, 4, 1]`.
* Lists in Python are **mutable**, so passing `nums` to the function allows the function to modify the original list.
* `list[2] = 5` changes the third element (`1`) to `5`.

### ✅ Output:

```
[7, 4, 5]
```


