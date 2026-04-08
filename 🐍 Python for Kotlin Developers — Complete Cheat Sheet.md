# 🐍 Python for Kotlin Developers — Complete Cheat Sheet

> **Goal:** Transition from Kotlin mindset → Python thinking, fast.
> **Format:** Concept → Kotlin → Python → Key Differences → Pro Tips

---

## 🧠 Mental Model First: Core Mindset Shifts

| Kotlin Thinking | Python Thinking |
|---|---|
| Everything has a type (static) | Types are inferred at runtime (dynamic) |
| Null safety is enforced by compiler | `None` checks are your responsibility |
| Semicolons optional, braces required | Indentation IS the syntax |
| `data class` for POJOs | `@dataclass` or plain `dict` |
| Coroutines for async | `async/await` with `asyncio` |
| `val`/`var` for immutability | No true immutability for variables |
| Compile-time errors save you | Tests save you |
| Explicit is default | Dynamic/flexible is default |

---

## 1. Variables & Data Types

### Kotlin
```kotlin
val name: String = "Alice"       // immutable
var age: Int = 30                 // mutable
val pi = 3.14                     // type inferred
val isActive: Boolean = true
val bigNum: Long = 100_000L
```

### Python
```python
name: str = "Alice"    # type hint (optional, not enforced)
age: int = 30
pi = 3.14              # no val/var distinction
is_active = True       # capitalized booleans!
big_num = 100_000      # underscores work here too
```

### Key Differences
- Python has **no `val`/`var`** — all variables are reassignable by default
- Type hints (`name: str`) are **optional and not enforced** at runtime
- Python uses `snake_case`, Kotlin uses `camelCase`
- Python booleans: `True`/`False` (capital T/F); Kotlin: `true`/`false`
- Python has **no primitives** — everything is an object
- Python integers have **unlimited precision** (no Int/Long distinction needed)

### ⚠️ Common Mistakes
```python
# Kotlin dev mistake: using camelCase
myVariable = 10    # ❌ not Pythonic
my_variable = 10   # ✅

# Forgetting True/False capitalization
is_done = true     # ❌ NameError!
is_done = True     # ✅
```

### 💡 Pro Tips
```python
# Multiple assignment (like destructuring)
x, y, z = 1, 2, 3

# Swap without temp variable
x, y = y, x

# Type checking at runtime
type(age)           # <class 'int'>
isinstance(age, int) # True
```

---

## 2. Null Safety vs None Handling

### Kotlin
```kotlin
// Nullable types enforced by compiler
var name: String? = null
val length = name?.length ?: 0        // safe call + Elvis
val upper = name!!.uppercase()         // throws NPE if null (dangerous)

fun greet(name: String?) {
    println(name ?: "Guest")
}
```

### Python
```python
# None is just a regular value — no compiler safety
name: str | None = None              # type hint only, not enforced

length = len(name) if name else 0    # conditional expression (Elvis equivalent)
upper = name.upper() if name else "" # manual None check

def greet(name=None):
    print(name or "Guest")           # 'or' acts like Elvis operator
```

### Key Differences

| Kotlin | Python |
|--------|--------|
| `String?` — compiler enforced | `str \| None` — hint only |
| `?.` safe call operator | `if x is not None:` check |
| `?:` Elvis operator | `x or default` / `x if x else default` |
| `!!` force-unwrap | Just access it (crash = `AttributeError`) |
| NPE at compile time | `AttributeError`/`TypeError` at runtime |

### ⚠️ Common Mistakes
```python
# Kotlin devs forget to check None
name = None
print(name.upper())   # ❌ AttributeError: 'NoneType' has no attribute 'upper'

# 'or' has a gotcha with falsy values
count = 0
result = count or 10   # ❌ returns 10! (0 is falsy)
result = count if count is not None else 10  # ✅ correct

# is vs ==
name = None
if name == None:    # ❌ works but wrong style
if name is None:    # ✅ Pythonic
```

### 💡 Pro Tips
```python
# Walrus operator (Python 3.8+) — like Kotlin's let {}
if (value := expensive_operation()) is not None:
    process(value)

# Safe dict access (like ?.get())
data = {"key": "value"}
result = data.get("missing_key", "default")  # no KeyError
```

---

## 3. Conditionals (if / when)

