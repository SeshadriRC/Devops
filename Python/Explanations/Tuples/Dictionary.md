## Question 1

What is the output of the following snippet code:

testdict = {
  "brand": "Samsung",
  "ram": "3",
  "Os": "Android",
  "year": 2020
}

print(testdict.items())

dict_items([('brand', 'Samsung'), ('ram', '3'), ('Os', 'Android'), ('year', 2020)])


## Question 2

…. is one of the dictionary built-in methods that used to delete the last item from the dictionary.

popitem() is a built-in dictionary method that removes and returns the last inserted key-value pair as a tuple.

## Question 3

What is the output of the following snippet code:

testdict = {
  "brand": "apple",
  "ram": "3",
  "year": 2020,
  "year": 2021
}

print(testdict)

The output will be:

{'brand': 'apple', 'ram': '3', 'year': 2021}

Explanation:

In Python dictionaries, keys must be unique.

If a key is repeated (like "year" here), the last assigned value overwrites the previous one.

So "year": 2020 is replaced by "year": 2021.

## Question 3
