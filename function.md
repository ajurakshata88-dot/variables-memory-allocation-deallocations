# Functions in Python

## 1. Why Do We Use Functions?

Without a function, we might repeat the same code for every person:

```python
def add(a, b):
  return a + b


result = add(2, 3)
print(result)  # 5
```

A function lets us write the logic once and reuse it:

```python
def welcome(name):
  print("Welcome,", name)


welcome("Akshata")
welcome("Anu")
welcome("Riya")
```

Functions help with:

- Code reuse
- Less repetition
- Better organization
- Easier maintenance
- Easier testing

## 2. What Is a Function?

A function is a reusable block of code that performs a specific task.

```python
def add(a, b):
  return a + b


result = add(2, 3)
print(result)  # 5
```

## 3. Defining and Calling a Function

Defining a function describes what it should do. It does not run the function body:

```python
def greet():
  print("Hello")
```

Calling the function runs its body:

```python
greet()
```

## 4. A Function Without Parameters

A function does not need parameters if it does not need information from the caller:

```python
def welcome():
  print("Welcome to Nighan2 Labs")


welcome()
```

Start with the simplest function that solves the problem. Add parameters when the function needs input.

## 5. Functions With Parameters

A parameter is a name in the function definition. An argument is the value passed when calling the function.

```python
def welcome(name):
  print("Welcome,", name)


welcome("Akshata")
```

Here, `name` is a parameter and `"Akshata"` is an argument.

## 6. Multiple Parameters

A function can accept more than one parameter:

```python
def add(a, b):
  print(a + b)


add(10, 20)
```

## 7. `return`: Sending a Value Back

`print()` displays a value. `return` sends a value back to the caller so the program can use it.

```python
def add(a, b):
  return a + b


result = add(10, 20)
print(result)  # 30
```

## 8. What Happens After `return`?

`return` immediately ends that function call. Any statements after it in the same function are not executed:

```python
def test():
  return 10
  print("This will not run")


print(test())
```

## 9. Returning Multiple Values

Python can return multiple values. They are packaged as a tuple and can be unpacked into variables:

```python
def calculate(a, b):
  return a + b, a - b, a * b


total, difference, product = calculate(10, 5)
print(total)       # 15
print(difference)  # 5
print(product)     # 50
```

## 10. Default Parameters

A default parameter value is used when the caller does not provide an argument for that parameter.

```python
def greet(name="Akshata"):
  print("Hello,", name)


greet()          # Hello, Akshata
greet("Anu")     # Hello, Anu
```

Use a default when there is a sensible, commonly used value. It lets callers omit that argument when they want the default behavior.

## 11. Positional Arguments

Positional arguments are matched to parameters by their order:

```python
def student(name, age):
  print(name, age)


student("Akshata", 21)
```

The first argument goes to `name`; the second goes to `age`.

## 12. Keyword Arguments

Keyword arguments are matched by parameter name, so their order does not matter:

```python
def student(name, age):
  print(name, age)


student(age=21, name="Akshata")
```

## 13. Combining Positional and Keyword Arguments

You can use positional arguments first, then keyword arguments:

```python
def student(name, age, course):
  print(name, age, course)


student("Akshata", 21, course="BCA")
student(name="Akshata", age=21, course="BCA")
```

This is invalid because a positional argument cannot come after a keyword argument:

```python
# student(name="Akshata", 21, course="BCA")
```

## 14. `*args`: Variable Positional Arguments

Use `*args` when a function should accept any number of positional arguments. Inside the function, `args` is a tuple.

```python
def add(*numbers):
  total = 0
  for number in numbers:
    total += number
  return total


print(add(10, 20))
print(add(10, 20, 30))
print(add(1, 2, 3, 4, 5))
```

The name `args` is conventional; the `*` is what collects the extra positional arguments.

## 15. `**kwargs`: Variable Keyword Arguments

Use `**kwargs` when a function should accept any number of keyword arguments. Inside the function, `kwargs` is a dictionary.

```python
def student(**details):
  print(details)


student(name="Akshata", age=21, course="BCA")
```

## 16. Combining Parameters

A function can combine regular parameters, default values, `*args`, and `**kwargs` in this order:

```python
def example(a, b=10, *args, **kwargs):
  print(a)
  print(b)
  print(args)
  print(kwargs)


example(1, 2, 3, 4, mode="fast")
```

## 17. Scope: Local and Global Variables

A variable created inside a function is local to that function:

```python
def show_local():
  x = 10
  print(x)


show_local()
```

