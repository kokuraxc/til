## Asterisk (\*) of Python

4 use cases of asterisk in Python.
1. multiplication and power operations.
2. extending the list-type containers.
3. variadic arguments ('packing').
4. unpacking the containers.

### multiplication and power operations

```py
>>> 2 * 3
6
>>> 2 ** 3
8
>>> 1.414 * 1.414
1.9993959999999997
>>> 1.414 ** 1.414
1.6320575353248798
```

### extending the list-type containers

```py
# Initialize the zero-valued list with 100 length
zeros_list = [0] * 100 

# Declare the zero-valued tuple with 100 length
zeros_tuple = (0,) * 100  

# Extending the "vector_list" by 3 times
vector_list = [[1, 2, 3]] 
for i, vector in enumerate(vector_list * 3):     
    print("{0} scalar product of vector: {1}".format((i + 1), [(i + 1) * e for e in vector]))
# 1 scalar product of vector: [1, 2, 3]
# 2 scalar product of vector: [2, 4, 6]
# 3 scalar product of vector: [3, 6, 9]
```

### variadic arguments (packing)

```py
def save_ranking(*args):
    print(args) 
save_ranking('ming', 'alice', 'tom', 'wilson', 'roy')
# ('ming', 'alice', 'tom', 'wilson', 'roy')
```

`*args` means accepting arbitrary number of *positional arguments*.

```py
def save_ranking(**kwargs):
    print(kwargs)
save_ranking(first='ming', second='alice', fourth='wilson', third='tom', fifth='roy')
# {'first': 'ming', 'second': 'alice', 'fourth': 'wilson', 'third': 'tom', 'fifth': 'roy'}
```

`**kwargs` means accepting arbitrary number of *keyword arguments*.

```py
def save_ranking(*args, **kwargs):
    print(args)     
    print(kwargs)
save_ranking('ming', 'alice', 'tom', fourth='wilson', fifth='roy')
# ('ming', 'alice', 'tom')
# {'fourth': 'wilson', 'fifth': 'roy'}
```

### unpacking the containers

```py
from functools import reduce

primes = [2, 3, 5, 7, 11, 13]

def product(*numbers):
    p = reduce(lambda x, y: x * y, numbers)
    return p 

product(*primes)
# 30030

product(primes)
# [2, 3, 5, 7, 11, 13]
```

Because the `product()` take the variable arguments, we need to unpack the our list data and pass it to that function. In this case, if we pass the `primes` as `*primes`, every elements of the `primes` list will be unpacked, then stored in list called `numbers`. If pass that list `primes` to the function without unpacking, the `numbers` will has only one `primes` list not all elements of `primes`.

For tuples, it's the same as lists. And for dict, just use \*\* instead of \*.

```py
headers = {
    'Accept': 'text/plain',
    'Content-Length': 348, 
    'Host': 'http://mingrammer.com' 
}  

def pre_process(**headers): 
    content_length = headers['Content-Length'] 
    print('content length: ', content_length) 
    
    host = headers['Host']
    if 'https' not in host: 
        raise ValueError('You must use SSL for http communication')  
        
pre_process(**headers)
# content length:  348
# Traceback (most recent call last):
#   File "<stdin>", line 1, in <module>
#   File "<stdin>", line 7, in pre_process
# ValueError: You must use SSL for http communication
```

There is also one more type of unpacking, it is not for function but just unpack the list or tuple data to other variables dynamically.

```py

numbers = [1, 2, 3, 4, 5, 6]

# The left side of unpacking should be list or tuple.
*a, = numbers
# a = [1, 2, 3, 4, 5, 6]

*a, b = numbers
# a = [1, 2, 3, 4, 5]
# b = 6

a, *b, = numbers
# a = 1 
# b = [2, 3, 4, 5, 6]

a, *b, c = numbers
# a = 1
# b = [2, 3, 4, 5]
# c = 6
```

Here, read from right to left, `numbers` is unpacked to different variables. `*a` and `*b` just pack part of the unpacked numbers from the list.

> [understanding the asterisk(\*) of Python](https://medium.com/understand-the-python/understanding-the-asterisk-of-python-8b9daaa4a558)