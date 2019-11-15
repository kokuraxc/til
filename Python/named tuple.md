##named tuple

A __namedtuple__ assign names, as wellas the numerical index, to each member.

__namedtuple__ instances are just as memory efficient as regular tuples because they do not have per-instance dictionaries. Each kind of __namedtuple__ is represented by its own class, created by using the `namedtuple()` *factory function*. The arguments are the **name** of the new class and a string containing the **names of the elements**.

```python
import collections

Person = namedtuple('Person', 'name age gender')
bob = Person(name='Bob', age=30, gender='male')
print(bob)
// Person(name='Bob', age=30, gender='male')
```

> [Python Module of the Week - namedtuple](https://pymotw.com/2/collections/namedtuple.html)

---

Named tuples are basically easy-to-create, lightweight object types. Named tuple instances can be referenced using object-like variable dereferencing or the standard tuple syntax. They can be used similarly to `struct` or other common record types, except that **they are immutable**.

For example, it is common to represent a point as a tuple `(x, y)`. This leads to code like the following:

```python
pt1 = (1.0, 5.0)
pt2 = (2.5, 1.5)

from math import sqrt
line_length = sqrt((pt1[0] - pt2[0])**2 + (pt1[1]-pt2[1])**2)
```
Using a named tuple it becomes **more readable**:
```python
from collections import namedtuple
Point = namedtuple('Point', 'x y')
pt1 = Point(1.0, 5.0)
pt2 = Point(2.5, 1.5)

from math import sqrt
line_length = sqrt((pt1.x-pt2.x)**2 + (pt1.y-pt2.y)**2)
```

However, named tuples are still backwards compatible with normal tuples.

Thus, **you should use named tuples instead of tuples anywhere you think object notation will make your code more pythonic and more easily readable**.

> [stack**overflow** What are "named tuples" in Python?](https://stackoverflow.com/a/2970722)