### Kotlin
```kotlin
// Standard if-else
val grade = if (score >= 90) "A" else if (score >= 80) "B" else "C"

// when (pattern matching)
when (status) {
    "active" -> println("Running")
    "idle"   -> println("Waiting")
    else     -> println("Unknown")
}

// when with ranges
when (age) {
    in 0..12  -> "Child"
    in 13..17 -> "Teen"
    else      -> "Adult"
}
```

### Python
```python
# Ternary (inline if-else)
grade = "A" if score >= 90 else "B" if score >= 80 else "C"

# match statement (Python 3.10+ equivalent of when)
match status:
    case "active":
        print("Running")
    case "idle":
        print("Waiting")
    case _:
        print("Unknown")

# Range check (no 'in range' needed for simple comparisons)
if 0 <= age <= 12:
    category = "Child"
elif 13 <= age <= 17:
    category = "Teen"
else:
    category = "Adult"
```

### Key Differences
- Python's `match` (3.10+) ≈ Kotlin's `when`, but older code uses `if/elif/else`
- Python ternary: `value_if_true if condition else value_if_false` (order is different!)
- Python has no `when` for older versions — use `if/elif/else` chains

### 💡 Pro Tips
```python
# match with structural patterns (Python 3.10+)
match point:
    case (0, 0):
        print("Origin")
    case (x, 0):
        print(f"X-axis at {x}")
    case (x, y):
        print(f"Point at {x}, {y}")

# dict as switch (classic Python pattern for older versions)
actions = {
    "start": lambda: print("Starting"),
    "stop":  lambda: print("Stopping"),
}
actions.get(command, lambda: print("Unknown"))()
```

---

## 4. Loops (for, while)

### Kotlin
```kotlin
// for over range
for (i in 1..5) println(i)
for (i in 1 until 5) println(i)   // excludes 5
for (i in 5 downTo 1 step 2) println(i)

// for over list
val items = listOf("a", "b", "c")
for (item in items) println(item)
for ((index, item) in items.withIndex()) println("$index: $item")

// while
var x = 0
while (x < 5) { x++ }

// forEach (functional)
items.forEach { println(it) }
```

### Python
```python
# for over range
for i in range(1, 6):       print(i)     # 1..5
for i in range(1, 5):       print(i)     # 1 until 5
for i in range(5, 0, -2):   print(i)     # downTo step

# for over list
items = ["a", "b", "c"]
for item in items:
    print(item)

# enumerate = withIndex()
for index, item in enumerate(items):
    print(f"{index}: {item}")

# while
x = 0
while x < 5:
    x += 1

# forEach equivalent
[print(item) for item in items]   # list comprehension (or just use for loop)
```

### Key Differences

| Kotlin | Python |
|--------|--------|
| `1..5` (inclusive) | `range(1, 6)` (end exclusive) |
| `1 until 5` | `range(1, 5)` |
| `downTo` / `step` | `range(start, stop, step)` |
| `withIndex()` | `enumerate()` |
| `break`/`continue` | same |
| `forEach { }` | `for` loop or list comprehension |

### 💡 Pro Tips
```python
# zip two lists (like Kotlin's zip())
for a, b in zip(list1, list2):
    print(a, b)

# Loop with else (runs if loop completes without break)
for i in range(5):
    if i == 10:
        break
else:
    print("No break occurred")  # This runs

# List comprehension (Pythonic loop)
squares = [x**2 for x in range(10)]
evens   = [x for x in range(20) if x % 2 == 0]
```

---

## 5. Functions

### Kotlin
```kotlin
// Basic
fun greet(name: String): String = "Hello, $name"

// Default arguments
fun connect(host: String = "localhost", port: Int = 8080): String {
    return "$host:$port"
}

// Named arguments
connect(port = 9090, host = "example.com")

// Varargs
fun sum(vararg nums: Int) = nums.sum()

// Lambda
val multiply = { a: Int, b: Int -> a * b }

// Higher-order function
fun applyOp(x: Int, op: (Int) -> Int) = op(x)
applyOp(5) { it * 2 }

// Extension function
fun String.shout() = uppercase() + "!!!"
```

