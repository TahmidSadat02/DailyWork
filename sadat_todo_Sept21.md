# Sadat — 21 Sept 2026 Python Learning Todo List (Basic to Medium)

**Date:** 21 September 2026  
**Topic:** Python Programming — Foundations to Intermediate Engineering Mastery  
**Environment:** Python 3.10+ (macOS Terminal / IDE / Interactive REPL)  
**Learning Philosophy:** Learn by typing and running code — theory coupled with real-world backend and banking/HRMS data examples.

---

## 🧭 Study Schedule & Roadmap Overview

```mermaid
graph LR
    A[Part 1: Core Basics] --> B[Part 2: Control & Functions]
    B --> C[Part 3: Data Structures Mastery]
    C --> D[Part 4: Error Handling & Files]
    D --> E[Part 5: Medium / OOP]
    E --> F[Part 6: Intermediate Pythonic Patterns]
    F --> G[Part 7: Hands-on Mini Projects]
```

| Section | Level | Core Focus Areas | Estimated Time |
| :--- | :---: | :--- | :---: |
| **Part 1** | Basic | Variables, Dynamic Typing, Strings & Formatting, Math | 45 mins |
| **Part 2** | Basic | Conditionals, Loops (`for`, `while`), Functions & Scope | 60 mins |
| **Part 3** | Basic–Medium | Lists, Tuples, Sets, Dictionaries, Slicing | 75 mins |
| **Part 4** | Basic–Medium | Exception Handling (`try/except/finally`), File I/O, JSON | 60 mins |
| **Part 5** | Medium | OOP: Classes, Instances, Inheritance, `@property`, Dunder methods | 90 mins |
| **Part 6** | Medium | Comprehensions, Lambda, Generators (`yield`), Decorators | 75 mins |
| **Part 7** | Medium | Real-World Hands-On Challenges & Mini Project | 60 mins |

---

## 📘 PART 1: Python Fundamentals & Data Types (Basic)

- [ ] **1.1 Variables, Type Casting & Operators**
  - [ ] Understand dynamic typing: `int`, `float`, `str`, `bool`, `None`.
  - [ ] Test type conversions: `int("100")`, `str(45.5)`, `bool(0)`, `bool("hello")`.
  - [ ] Understand truthy vs falsy values (`0`, `""`, `[]`, `{}`, `None` are falsy).
  - [ ] Practice arithmetic and assignment operators: `+`, `-`, `*`, `/`, `//` (floor division), `%` (modulo), `**` (power).

- [ ] **1.2 Strings & Modern Text Formatting**
  - [ ] String indexing & slicing: `s[0]`, `s[-1]`, `s[1:5]`, `s[::-1]` (string reverse).
  - [ ] Essential string methods: `.strip()`, `.lower()`, `.upper()`, `.split()`, `.join()`, `.replace()`, `.startswith()`, `.endswith()`.
  - [ ] Master f-strings (formatted string literals):
    ```python
    name = "Sumaiya Islam"
    base = 38000
    print(f"Employee: {name.upper()} | Base: BDT {base:,.2f}")
    # Output: Employee: SUMAIYA ISLAM | Base: BDT 38,000.00
    ```

---

## 📘 PART 2: Control Flow & Functions (Basic)

- [ ] **2.1 Conditional Logic**
  - [ ] `if`, `elif`, `else` constructs with `and`, `or`, `not`.
  - [ ] Ternary operator / conditional expression:
    ```python
    status = "Confirmed" if score >= 3.5 else "Probation Extended"
    ```

- [ ] **2.2 Loops & Iteration**
  - [ ] `for` loops with `range(start, stop, step)`.
  - [ ] Loop helper `enumerate()` for index tracking:
    ```python
    employees = ["Alex", "Nusrat", "Tanvir"]
    for idx, emp in enumerate(employees, start=1):
        print(f"#{idx}: {emp}")
    ```
  - [ ] `zip()` for parallel iteration over multiple collections.
  - [ ] `while` loops with `break`, `continue`, and loop `else` clauses.

