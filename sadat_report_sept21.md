# Sadat — 21 Sept 2026 Python Learning & Practice Report (Basic to Medium)

**Date:** 21 September 2026  
**Subject:** Python Programming Mastery — Core Foundations, OOP & Intermediate Engineering Patterns  
**Environment:** Python 3.10+ (macOS / IDE / Terminal REPL)  
**Reference Todo:** [`sadat_todo_Sept21.md`](sadat_todo_Sept21.md)  
**Status:** ✅ **100% Completed & Verified**

---

## Executive Summary

Today's session was dedicated to intensive mastery of **Python from foundational basics up to intermediate (medium) engineering patterns**. The curriculum bridged theoretical syntax with production-grade backend implementation—specifically focusing on data structures, defensive error handling, Object-Oriented Programming (OOP), memory-efficient generators, type annotations, and function decorators.

All 7 core modules from `sadat_todo_Sept21.md` were studied, practiced, and verified with executable code samples reflecting real-world banking and HRMS backend logic.

| Module | Core Domain | Key Concepts Mastered | Assessment Status |
| :---: | :--- | :--- | :---: |
| **Part 1** | **Python Fundamentals** | Dynamic typing, truthy/falsy evaluation, string slicing, modern f-string formatters | ✅ Mastered |
| **Part 2** | **Control Flow & Functions** | Ternary expressions, `enumerate()`, `zip()`, `*args`, `**kwargs`, variable scope | ✅ Mastered |
| **Part 3** | **Data Collections** | Lists, Dicts (O(1) lookups), Sets, Tuples, shallow vs deep copying | ✅ Mastered |
| **Part 4** | **Error Handling & Files** | `try/except/else/finally`, custom exceptions, `with open()`, `json`, `pathlib.Path` | ✅ Mastered |
| **Part 5** | **Object-Oriented Programming** | Classes, instances, inheritance, `@property` getters/setters, `@dataclass` | ✅ Mastered |
| **Part 6** | **Intermediate Patterns** | Comprehensions, lambda & custom sorting, generators (`yield`), decorators (`@wraps`) | ✅ Mastered |
| **Part 7** | **Hands-On Mini Projects** | Salary slip calculator, loan amortisation class, audit logging decorator | ✅ Implemented |

---

## Detailed Topic Breakdown & Verified Implementations

### 📘 Part 1: Fundamentals, Types & String Formatting
1. **Dynamic Typing & Truthiness:**
   - Practiced type casting: `int("100")`, `float("45.5")`, `bool([]) -> False`, `bool({"a": 1}) -> True`.
   - Verified that empty containers (`""`, `[]`, `{}`, `set()`), `0`, `0.0`, and `None` evaluate to `False` in conditional statements.
2. **String Manipulation & Slicing:**
   - Reversing strings via step slicing: `"PrimeBank"[::-1] -> "knaBemirP"`.
   - Methods: `.strip()`, `.lower()`, `.upper()`, `.split(",")`, `", ".join(list)`.
3. **Advanced F-Strings (Formatted String Literals):**
   - Currency and comma separators: `f"{38000.5:,.2f}" -> "38,000.50"`.
   - Percentage formatting: `f"{0.08:.1%} -> "8.0%"`.
   - Alignment and padding: `f"{'Basic Pay':<15}: BDT {25000:>10,}"`.

---

### 📘 Part 2: Control Flow & Flexible Functions
1. **Idiomatic Branching & Ternary Operators:**
   - Clean conditional assignment: `decision = "Approved" if rating >= 3.0 else "Needs Review"`.
2. **Modern Iteration Idioms:**
   - Using `enumerate(iterable, start=1)` instead of maintaining manual index counters.
   - Using `zip(names, salaries)` for parallel iteration across matching lists without indexing errors.
3. **Variable Argument Handling (`*args` & `**kwargs`):**
   - `*args`: Collects arbitrary positional arguments into a tuple.
   - `**kwargs`: Collects arbitrary keyword arguments into a dictionary.
   ```python
   def compute_gross_salary(base: float, *bonuses, **deductions) -> float:
       total_earnings = base + sum(bonuses)
       total_deductions = sum(deductions.values())
       return total_earnings - total_deductions

   net = compute_gross_salary(38000, 4500, 2000, pf=3800, tax=1500)
   # net = (38000 + 4500 + 2000) - (3800 + 1500) = 39,200
   ```

---

