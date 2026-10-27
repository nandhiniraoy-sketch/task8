# Task 8: Exception Handling for Data Programs

**Track:** Data Science\
**Level:** 1 · Day 8

## 1. Objective

Learn how to handle common Python errors using `try`, `except`, `else`,
and `finally` while processing data. The goal is to make data-processing
programs more reliable and resistant to invalid input.

## 2. Requirements

-   Demonstrate common Python exceptions.
-   Write a program using `try` and `except`.
-   Build a data-conversion program that safely handles invalid values.
-   Practice handling `ValueError` and `TypeError`.
-   Keep exception handling specific.
-   Use `finally` when cleanup is required.

## 3. Common Python exceptions

``` python
# ValueError: the value has the wrong format
int("hello")

# TypeError: an operation is used with an incompatible type
"10" + 5

# ZeroDivisionError: division by zero
10 / 0

# IndexError: a list index does not exist
numbers = [10, 20]
print(numbers[5])

# KeyError: a dictionary key does not exist
customer = {"name": "Asha"}
print(customer["age"])
```

Each statement above raises an exception. Run them one at a time to see
the error messages.

## 4. Example: try, except, else, and finally

``` python
def divide_numbers(a, b):
    try:
        result = a / b
    except ZeroDivisionError:
        print("Error: the divisor cannot be zero.")
    except TypeError:
        print("Error: both inputs must be numbers.")
    else:
        print("Result:", result)
    finally:
        print("Division attempt finished.")


divide_numbers(10, 2)
divide_numbers(10, 0)
divide_numbers("10", 2)
```

-   `try`: contains code that might raise an exception.
-   `except`: handles a particular exception.
-   `else`: runs only when no exception occurred in the `try` block.
-   `finally`: runs whether or not an exception occurred.

## 5. Example: safely convert data values

This program converts a list of values to floating-point numbers.
Invalid values are reported and skipped rather than stopping the whole
program.

``` python
raw_values = ["12.5", "8", "invalid", None, "3.75", "-2"]

valid_values = []
invalid_values = []

for item in raw_values:
    try:
        number = float(item)
    except (ValueError, TypeError) as error:
        print(f"Skipping {item!r}: {error}")
        invalid_values.append(item)
    else:
        valid_values.append(number)

print("Valid values:", valid_values)
print("Invalid values:", invalid_values)

if valid_values:
    average = sum(valid_values) / len(valid_values)
    print(f"Average of valid values: {average:.2f}")
else:
    print("No valid numeric values were provided.")
```

### Sample output

``` text
Skipping 'invalid': could not convert string to float: 'invalid'
Skipping None: float() argument must be a string or a real number, not 'NoneType'
Valid values: [12.5, 8.0, 3.75, -2.0]
Invalid values: ['invalid', None]
Average of valid values: 5.56
```

*Note:* Exact error wording can vary slightly between Python versions.

## 6. Example: data conversion with user input

``` python
def get_number(prompt):
    while True:
        raw_input = input(prompt)

        try:
            return float(raw_input)
        except ValueError:
            print("Please enter a valid number, such as 12 or 3.5.")


first = get_number("Enter the first number: ")
second = get_number("Enter the second number: ")

print("Total:", first + second)
```

This example catches invalid numeric text and asks again instead of
terminating.

## 7. Example: process customer records safely

``` python
customers = [
    {"name": "Asha", "age": "22"},
    {"name": "Ravi", "age": "unknown"},
    {"name": "Mina", "age": "19"},
    {"name": "Dev"},  # age is missing
]

valid_customers = []

for customer in customers:
    name = customer.get("name", "Unknown")
    raw_age = customer.get("age")

    try:
        age = int(raw_age)
    except (ValueError, TypeError):
        print(f"Skipping {name}: age is missing or invalid.")
        continue

    valid_customers.append({"name": name, "age": age})

print("Valid customer records:")
for customer in valid_customers:
    print(customer)
```

Using `.get()` handles a missing dictionary key without raising
`KeyError`. The conversion is still protected because the value may be
absent or invalid.

## 8. Best practices

1.  **Catch specific exceptions.** Prefer `ValueError` or `TypeError`
    over a bare `except:`.
2.  **Keep the `try` block small.** This makes it easier to identify
    which operation failed.
3.  **Do not hide errors silently.** Log, report, skip, or correct
    invalid data intentionally.
4.  **Validate results.** Check for empty lists before calculating
    averages.
5.  **Use `finally` for cleanup.** It is useful for closing resources;
    context managers such as `with open(...)` are often even better for
    files.
6.  **Avoid using exceptions for ordinary control flow** when a simple
    condition is clearer.

## 9. Practice exercises

1.  Ask the user to enter an integer and handle non-integer input.
2.  Convert `["10", "20", "abc", "30", None]` into numbers, recording
    invalid values.
3.  Safely calculate an average when the input list may be empty.
4.  Write a function that divides two values and handles zero division
    and incompatible types.
5.  Process a list of student records, skipping records with missing or
    invalid marks.

## 10. Run the examples

1.  Install Python 3 if it is not already installed.

2.  Save an example in a file such as `exception_handling.py`.

3.  Run it from a terminal:

    ``` bash
    python exception_handling.py
    ```

4.  Alternatively, create a Jupyter Notebook and place each example in a
    separate cell.

## 11. Expected deliverables

-   Examples demonstrating common Python exceptions.
-   A program demonstrating `try`, `except`, `else`, and `finally`.
-   A data-conversion program that handles invalid values safely.
-   Practice exercises or notes explaining the exceptions handled.

------------------------------------------------------------------------

**Task completed when:** the programs run, invalid values do not
unexpectedly terminate the data-processing workflow, and each handled
exception has a clear response.