- [ ] **2.3 Functions & Scope**
  - [ ] Defining functions with `def`, return values, and docstrings.
  - [ ] Default argument values and keyword arguments.
  - [ ] Variable argument unpacking: `*args` (positional tuple) and `**kwargs` (keyword dict):
    ```python
    def create_salary_slip(employee_id, **allowances):
        gross = sum(allowances.values())
        return {"emp": employee_id, "gross": gross, "items": allowances}
    ```
  - [ ] Scope rules: Local vs Global, and `global` / `nonlocal` keywords.

---

## 📙 PART 3: Built-In Collections & Data Structures (Basic–Medium)

- [ ] **3.1 Lists (Ordered, Mutable)**
  - [ ] Common methods: `.append()`, `.extend()`, `.insert()`, `.pop()`, `.remove()`, `.sort()`.
  - [ ] List slicing tricks: copying a list via `items[:]` or `.copy()`.
  - [ ] Shallow copy vs deep copy (`import copy; copy.deepcopy()`).

- [ ] **3.2 Dictionaries (Key-Value, Mutable, Fast O(1) Lookups)**
  - [ ] Accessing keys safely: `.get(key, default)` vs `dict[key]` (avoiding `KeyError`).
  - [ ] Iterating dicts: `.keys()`, `.values()`, `.items()`.
  - [ ] Useful dict methods: `.update()`, `.pop()`, `.setdefault()`.
  - [ ] Nested dictionary navigation (common in JSON/API payloads).

- [ ] **3.3 Tuples & Sets**
  - [ ] **Tuples:** Immutable sequences `(1, 2, 3)`, tuple packing/unpacking `x, y = y, x`.
  - [ ] **Sets:** Unordered unique collections `set([1, 2, 2, 3]) -> {1, 2, 3}`.
  - [ ] Set operations: union `|`, intersection `&`, difference `-`, symmetric difference `^`.

---

## 📙 PART 4: Error Handling & File Operations (Basic–Medium)

- [ ] **4.1 Exception Handling Architecture**
  - [ ] `try`, `except`, `else`, `finally` block execution flow:
    ```python
    try:
        daily_rate = base_salary / working_days
    except ZeroDivisionError as err:
        print(f"Error: Working days cannot be zero! Details: {err}")
    except TypeError:
        print("Error: Salary and days must be numeric.")
    else:
        print(f"Daily rate: {daily_rate}")
    finally:
        print("Calculation complete.")
    ```
  - [ ] Raising exceptions with `raise ValueError(...)`.
  - [ ] Writing custom exception classes inheriting from `Exception`.

- [ ] **4.2 File I/O & Context Managers**
  - [ ] The `with open(..., mode)` pattern (automatic file closure).
  - [ ] Reading and writing text files (`.read()`, `.readline()`, `.readlines()`, `.write()`).
  - [ ] Working with `json` module: `json.loads()`, `json.dumps()`, `json.load()`, `json.dump()`.
  - [ ] Path manipulation using modern `pathlib.Path`:
    ```python
    from pathlib import Path
    data_dir = Path("data")
    data_dir.mkdir(exist_ok=True)
    file_path = data_dir / "report.json"
    ```

---

## 🚀 PART 5: Object-Oriented Programming (OOP) (Medium)

- [ ] **5.1 Classes, Instances & Constructors**
  - [ ] Defining a class with `class Employee:` and constructor `def __init__(self, ...)`.
  - [ ] Instance attributes vs Class attributes.
  - [ ] Special dunder (double-underscore) methods: `__str__` (user readable) and `__repr__` (debugging).

- [ ] **5.2 Encapsulation & Properties**
  - [ ] Private/protected naming convention (`_protected`, `__private`).
  - [ ] Using the `@property` decorator for getters and `@<field>.setter` for validation:
    ```python
    class BankAccount:
        def __init__(self, account_no, initial_balance=0.0):
            self.account_no = account_no
            self._balance = initial_balance

        @property
        def balance(self):
            return self._balance

        @balance.setter
        def balance(self, value):
            if value < 0:
                raise ValueError("Balance cannot be negative.")
            self._balance = value
    ```