A function can read a global variable defined outside it:

```python
x = 100


def show_global():
  print(x)


show_global()
```

## 18. The `global` Keyword

Use `global` to assign to a global variable from inside a function:

```python
count = 0


def increment():
  global count
  count += 1


increment()
print(count)  # 1
```

Avoid global state when possible. Parameters and return values usually make functions easier to reuse and test.

## 19. Local Scope Stays Inside the Function

A local variable cannot be used outside the function where it was created:

```python
def test():
  x = 10


test()
print(x)  # NameError: x is not defined here
```

## 20. Functions Can Call Other Functions

Functions can be combined to organize a larger task into smaller steps:

```python
def add(a, b):
  return a + b


def display_result():
  result = add(10, 20)
  print(result)


display_result()
```

A larger program might follow a flow like this:

```text
main() -> validate() -> calculate() -> save() -> display()
```
## 21. Function-Calling Flow

When a function is called, Python matches the arguments to its parameters, runs the function body, and returns a value to the call site:

```python
def multiply(a, b):
  return a * b


result = multiply(5, 4)
print(result)  # 20
```

Python passes `5` to `a` and `4` to `b`, evaluates `a * b`, then assigns the returned value (`20`) to `result`.

## 22. Functions Are Objects

Functions are objects in Python. You can assign a function to another variable and call it through that variable:

```python
def greet():
  print("Hello")


say_hello = greet
say_hello()  # Hello
```

Writing `greet` refers to the function object; writing `greet()` calls it.

## 23. Passing a Function to Another Function

A function can be passed as an argument to another function. A function that accepts or returns another function is called a higher-order function.

```python
def square(x):
  return x * x


def process(function, value):
  return function(value)


print(process(square, 5))  # 25
```

Here, `process` receives `square` as a value and calls it with `5`.

## 24. Lambda Functions

A `lambda` expression creates a small anonymous function. It contains one expression and returns that expression's result:

```python
square = lambda x: x * x
print(square(5))  # 25
```

Lambdas are often useful for short operations passed to functions such as `map`:

```python
numbers = [1, 2, 3, 4]
result = list(map(lambda x: x * 2, numbers))
print(result)  # [2, 4, 6, 8]
```

## 25. Recursion

A recursive function calls itself. It needs a base case that stops the recursion:

```python
def countdown(n):
  if n == 0:
    return
  print(n)
  countdown(n - 1)


countdown(5)
```

The `n == 0` condition is the base case. Without a base case, the calls would continue until Python raises a recursion error.

## 26. Function Documentation

A docstring describes what a function does. Put it as the first statement in the function body:

```python
def add(a, b):
  """Return the sum of two numbers."""
  return a + b


print(add.__doc__)
```

Clear docstrings are a useful professional Python habit, especially for functions whose purpose or behavior is not obvious.

## 27. Type Hints

Type hints communicate the intended types of parameters and return values to readers and tools:

```python
def add(a: int, b: int) -> int:
  return a + b
```

Python generally does not enforce these annotations at runtime. Type checkers and editors can use them to help identify mistakes.

## 28. A Practical Program: Electricity Bill

This example charges 2 per unit for the first 100 units, 4 per unit for the next 100, and 6 per unit above 200. It also adds a fixed charge of 100:

```python
def calculate_bill(units):
  if units < 0:
    raise ValueError("Units cannot be negative")

  if units <= 100:
    amount = units * 2
  elif units <= 200:
    amount = 100 * 2 + (units - 100) * 4
  else:
    amount = 100 * 2 + 100 * 4 + (units - 200) * 6

  return amount + 100


units = int(input("Enter units: "))
bill = calculate_bill(units)
print("Bill:", bill)
```

Putting the calculation in `calculate_bill` separates the billing logic from input and output. This makes the calculation easier to read, test, reuse, and maintain.

## 29. Function Design

A well-designed function usually has clearly defined inputs, performs a focused piece of processing, and optionally returns an output. A clear name and a manageable amount of work make the function easier to understand and test.

## 30. Avoid Giant Functions

A function that handles input, validation, calculations, database work, and display all at once is difficult to understand and change. Split those jobs into focused functions instead:

```python
def get_student():
  pass


def validate_student(student):
  pass


def calculate_student_result(student):
  pass


def save_student(student):
  pass


def display_student(student):
  pass
```

Each function now has one main responsibility. This applies the single-responsibility principle at the function level, improving readability, testing, and maintenance without requiring a class.
 