### Python
```python
# Basic
def greet(name: str) -> str:
    return f"Hello, {name}"

# Default arguments
def connect(host: str = "localhost", port: int = 8080) -> str:
    return f"{host}:{port}"

# Named arguments (keyword args)
connect(port=9090, host="example.com")

# *args (varargs)
def sum_all(*nums):
    return sum(nums)

# Lambda
multiply = lambda a, b: a * b

# Higher-order function
def apply_op(x, op):
    return op(x)
apply_op(5, lambda x: x * 2)

# No extension functions — use standalone functions or monkey-patching
def shout(s: str) -> str:
    return s.upper() + "!!!"
```

### Key Differences

| Kotlin | Python |
|--------|--------|
| `fun` keyword | `def` keyword |
| Return type after `:` | Return type hint with `->` (optional) |
| `vararg` | `*args` |
| `{ it * 2 }` lambda | `lambda x: x * 2` |
| Extension functions | No native equivalent |
| `**kwargs` | Named dict of keyword args |
| Single-expression `fun f() = expr` | No equivalent, need `return` |

### ⚠️ Common Mistakes
```python
# Mutable default arguments — a classic Python gotcha!
def add_item(item, lst=[]):   # ❌ list is shared across calls!
    lst.append(item)
    return lst

def add_item(item, lst=None): # ✅
    if lst is None:
        lst = []
    lst.append(item)
    return lst
```

### 💡 Pro Tips
```python
# **kwargs = named parameters as dict
def configure(**kwargs):
    host = kwargs.get("host", "localhost")
    port = kwargs.get("port", 8080)

# Decorators (like annotations + aspect-oriented)
def log(func):
    def wrapper(*args, **kwargs):
        print(f"Calling {func.__name__}")
        return func(*args, **kwargs)
    return wrapper

@log
def my_function():
    pass

# Type hints with complex types
from typing import Optional, List, Callable
def process(items: List[int], transform: Callable[[int], int]) -> Optional[int]:
    ...
```

---

## 6. Collections (List, Set, Map)

### Kotlin
```kotlin
// List
val immutableList = listOf(1, 2, 3)
val mutableList   = mutableListOf(1, 2, 3)
mutableList.add(4)

// Set
val set    = setOf(1, 2, 3)
val mutSet = mutableSetOf(1, 2, 3)

// Map
val map    = mapOf("a" to 1, "b" to 2)
val mutMap = mutableMapOf("a" to 1)
mutMap["c"] = 3

// Access
val first = immutableList[0]
val value = map["a"]           // returns Int?
val safe  = map.getOrDefault("z", 0)
```

### Python
```python
# List (always mutable)
my_list = [1, 2, 3]
my_list.append(4)

# Tuple (immutable list)
my_tuple = (1, 2, 3)

# Set
my_set = {1, 2, 3}
my_set.add(4)
frozen = frozenset([1, 2, 3])    # immutable set

# Dict (Map)
my_dict = {"a": 1, "b": 2}
my_dict["c"] = 3

# Access
first = my_list[0]
value = my_dict["a"]             # KeyError if missing!
safe  = my_dict.get("z", 0)      # safe access with default
```

### Collection Comparison Table

| Kotlin | Python | Mutable? |
|--------|--------|----------|
| `listOf()` | `tuple` | ❌ |
| `mutableListOf()` | `list` | ✅ |
| `setOf()` | `frozenset` | ❌ |
| `mutableSetOf()` | `set` | ✅ |
| `mapOf()` | `dict` (sort of) | ✅ always |
| `mutableMapOf()` | `dict` | ✅ |

### 💡 Pro Tips
```python
# Dict comprehension (super useful)
squares = {x: x**2 for x in range(5)}
# {0: 0, 1: 1, 2: 4, 3: 9, 4: 16}

# Merging dicts (Python 3.9+)
merged = dict1 | dict2

# Unpacking
first, *rest = [1, 2, 3, 4, 5]
# first=1, rest=[2, 3, 4, 5]

# Named tuple (like a lightweight data class)
from collections import namedtuple
Point = namedtuple("Point", ["x", "y"])
p = Point(1, 2)
print(p.x, p.y)

# defaultdict — no getOrDefault needed
from collections import defaultdict
counts = defaultdict(int)
counts["missing"] += 1   # no KeyError!
```

---

## 7. OOP (Classes, Inheritance, Data Classes)

### Kotlin
```kotlin
// Regular class
class Person(val name: String, var age: Int) {
    fun greet() = "Hi, I'm $name"
    
    companion object {
        fun create(name: String) = Person(name, 0)
    }
}

// Data class
data class Point(val x: Int, val y: Int)

// Inheritance
open class Animal(val name: String) {
    open fun sound() = "..."
}
class Dog(name: String) : Animal(name) {
    override fun sound() = "Woof"
}

// Interface
interface Printable {
    fun print()
}
```

