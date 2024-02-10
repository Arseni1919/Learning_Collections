# Learning Collections

## Improving Code Readability: `namedtuple()`

To create new tuple subclass using `namedtuple()`, you need two required arguments:

1. `typename` is the name of the class you’re creating. It must be a string with a valid Python identifier.
2. `field_names` is the list of field names you’ll use to access the items in the resulting tuple. 
It can be:
   - An iterable of strings, such as `["field1", "field2", ..., "fieldN"]`
   - A string with whitespace-separated field names, such as `"field1 field2 ... fieldN"`
   - A string with comma-separated field names, such as `"field1, field2, ..., fieldN"`

```python
>>> from collections import namedtuple

>>> # Use a list of strings as field names
>>> Point = namedtuple("Point", ["x", "y"])
>>> point = Point(2, 4)
>>> point
Point(x=2, y=4)

>>> # Access the coordinates
>>> point.x
2
>>> point.y
4
>>> point[0]
2

>>> # Use a generator expression as field names
>>> Point = namedtuple("Point", (field for field in "xy"))
>>> Point(2, 4)
Point(x=2, y=4)

>>> # Use a string with comma-separated field names
>>> Point = namedtuple("Point", "x, y")
>>> Point(2, 4)
Point(x=2, y=4)

>>> # Use a string with space-separated field names
>>> Point = namedtuple("Point", "x y")
>>> Point(2, 4)
Point(x=2, y=4)
```

```python
>>> from collections import namedtuple

>>> # Define default values for fields
>>> Person = namedtuple("Person", "name job", defaults=["Python Developer"])
>>> person = Person("Jane")
>>> person
Person(name='Jane', job='Python Developer')

>>> # Create a dictionary from a named tuple
>>> person._asdict()
{'name': 'Jane', 'job': 'Python Developer'}

>>> # Replace the value of a field
>>> person = person._replace(job="Web Developer")
>>> person
Person(name='Jane', job='Web Developer')
```


## Building Efficient Queues and Stacks: `deque`

Note: The word deque is pronounced “deck” and stands for double-ended queue.

Python’s `deque` was the first data structure in collections. 
This sequence-like data type is a generalization of stacks and queues designed to support memory-efficient and fast append and pop operations on both ends of the data structure.

```python
>>> from collections import deque

>>> ticket_queue = deque()
>>> ticket_queue
deque([])

>>> # People arrive to the queue
>>> ticket_queue.append("Jane")
>>> ticket_queue.append("John")
>>> ticket_queue.append("Linda")

>>> ticket_queue
deque(['Jane', 'John', 'Linda'])

>>> # People bought their tickets
>>> ticket_queue.popleft()
'Jane'
>>> ticket_queue.popleft()
'John'
>>> ticket_queue.popleft()
'Linda'

>>> # No people on the queue
>>> ticket_queue.popleft()
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
IndexError: pop from an empty deque
```

The deque initializer takes two optional arguments:

- `iterable` holds an iterable that serves as an initializer.
- `maxlen` holds an integer number that specifies the maximum length of the deque.

If you don’t provide an iterable, then you get an empty deque. 
If you supply a value to `maxlen`, then your deque will only store up to `maxlen` items.

```python
>>> from collections import deque

>>> recent_files = deque(["core.py", "README.md", "__init__.py"], maxlen=3)

>>> recent_files.appendleft("database.py")
>>> recent_files
deque(['database.py', 'core.py', 'README.md'], maxlen=3)

>>> recent_files.appendleft("requirements.txt")
>>> recent_files
deque(['requirements.txt', 'database.py', 'core.py'], maxlen=3)
```

```python
>>> from collections import deque

>>> # Use different iterables to create deques
>>> deque((1, 2, 3, 4))
deque([1, 2, 3, 4])

>>> deque([1, 2, 3, 4])
deque([1, 2, 3, 4])

>>> deque("abcd")
deque(['a', 'b', 'c', 'd'])

>>> # Unlike lists, deque doesn't support .pop() with arbitrary indices
>>> deque("abcd").pop(2)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: pop() takes no arguments (1 given)

>>> # Extend an existing deque
>>> numbers = deque([1, 2])
>>> numbers.extend([3, 4, 5])
>>> numbers
deque([1, 2, 3, 4, 5])

>>> numbers.extendleft([-1, -2, -3, -4, -5])
>>> numbers
deque([-5, -4, -3, -2, -1, 1, 2, 3, 4, 5])

>>> # Insert an item at a given position
>>> numbers.insert(5, 0)
>>> numbers
deque([-5, -4, -3, -2, -1, 0, 1, 2, 3, 4, 5])
```

