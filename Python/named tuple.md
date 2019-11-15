A __namedtuple__ assign names, as wellas the numerical index, to each member.

__namedtuple__ instances are just as memory efficient as regular tuples because they do not have per-instance dictionaries. Each kind of __namedtuple__ is represented by its own class, created by using the `namedtuple()` *factory function*. The arguments are the **name** of the new class and a string containing the **names of the elements**.

```
import collections

Person = namedtuple('Person', 'name age gender')
bob = Person(name='Bob', age=30, gender='male')
print(bob)
// Person(name='Bob', age=30, gender='male')
```