### Python
```python
# Regular class
class Person:
    def __init__(self, name: str, age: int):
        self.name = name
        self.age = age

    def greet(self) -> str:
        return f"Hi, I'm {self.name}"

    @classmethod
    def create(cls, name: str) -> "Person":
        return cls(name, 0)

    @staticmethod
    def validate_age(age: int) -> bool:
        return age >= 0

# Data class
from dataclasses import dataclass

@dataclass
class Point:
    x: int
    y: int
# Auto-generates __init__, __repr__, __eq__

# Inheritance
class Animal:
    def __init__(self, name: str):
        self.name = name

    def sound(self) -> str:
        return "..."

class Dog(Animal):
    def sound(self) -> str:         # override (no 'override' keyword)
        return "Woof"

# Interface = Abstract Base Class
from abc import ABC, abstractmethod

class Printable(ABC):
    @abstractmethod
    def print(self):
        pass
```

### Key Differences

| Kotlin | Python |
|--------|--------|
| Properties in constructor | `self.x = x` in `__init__` |
| `companion object` | `@classmethod` / `@staticmethod` |
| `data class` auto-generates | `@dataclass` decorator |
| `open` class required for inheritance | All classes inheritable by default |
| `override` keyword required | Just redefine the method |
| `interface` | `ABC` with `@abstractmethod` |
| `object` singleton | Module-level or `__new__` tricks |

### 💡 Pro Tips
```python
# Properties (like Kotlin's get/set)
class Circle:
    def __init__(self, radius: float):
        self._radius = radius

    @property
    def radius(self) -> float:
        return self._radius

    @radius.setter
    def radius(self, value: float):
        if value < 0:
            raise ValueError("Radius cannot be negative")
        self._radius = value

# __dunder__ methods (operator overloading)
@dataclass
class Vector:
    x: float
    y: float

    def __add__(self, other: "Vector") -> "Vector":
        return Vector(self.x + other.x, self.y + other.y)

    def __repr__(self) -> str:
        return f"Vector({self.x}, {self.y})"
```

---

## 8. Functional Programming (map, filter, reduce)

### Kotlin
```kotlin
val numbers = listOf(1, 2, 3, 4, 5)

val doubled  = numbers.map { it * 2 }
val evens    = numbers.filter { it % 2 == 0 }
val sum      = numbers.reduce { acc, n -> acc + n }
val total    = numbers.fold(10) { acc, n -> acc + n }    // with initial value
val flat     = listOf(listOf(1,2), listOf(3,4)).flatten()
val grouped  = numbers.groupBy { if (it % 2 == 0) "even" else "odd" }
val sorted   = numbers.sortedBy { it }
val first    = numbers.firstOrNull { it > 3 }
```

### Python
```python
numbers = [1, 2, 3, 4, 5]

# map returns an iterator — wrap in list()
doubled  = list(map(lambda x: x * 2, numbers))
# Pythonic: use list comprehension instead!
doubled  = [x * 2 for x in numbers]

evens    = list(filter(lambda x: x % 2 == 0, numbers))
evens    = [x for x in numbers if x % 2 == 0]           # Pythonic

from functools import reduce
total    = reduce(lambda acc, n: acc + n, numbers)       # no initial
total    = reduce(lambda acc, n: acc + n, numbers, 10)   # with initial

flat     = [x for sub in [[1,2],[3,4]] for x in sub]    # flatten
# or: import itertools; list(itertools.chain.from_iterable(...))

# groupBy equivalent
from itertools import groupby
grouped  = {}
for x in numbers:
    key = "even" if x % 2 == 0 else "odd"
    grouped.setdefault(key, []).append(x)

sorted_n = sorted(numbers)                               # returns new list
numbers.sort()                                           # in-place

first    = next((x for x in numbers if x > 3), None)    # firstOrNull
```

### Key Differences
- Python's `map()`/`filter()` return **lazy iterators**, not lists — wrap in `list()`
- **List comprehensions are more Pythonic** than `map`/`filter` with lambdas
- `reduce` requires importing from `functools`
- No built-in `groupBy` — use `itertools.groupby` or manual dict building