### 📙 Part 3: Collections & Data Structures
1. **Lists vs Tuples:**
   - **Lists:** Ordered, mutable, dynamic arrays (`.append()`, `.extend()`, `.pop()`, `.sort()`).
   - **Tuples:** Ordered, immutable sequences, hashable, used for fixed records and dict keys.
2. **Dictionaries (Hash Maps):**
   - Practiced safe retrieval via `dict.get(key, default)` preventing runtime `KeyError`.
   - Dict iteration: `.items()` yielding `(key, value)` pairs.
3. **Sets (Hash Sets):**
   - Automatic deduplication and fast $O(1)$ membership testing (`if item in set_data:`).
   - Set algebra: union (`|`), intersection (`&`), difference (`-`), symmetric difference (`^`).
4. **Shallow vs Deep Copy:**
   - Shallow copy (`list.copy()`, `copy.copy()`) copies top-level references; nested mutable structures remain linked.
   - Deep copy (`copy.deepcopy()`) recursively duplicates all nested objects, preventing side effects.

---

### 📙 Part 4: Robust Error Handling & Modern File I/O
1. **The Full `try / except / else / finally` Structure:**
   - `try`: Code that might raise an exception.
   - `except ExceptionType as err`: Handles specific error types gracefully.
   - `else`: Runs strictly when NO exception occurred in `try`.
   - `finally`: Always runs (e.g. resource cleanup, closing connections).
2. **Custom Domain Exceptions:**
   ```python
   class PolicyLimitExceededError(Exception):
       """Raised when an increment or allowance breaches bank policy."""
       def __init__(self, requested_pct: float, max_allowed_pct: float):
           super().__init__(f"Requested {requested_pct}% exceeds policy cap of {max_allowed_pct}%")
           self.requested = requested_pct
           self.limit = max_allowed_pct
   ```
3. **Context Managers (`with open(...)`) & JSON:**
   - Safe file handling using `with` to guarantee file descriptor closure.
   - Working with `pathlib.Path` for cross-platform file system navigation.
   - `json.dumps(obj, indent=2)` and `json.loads(str_data)`.

---

### 🚀 Part 5: Object-Oriented Programming (OOP) & Dataclasses
1. **Encapsulation & `@property` Pattern:**
   - Protecting internal state with private attributes (`_balance`).
   - Validating values on assignment through setter methods without breaking public dot syntax.
   ```python
   class BankAccount:
       def __init__(self, account_no: str, initial_balance: float = 0.0):
           self.account_no = account_no
           self._balance = initial_balance

       @property
       def balance(self) -> float:
           return self._balance

       @balance.setter
       def balance(self, amount: float):
           if amount < 0:
               raise ValueError("Account balance cannot be negative!")
           self._balance = amount

       def __str__(self) -> str:
           return f"Account [{self.account_no}] - Balance: BDT {self._balance:,.2f}"

       def __repr__(self) -> str:
           return f"BankAccount(account_no='{self.account_no}', initial_balance={self._balance})"
   ```
2. **Inheritance & Polymorphism:**
   - Subclassed `StaffLoanAccount` from `BankAccount` using `super().__init__()`.
   - Implemented interest calculation and monthly repayment deductions.
3. **Modern Dataclasses (`@dataclass`):**
   - Eliminated verbose `__init__`, `__repr__`, and `__eq__` boilerplate.
   ```python
   from dataclasses import dataclass

   @dataclass
   class EmployeeRecord:
       employee_id: str
       full_name: str
       grade: str
       base_salary: float
       is_active: bool = True
   ```

---

### 🚀 Part 6: Idiomatic Python & Intermediate Patterns
1. **Comprehensions:**
   - List comprehension: `[emp.upper() for emp in employees if emp.startswith("H")]`
   - Dict comprehension: `{emp.employee_id: emp.base_salary for emp in staff}`
   - Set comprehension: `{emp.grade for emp in staff}`
2. **Generators & `yield` (Memory Efficiency):**
   - Compared memory footprints: a 1,000,000-item list consumes ~8MB RAM, while a generator pipeline consumes ~128 bytes by yielding elements on demand.
   ```python
   def payroll_batch_streamer(employee_list, batch_size=5):
       for i in range(0, len(employee_list), batch_size):
           yield employee_list[i : i + batch_size]
   ```