Deques also support sequence operations:

Method	Description
- `.clear()`	Remove all the elements from a deque
- `.copy()`	Create a shallow copy of a deque
- `.count(x)`	Count the number of deque elements equal to x
- `.remove(value)`	Remove the first occurrence of value

Another interesting feature of deques is the ability to rotate their elements using `.rotate()`:

```python
>>> from collections import deque

>>> ordinals = deque(["first", "second", "third"])
>>> ordinals.rotate()
>>> ordinals
deque(['third', 'first', 'second'])

>>> ordinals.rotate(2)
>>> ordinals
deque(['first', 'second', 'third'])

>>> ordinals.rotate(-2)
>>> ordinals
deque(['third', 'first', 'second'])

>>> ordinals.rotate(-1)
>>> ordinals
deque(['first', 'second', 'third'])
```

Finally, you can use indices to access the elements in a deque, but you can’t slice a deque:

```python
>>> from collections import deque

>>> ordinals = deque(["first", "second", "third"])
>>> ordinals[1]
'second'

>>> ordinals[0:2]
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: sequence index must be integer, not 'slice'
```


## Handling Missing Keys: `defaultdict`

```python
>>> from collections import defaultdict

>>> counter = defaultdict(int)
>>> counter
defaultdict(<class 'int'>, {})
>>> counter["dogs"]
0
>>> counter
defaultdict(<class 'int'>, {'dogs': 0})

>>> counter["dogs"] += 1
>>> counter["dogs"] += 1
>>> counter["dogs"] += 1
>>> counter["cats"] += 1
>>> counter["cats"] += 1
>>> counter
defaultdict(<class 'int'>, {'dogs': 3, 'cats': 2})
```


## Keeping Your Dictionaries Ordered: `OrderedDict`

```python
>>> from collections import OrderedDict

>>> life_stages = OrderedDict()

>>> life_stages["childhood"] = "0-9"
>>> life_stages["adolescence"] = "9-18"
>>> life_stages["adulthood"] = "18-65"
>>> life_stages["old"] = "+65"

>>> for stage, years in life_stages.items():
...     print(stage, "->", years)
...
childhood -> 0-9
adolescence -> 9-18
adulthood -> 18-65
old -> +65
```

## Counting Objects in One Go: `Counter`

```python
>>> from collections import Counter

>>> Counter("mississippi")
Counter({'i': 4, 's': 4, 'p': 2, 'm': 1})
```
```python
>>> from collections import Counter

>>> Counter([1, 1, 2, 3, 3, 3, 4])
Counter({3: 3, 1: 2, 2: 1, 4: 1})

>>> Counter(([1], [1]))
Traceback (most recent call last):
  ...
TypeError: unhashable type: 'list'
```
```python
>>> from collections import Counter

>>> letters = Counter("mississippi")
>>> letters
Counter({'i': 4, 's': 4, 'p': 2, 'm': 1})

>>> # Update the counts of m and i
>>> letters.update(m=3, i=4)
>>> letters
Counter({'i': 8, 'm': 4, 's': 4, 'p': 2})

>>> # Add a new key-count pair
>>> letters.update({"a": 2})
>>> letters
Counter({'i': 8, 'm': 4, 's': 4, 'p': 2, 'a': 2})

>>> # Update with another counter
>>> letters.update(Counter(["s", "s", "p"]))
>>> letters
Counter({'i': 8, 's': 6, 'm': 4, 'p': 3, 'a': 2})
```
Another difference between `Counter` and `dict` is that accessing a missing key returns 0 instead of raising a `KeyError`:
```python
>>> from collections import Counter

>>> letters = Counter("mississippi")
>>> letters["a"]
0
```

## Chaining Dictionaries Together: `ChainMap`

```python
>>> from collections import ChainMap

>>> cmd_proxy = {}  # The user doesn't provide a proxy
>>> local_proxy = {"proxy": "proxy.local.com"}
>>> global_proxy = {"proxy": "proxy.global.com"}

>>> config = ChainMap(cmd_proxy, local_proxy, global_proxy)
>>> config["proxy"]
'proxy.local.com'
```
ChainMap allows you to define the appropriate priority for the application’s proxy configuration. A key lookup searches cmd_proxy, then local_proxy, and finally global_proxy, returning the first instance of the key at hand. In this example, the user doesn’t provide a proxy at the command line, so your application uses the proxy in local_proxy.









## Credits

- [Python's collections: A Buffet of Specialized Data Types](https://realpython.com/python-collections-module/)