### 💡 Pro Tips
```python
# Generator expressions (lazy — like Kotlin sequences)
lazy_squares = (x**2 for x in range(1000000))  # no memory until consumed

# all() / any()
all(x > 0 for x in numbers)   # like Kotlin's all {}
any(x > 4 for x in numbers)   # like Kotlin's any {}

# sum/min/max with key
max(numbers, key=lambda x: -x)    # min element

# zip for parallel iteration
paired = list(zip([1,2,3], ["a","b","c"]))
# [(1,'a'), (2,'b'), (3,'c')]
```

---

## 9. Exception Handling

### Kotlin
```kotlin
try {
    val result = riskyOperation()
} catch (e: IllegalArgumentException) {
    println("Bad arg: ${e.message}")
} catch (e: Exception) {
    println("General: ${e.message}")
} finally {
    println("Always runs")
}

// Custom exception
class AppException(message: String) : Exception(message)

// runCatching (functional style)
val result = runCatching { riskyOperation() }
    .onSuccess { println("Got: $it") }
    .onFailure { println("Failed: ${it.message}") }
    .getOrDefault(0)
```

### Python
```python
try:
    result = risky_operation()
except ValueError as e:
    print(f"Bad value: {e}")
except (TypeError, KeyError) as e:   # catch multiple
    print(f"Type/Key error: {e}")
except Exception as e:
    print(f"General: {e}")
else:
    print("Runs only if no exception")    # no Kotlin equivalent!
finally:
    print("Always runs")

# Custom exception
class AppException(Exception):
    def __init__(self, message: str):
        super().__init__(message)

# Raising
raise AppException("Something went wrong")

# Context managers (like use{} / Closeable)
with open("file.txt") as f:
    content = f.read()   # auto-closes even on exception
```

### Key Differences

| Kotlin | Python |
|--------|--------|
| `catch (e: Type)` | `except Type as e` |
| No `else` block | `else:` block runs if no exception |
| `finally` | `finally:` |
| `runCatching {}` | Use `contextlib.suppress()` or manual |
| Checked vs unchecked N/A | All exceptions are unchecked |
| `throw` | `raise` |

---

## 10. Coroutines vs Async/Await

### Kotlin
```kotlin
import kotlinx.coroutines.*

// Coroutine scope
fun main() = runBlocking {
    val result = async { fetchData() }
    println(result.await())
}

suspend fun fetchData(): String {
    delay(1000)
    return "data"
}

// Launch (fire and forget)
GlobalScope.launch {
    doSomething()
}

// Parallel execution
val a = async { fetchA() }
val b = async { fetchB() }
println("${a.await()} ${b.await()}")
```

### Python
```python
import asyncio

# Async function
async def fetch_data() -> str:
    await asyncio.sleep(1)
    return "data"

# Entry point
async def main():
    result = await fetch_data()
    print(result)

asyncio.run(main())     # like runBlocking

# Fire and forget (create task)
async def main():
    task = asyncio.create_task(fetch_data())  # like launch
    await task

# Parallel execution (like async { }.await())
async def main():
    a, b = await asyncio.gather(fetch_a(), fetch_b())
    print(a, b)
```

### Key Differences

| Kotlin | Python |
|--------|--------|
| `suspend fun` | `async def` |
| `await` inside coroutine | `await` inside `async def` |
| `async { }` returns `Deferred` | `asyncio.create_task()` |
| `runBlocking { }` | `asyncio.run()` |
| `launch { }` | `asyncio.create_task()` |
| `async { }.await()` | `await coroutine` |
| `awaitAll()` | `asyncio.gather()` |
| Flows | `async generators` |

### 💡 Pro Tips
```python
# aiohttp for async HTTP (like Ktor client)
import aiohttp

async def fetch_url(url: str) -> str:
    async with aiohttp.ClientSession() as session:
        async with session.get(url) as response:
            return await response.text()

# Timeout
async def with_timeout():
    try:
        result = await asyncio.wait_for(fetch_data(), timeout=5.0)
    except asyncio.TimeoutError:
        print("Timed out!")
```

---

## 11. File Handling

### Kotlin
```kotlin
import java.io.File

// Read
val content = File("file.txt").readText()
val lines   = File("file.txt").readLines()

// Write
File("output.txt").writeText("Hello, World!")
File("output.txt").appendText("More content")

// With buffering
File("large.txt").bufferedReader().use { reader ->
    reader.forEachLine { line -> println(line) }
}

// Path
val path = "data/file.txt"
```