- [ ] **5.3 Inheritance & Polymorphism**
  - [ ] Subclassing and using `super().__init__(...)`.
  - [ ] Method overriding and polymorphic dispatch.
  - [ ] Check types using `isinstance(obj, ClassName)` and `issubclass(Sub, Base)`.

- [ ] **5.4 Modern Dataclasses (Python 3.7+)**
  - [ ] Using `@dataclass` from `dataclasses` module for clean boilerplate-free models:
    ```python
    from dataclasses import dataclass

    @dataclass
    class SalaryComponent:
        name: str
        component_type: str  # Earning or Deduction
        amount: float
        is_taxable: bool = True
    ```

---

## 🚀 PART 6: Idiomatic Python & Intermediate Patterns (Medium)

- [ ] **6.1 Comprehensions (Write Clean, Pythonic Code)**
  - [ ] **List Comprehensions:** `[x * 2 for x in nums if x > 0]`
  - [ ] **Dict Comprehensions:** `{emp["name"]: emp["salary"] for emp in data}`
  - [ ] **Set Comprehensions:** `{emp["department"] for emp in employees}`

- [ ] **6.2 Functional Helpers & Lambdas**
  - [ ] Lambda functions: `square = lambda x: x ** 2`.
  - [ ] Custom sorting with `key` functions:
    ```python
    staff = [("Sumaiya", 41040), ("Tanvir", 35000), ("Rubina", 38000)]
    sorted_staff = sorted(staff, key=lambda s: s[1], reverse=True)
    ```
  - [ ] Using `map()` and `filter()`.

- [ ] **6.3 Generators & Iterators (`yield`)**
  - [ ] Difference between returning a list (in-memory) vs streaming with `yield` (memory-efficient).
  - [ ] Writing a generator function:
    ```python
    def batch_payroll_generator(employees, batch_size=5):
        for i in range(0, len(employees), batch_size):
            yield employees[i : i + batch_size]
    ```
  - [ ] Generator expressions: `(x * 2 for x in range(1000000))`.

- [ ] **6.4 Function Decorators**
  - [ ] Understanding functions as first-class citizens (passing functions into functions).
  - [ ] Writing a reusable execution timing / logging decorator:
    ```python
    import time
    from functools import wraps

    def log_execution(func):
        @wraps(func)
        def wrapper(*args, **kwargs):
            start = time.time()
            result = func(*args, **kwargs)
            duration = (time.time() - start) * 1000
            print(f"⏱️ [{func.__name__}] executed in {duration:.2f} ms")
            return result
        return wrapper
    ```

- [ ] **6.5 Type Hinting & Annotations**
  - [ ] Adding typing annotations: `def calculate_tax(salary: float, tax_rate: float = 0.10) -> float:`.
  - [ ] Using `typing` module: `List[str]`, `Dict[str, Any]`, `Optional[int]`, `Union[int, float]`.

---

## 🛠️ PART 7: Hands-On Mini Projects & Exercises

- [ ] **Exercise 1 (Basic Data Processing): Bank Salary Slip Parser**
  - [ ] Write a script that takes a raw employee dict, calculates HRA (40%), PF (10%), Net Pay, and formats a printed payslip.
- [ ] **Exercise 2 (Medium OOP): Prime Bank Account & Loan Amortisation Engine**
  - [ ] Build a `BankAccount` class and a `StaffLoan` subclass that computes monthly EMI installment over `N` months with balance tracking.
- [ ] **Exercise 3 (Intermediate Decorator & File I/O): Audit Logger**
  - [ ] Create a `@audit_trail` decorator that automatically appends user action, function name, timestamp, and arguments into an `audit.log` file.

---

## 📝 Practice Verification Checklist

To verify you have grasped both Basic and Medium concepts, make sure you can answer or write without looking up syntax:
- [ ] Can you explain the difference between `list.sort()` and `sorted(list)`?
- [ ] Can you explain why mutable objects (like `[]` or `{}`) should never be used as default parameter values?
- [ ] Can you explain the difference between a shallow copy and deep copy?
- [ ] Can you explain how `@property` works under the hood?
- [ ] Can you explain when to use a generator (`yield`) instead of a list?