3. **Function Decorators (`@wraps`):**
   - Built a reusable timing and logging decorator that wraps any function, measures execution duration, and preserves function docstrings and metadata.
   ```python
   import time
   from functools import wraps

   def audit_performance(func):
       @wraps(func)
       def wrapper(*args, **kwargs):
           t0 = time.perf_counter()
           res = func(*args, **kwargs)
           elapsed = (time.perf_counter() - t0) * 1000
           print(f"⚡ [AUDIT] {func.__name__} finished in {elapsed:.3f} ms")
           return res
       return wrapper
   ```
4. **Type Hints & Static Typing:**
   - Used `typing.Optional`, `typing.List`, `typing.Dict`, `typing.Tuple`, `typing.Union`.

---

## 🛠️ Part 7: Hands-On Code Implementations

### Exercise 1: Salary Slip Computation & Tax Deduction Engine
```python
def generate_salary_slip(employee: dict) -> dict:
    base = employee.get("base", 0.0)
    hra = base * 0.40  # 40% House Rent Allowance
    medical = 3000.0   # Fixed Medical Allowance
    festival_bonus = employee.get("bonus", 0.0)
    gross = base + hra + medical + festival_bonus

    pf_deduction = base * 0.10  # 10% Provident Fund
    welfare = 500.0             # Fixed Welfare Fund
    # Simplified slab-based tax calculation
    taxable_monthly = gross - medical  # Medical exempt up to limit
    tax_withholding = taxable_monthly * 0.05 if taxable_monthly > 30000 else 0.0
    total_deductions = pf_deduction + welfare + tax_withholding

    net_pay = gross - total_deductions

    return {
        "employee_id": employee.get("id"),
        "name": employee.get("name"),
        "earnings": {"Basic": base, "HRA": hra, "Medical": medical, "Bonus": festival_bonus},
        "gross": gross,
        "deductions": {"PF": pf_deduction, "Welfare": welfare, "Tax": tax_withholding},
        "total_deductions": total_deductions,
        "net_pay": net_pay
    }
```

### Exercise 2: Staff Loan EMI Calculator with Balance Amortisation
```python
class StaffLoan:
    def __init__(self, loan_id: str, principal: float, annual_rate: float, tenure_months: int):
        self.loan_id = loan_id
        self.principal = principal
        self.monthly_rate = (annual_rate / 100) / 12
        self.tenure_months = tenure_months
        self.remaining_balance = principal

    def calculate_emi(self) -> float:
        r = self.monthly_rate
        n = self.tenure_months
        if r == 0:
            return self.principal / n
        emi = (self.principal * r * (1 + r)**n) / ((1 + r)**n - 1)
        return round(emi, 2)

    def apply_monthly_payment(self) -> float:
        emi = self.calculate_emi()
        interest = self.remaining_balance * self.monthly_rate
        principal_repaid = emi - interest
        self.remaining_balance = max(0.0, self.remaining_balance - principal_repaid)
        return emi
```

---

## 💡 Self-Assessment Q&A Verification

| Question | Core Engineering Answer |
| :--- | :--- |
| **`list.sort()` vs `sorted(list)`?** | `list.sort()` sorts in-place, returns `None`, and modifies the original list. `sorted(list)` returns a new sorted list, leaving the original unchanged. |
| **Why avoid mutable default arguments (`def f(x=[])`)?** | Default arguments are evaluated once when the function definition is loaded, not on each call. A mutable object is shared across all function invocations, creating unintended state leaks. Use `def f(x=None): if x is None: x = []`. |
| **Shallow vs Deep copy difference?** | A shallow copy constructs a new compound object and inserts references to the original child objects. A deep copy recursively duplicates all child objects, completely isolating the new copy. |
| **How does `@property` work?** | It implements Python's descriptor protocol (`__get__`, `__set__`), translating attribute lookup (`obj.attr`) into a method call, enabling computed properties and validation on attribute access. |
| **When to use a generator (`yield`)?** | When processing large datasets, unbounded streams, or reading line-by-line files where holding the entire collection in memory at once would cause high RAM usage or Out-Of-Memory (OOM) errors. |

---

## Conclusion & Next Steps
- Today's learning established solid foundations across Python data structures, error handling, OOP principles, and clean Pythonic idioms.
- The demonstrated patterns directly empower writing robust Frappe DocType controllers, whitelisted API endpoints (`@frappe.whitelist()`), automated query reports, and Playwright automation scripts.
- Both [`sadat_todo_Sept21.md`](sadat_todo_Sept21.md) and this report [`sadat_report_sept21.md`](sadat_report_sept21.md) are synchronized with the DailyWork repository.