### Python
```python
# Read (context manager auto-closes file)
with open("file.txt", "r") as f:
    content = f.read()                    # whole file
    
with open("file.txt", "r") as f:
    lines = f.readlines()                 # list of lines

with open("file.txt", "r") as f:
    for line in f:                        # memory-efficient
        print(line.strip())

# Write
with open("output.txt", "w") as f:       # "w" overwrites
    f.write("Hello, World!")

with open("output.txt", "a") as f:       # "a" appends
    f.write("More content")

# Using pathlib (modern, like Java's Path)
from pathlib import Path

path = Path("data") / "file.txt"         # cross-platform path joining
content = path.read_text()
path.write_text("Hello!")
path.exists()
path.mkdir(parents=True, exist_ok=True)  # like mkdirs()
```

### File Modes Reference

| Mode | Kotlin equiv | Description |
|------|-------------|-------------|
| `"r"` | `readText()` | Read text |
| `"w"` | `writeText()` | Write (overwrite) |
| `"a"` | `appendText()` | Append |
| `"rb"` | `readBytes()` | Read binary |
| `"wb"` | `writeBytes()` | Write binary |

---

## 12. JSON Parsing

### Kotlin (with kotlinx.serialization)
```kotlin
import kotlinx.serialization.*
import kotlinx.serialization.json.*

@Serializable
data class User(val name: String, val age: Int)

// Serialize
val user = User("Alice", 30)
val json = Json.encodeToString(user)
// {"name":"Alice","age":30}

// Deserialize
val parsed = Json.decodeFromString<User>(json)

// From Map
val jsonElement = Json.parseToJsonElement(json)
val name = jsonElement.jsonObject["name"]?.jsonPrimitive?.content
```

### Python (built-in json module)
```python
import json
from dataclasses import dataclass

@dataclass
class User:
    name: str
    age: int

# Serialize (to JSON string)
user = User("Alice", 30)
json_str = json.dumps({"name": user.name, "age": user.age})
# or with __dict__: json.dumps(user.__dict__)

# Deserialize (from JSON string)
data = json.loads(json_str)
parsed_user = User(**data)       # dict unpacking

# From/to file
with open("data.json") as f:
    data = json.load(f)          # load from file

with open("output.json", "w") as f:
    json.dump(data, f, indent=2) # write to file

# Pretty print
print(json.dumps(data, indent=2))
```

### 💡 Pro Tips — Pydantic (like kotlinx.serialization with validation)
```python
# pip install pydantic
from pydantic import BaseModel, validator

class User(BaseModel):
    name: str
    age: int

    @validator("age")
    def age_must_be_positive(cls, v):
        if v < 0:
            raise ValueError("Age must be positive")
        return v

# Parse from dict (like decodeFromString)
user = User(**{"name": "Alice", "age": 30})
# Serialize
json_str = user.model_dump_json()
```

---

## 13. Common Libraries — Ecosystem Comparison

| Category | Kotlin/Android | Python |
|----------|---------------|--------|
| HTTP Client | Ktor, OkHttp | `requests`, `httpx`, `aiohttp` |
| JSON | kotlinx.serialization, Gson | `json` (built-in), `pydantic` |
| DI | Hilt, Koin | no standard; `dependency-injector` |
| Testing | JUnit, Kotest | `pytest`, `unittest` |
| Logging | Timber, Logcat | `logging` (built-in) |
| DB ORM | Room, Exposed | `SQLAlchemy`, `tortoise-orm` |
| Reactive | Kotlin Flows | `RxPY`, async generators |
| CLI | clikt | `click`, `typer`, `argparse` |
| Data Science | — | `pandas`, `numpy`, `scipy` |
| Web Framework | Ktor | `FastAPI`, `Django`, `Flask` |
| Scraping | — | `BeautifulSoup`, `playwright` |
| ML | — | `scikit-learn`, `tensorflow`, `pytorch` |
| Config | BuildConfig | `python-dotenv`, `pydantic-settings` |
| Task Queue | WorkManager | `celery`, `rq` |

### Quick Package Management

```bash
# Kotlin/Gradle
dependencies {
    implementation("io.ktor:ktor-client-core:2.0.0")
}

# Python (pip)
pip install requests pydantic fastapi

# Python (modern, with pyproject.toml)
uv add requests pydantic     # uv is the modern fast tool
```

---

## ⚡ Mini Practice Snippets

### Snippet 1: FizzBuzz
```python
# Kotlin style thinking → Python Pythonic
for i in range(1, 101):
    print("FizzBuzz" if i % 15 == 0 else
          "Fizz"     if i % 3  == 0 else
          "Buzz"     if i % 5  == 0 else str(i))
```

### Snippet 2: Fetch and Parse JSON
```python
import requests

def get_user(user_id: int) -> dict | None:
    response = requests.get(f"https://api.example.com/users/{user_id}")
    if response.status_code == 200:
        return response.json()
    return None

user = get_user(1)
print(user.get("name", "Unknown"))
```

### Snippet 3: Async API Call
```python
import asyncio
import aiohttp

async def fetch_users(ids: list[int]) -> list[dict]:
    async with aiohttp.ClientSession() as session:
        tasks = [session.get(f"https://api.example.com/users/{i}") for i in ids]
        responses = await asyncio.gather(*tasks)
        return [await r.json() for r in responses]

users = asyncio.run(fetch_users([1, 2, 3]))
```

### Snippet 4: Data Processing Pipeline
```python
from dataclasses import dataclass
from typing import List

@dataclass
class Product:
    name: str
    price: float
    category: str

products = [
    Product("iPhone", 999.0, "Electronics"),
    Product("Book", 19.99, "Education"),
    Product("Laptop", 1299.0, "Electronics"),
]

# Kotlin: products.filter { it.category == "Electronics" }.map { it.price }.average()
electronics_avg = sum(
    p.price for p in products if p.category == "Electronics"
) / len([p for p in products if p.category == "Electronics"])

print(f"Average electronics price: ${electronics_avg:.2f}")
```

---

## 🗓️ 1-Day Revision Guide

### Morning (2 hrs): Core Syntax
- [ ] Variables: `snake_case`, no `val`/`var`, type hints optional
- [ ] `None` vs null — always use `is None`, not `== None`
- [ ] Indentation = braces — 4 spaces, not tabs
- [ ] `True`/`False` capitalized, `and`/`or`/`not` (not `&&`/`||`/`!`)

### Mid-Morning (1 hr): Collections
- [ ] `list` = mutable, `tuple` = immutable
- [ ] `dict` for maps, `.get(key, default)` for safe access
- [ ] List comprehensions: `[x for x in items if condition]`

### Afternoon (2 hrs): Functions & OOP
- [ ] `def`, `*args`, `**kwargs`, default params gotcha (no mutable defaults!)
- [ ] `class`, `self`, `__init__`, `@property`
- [ ] `@dataclass`, inheritance, `@classmethod`, `@staticmethod`

### Late Afternoon (1 hr): Async & Error Handling
- [ ] `async def` + `await`, `asyncio.run()`, `asyncio.gather()`
- [ ] `try/except/else/finally`, `raise`, `with` statement

### Evening (1 hr): Ecosystem Scan
- [ ] `requests` for HTTP, `json` for JSON, `pathlib` for files
- [ ] `pytest` basics, `pydantic` for validation
- [ ] Virtual environments: `python -m venv .venv && source .venv/bin/activate`

---

## 🚨 Top 10 Kotlin-Dev Mistakes in Python

| # | Mistake | Fix |
|---|---------|-----|
| 1 | Using camelCase for functions/vars | Use `snake_case` |
| 2 | `if name == None` | `if name is None` |
| 3 | Mutable default arguments `def f(lst=[])` | Use `def f(lst=None)` |
| 4 | `true`/`false` instead of `True`/`False` | Always capitalize |
| 5 | Forgetting `list()` around `map()`/`filter()` | Wrap in `list()` or use comprehensions |
| 6 | `dict["key"]` on missing key | Use `dict.get("key", default)` |
| 7 | Expecting compiler type errors | Write tests with `pytest` |
| 8 | Using `++i` / `i++` | Use `i += 1` |
| 9 | Calling `async def` without `await` | Always `await` or use `asyncio.run()` |
| 10 | Forgetting `self` in class methods | Every instance method needs `self` as first arg |

---

*Happy Pythoning! 🐍 — Remember: Python rewards simplicity and readability above all else.*