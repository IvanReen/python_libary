
# collextions：容器数据类型

## ChainMap：搜索多个字典

访问值


```python
import collections

a = {'a': 'A', 'c': 'C'}
b = {'b': 'B', 'c': 'D'}

m = collections.ChainMap(a, b)

print('Individual Values')
print('a = {}'.format(m['a']))
print('b = {}'.format(m['b']))
print('c = {}'.format(m['c']))
print()

print('Keys = {}'.format(list(m.keys())))
print('Values = {}'.format(list(m.values())))
print()

print('Items:')
for k, v in m.items():
    print('{} = {}'.format(k, v))
print()

print('"d" in m: {}'.format(('d' in m)))
```

    Individual Values
    a = A
    b = B
    c = C
    
    Keys = ['b', 'c', 'a']
    Values = ['B', 'C', 'A']
    
    Items:
    b = B
    c = C
    a = A
    
    "d" in m: False


重排


```python
import collections

a = {'a': 'A', 'c': 'C'}
b = {'b': 'B', 'c': 'D'}

m = collections.ChainMap(a, b)

print(m.maps)
print('c = {}\n'.format(m['c']))

# reverse the list
m.maps = list(reversed(m.maps))

print(m.maps)
print('c = {}'.format(m['c']))
```

    [{'a': 'A', 'c': 'C'}, {'b': 'B', 'c': 'D'}]
    c = C
    
    [{'b': 'B', 'c': 'D'}, {'a': 'A', 'c': 'C'}]
    c = D


更新值


```python
import collections

a = {'a': 'A', 'c': 'C'}
b = {'b': 'B', 'c': 'D'}

m = collections.ChainMap(a, b)
print('Before: {}'.format(m['c']))
a['c'] = 'E'
print('After : {}'.format(m['c']))
```

    Before: C
    After : E



```python
import collections

a = {'a': 'A', 'c': 'C'}
b = {'b': 'B', 'c': 'D'}

m = collections.ChainMap(a, b)
print('Before:', m)
m['c'] = 'E'
print('After :', m)
print('a:', a)
```

    Before: ChainMap({'a': 'A', 'c': 'C'}, {'b': 'B', 'c': 'D'})
    After : ChainMap({'a': 'A', 'c': 'E'}, {'b': 'B', 'c': 'D'})
    a: {'a': 'A', 'c': 'E'}



```python
import collections

a = {'a': 'A', 'c': 'C'}
b = {'b': 'B', 'c': 'D'}

m1 = collections.ChainMap(a, b)
m2 = m1.new_child()

print('m1 before:', m1)
print('m2 before:', m2)

m2['c'] = 'E'

print('m1 after:', m1)
print('m2 after:', m2)
```

    m1 before: ChainMap({'a': 'A', 'c': 'C'}, {'b': 'B', 'c': 'D'})
    m2 before: ChainMap({}, {'a': 'A', 'c': 'C'}, {'b': 'B', 'c': 'D'})
    m1 after: ChainMap({'a': 'A', 'c': 'C'}, {'b': 'B', 'c': 'D'})
    m2 after: ChainMap({'c': 'E'}, {'a': 'A', 'c': 'C'}, {'b': 'B', 'c': 'D'})



```python
import collections

a = {'a': 'A', 'c': 'C'}
b = {'b': 'B', 'c': 'D'}
c = {'c': 'E'}

m1 = collections.ChainMap(a, b)
m2 = m1.new_child(c)

print('m1["c"] = {}'.format(m1['c']))
print('m2["c"] = {}'.format(m2['c']))
```

    m1["c"] = C
    m2["c"] = E


## Counter：统计可散列的对象

初始化


```python
import collections

print(collections.Counter(['a', 'b', 'c', 'a', 'b', 'b']))
print(collections.Counter({'a': 2, 'b': 3, 'c': 1}))
print(collections.Counter(a=2, b=3, c=1))
```

    Counter({'b': 3, 'a': 2, 'c': 1})
    Counter({'b': 3, 'a': 2, 'c': 1})
    Counter({'b': 3, 'a': 2, 'c': 1})



```python
import collections

# 空对象
c = collections.Counter()
print('Initial :', c)

# 通过update方法填充
c.update('abcdaab')
print('Sequence:', c)

c.update({'a': 1, 'd': 5})
print('Dict    :', c)
```

    Initial : Counter()
    Sequence: Counter({'a': 3, 'b': 2, 'c': 1, 'd': 1})
    Dict    : Counter({'d': 6, 'a': 4, 'b': 2, 'c': 1})


访问计数，使用字典的方式获取值


```python
import collections

c = collections.Counter('abcdaab')

for letter in 'abcde':
    print('{} : {}'.format(letter, c[letter]))
```

    a : 3
    b : 2
    c : 1
    d : 1
    e : 0


elements()方法获取所有的key，无序的


```python
import collections

c = collections.Counter('extremely')
c['z'] = 0
print(c)
print(list(c.elements()))
```

    Counter({'e': 3, 'x': 1, 't': 1, 'r': 1, 'm': 1, 'l': 1, 'y': 1, 'z': 0})
    ['e', 'e', 'e', 'x', 't', 'r', 'm', 'l', 'y']


most_common()方法，输出前多少个


```python
import collections

c = collections.Counter()
with open('/usr/share/dict/words', 'rt') as f:
    for line in f:
        c.update(line.rstrip().lower())

print('Most common:')
for letter, count in c.most_common(3):
    print('{}: {:>7}'.format(letter, count))
```

    Most common:
    e:  235331
    i:  201032
    a:  199554


算术操作


```python
import collections

c1 = collections.Counter(['a', 'b', 'c', 'a', 'b', 'b'])
c2 = collections.Counter('alphabet')

print('C1:', c1)
print('C2:', c2)

print('\nCombined counts:')
print(c1 + c2)

print('\nSubtraction:')
print(c1 - c2)

print('\nIntersection (taking positive minimums):')
print(c1 & c2)

print('\nUnion (taking maximums):')
print(c1 | c2)
```

    C1: Counter({'b': 3, 'a': 2, 'c': 1})
    C2: Counter({'a': 2, 'l': 1, 'p': 1, 'h': 1, 'b': 1, 'e': 1, 't': 1})
    
    Combined counts:
    Counter({'a': 4, 'b': 4, 'c': 1, 'l': 1, 'p': 1, 'h': 1, 'e': 1, 't': 1})
    
    Subtraction:
    Counter({'b': 2, 'c': 1})
    
    Intersection (taking positive minimums):
    Counter({'a': 2, 'b': 1})
    
    Union (taking maximums):
    Counter({'b': 3, 'a': 2, 'c': 1, 'l': 1, 'p': 1, 'h': 1, 'e': 1, 't': 1})


## defaultdict：缺少的键返回一个默认值

初始化


```python
import collections


def default_factory():
    return 'default value'


d = collections.defaultdict(default_factory, foo='bar')
print('d:', d)
print('foo =>', d['foo'])
print('bar =>', d['bar'])
```

    d: defaultdict(<function default_factory at 0x105579950>, {'foo': 'bar'})
    foo => bar
    bar => default value



```python
s = [('yellow', 1), ('blue', 2), ('yellow', 3), ('blue', 4), ('red', 1)]
d = collections.defaultdict(list)
for k, v in s:
    d[k].append(v)
sorted(d.items())
```




    [('blue', [2, 4]), ('red', [1]), ('yellow', [1, 3])]




```python
d = {}
for k, v in s:
    d.setdefault(k, []).append(v)

sorted(d.items())
```




    [('blue', [2, 4]), ('red', [1]), ('yellow', [1, 3])]



## deque：双端队列


```python
import collections

d = collections.deque('abcdefg')
print('Deque:', d)
print('Length:', len(d))
print('Left end:', d[0])
print('Right end:', d[-1])

d.remove('c')
print('remove(c):', d)
```

    Deque: deque(['a', 'b', 'c', 'd', 'e', 'f', 'g'])
    Length: 7
    Left end: a
    Right end: g
    remove(c): deque(['a', 'b', 'd', 'e', 'f', 'g'])


填充


```python
import collections

# Add to the right
d1 = collections.deque()
d1.extend('abcdefg')
print('extend    :', d1)
d1.append('h')
print('append    :', d1)

# Add to the left
d2 = collections.deque()
d2.extendleft(range(6))
print('extendleft:', d2)
d2.appendleft(6)
print('appendleft:', d2)
```

    extend    : deque(['a', 'b', 'c', 'd', 'e', 'f', 'g'])
    append    : deque(['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h'])
    extendleft: deque([5, 4, 3, 2, 1, 0])
    appendleft: deque([6, 5, 4, 3, 2, 1, 0])


消费


```python
import collections

print('From the right:')
d = collections.deque('abcdefg')
while True:
    try:
        print(d.pop(), end='')
    except IndexError:
        break
print()

print('\nFrom the left:')
d = collections.deque(range(6))
while True:
    try:
        print(d.popleft(), end='')
    except IndexError:
        break
print()
```

    From the right:
    gfedcba
    
    From the left:
    012345


在线程中消费


```python
import collections
import threading
import time

candle = collections.deque(range(5))


def burn(direction, nextSource):
    while True:
        try:
            next = nextSource()
        except IndexError:
            break
        else:
            print('{:>8}: {}'.format(direction, next))
            time.sleep(0.1)
    print('{:>8} done'.format(direction))
    return


left = threading.Thread(target=burn, args=('Left', candle.popleft))
right = threading.Thread(target=burn, args=('Right', candle.pop))

left.start()
right.start()

left.join()
right.join()
```

        Left: 0   Right: 4
    
       Right: 3
        Left: 1
       Right: 2
        Left done
       Right done


旋转


```python
import collections

d = collections.deque(range(10))
print('Normal        :', d)

d = collections.deque(range(10))
d.rotate(2)
print('Right rotation:', d)

d = collections.deque(range(10))
d.rotate(-2)
print('Left rotation :', d)
```

    Normal        : deque([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])
    Right rotation: deque([8, 9, 0, 1, 2, 3, 4, 5, 6, 7])
    Left rotation : deque([2, 3, 4, 5, 6, 7, 8, 9, 0, 1])


限制队列大小


```python
import collections
import random

# Set the random seed so we see the same output each time the script is run.
random.seed(1)

d1 = collections.deque(maxlen=3)
d2 = collections.deque(maxlen=3)

for i in range(5):
    n = random.randint(0, 100)
    print('n =', n)
    d1.append(n)
    d2.appendleft(n)
    print('D1:', d1)
    print('D2:', d2)
```

    n = 17
    D1: deque([17], maxlen=3)
    D2: deque([17], maxlen=3)
    n = 72
    D1: deque([17, 72], maxlen=3)
    D2: deque([72, 17], maxlen=3)
    n = 97
    D1: deque([17, 72, 97], maxlen=3)
    D2: deque([97, 72, 17], maxlen=3)
    n = 8
    D1: deque([72, 97, 8], maxlen=3)
    D2: deque([8, 97, 72], maxlen=3)
    n = 32
    D1: deque([97, 8, 32], maxlen=3)
    D2: deque([32, 8, 97], maxlen=3)


## namedtuple：带名字字段的元祖子类


```python
bob = ('Bob', 30, 'male')
print('Representation:', bob)

jane = ('Jane', 29, 'female')
print('\nField by index:', jane[0])

print('\nFields by index:')
for p in [bob, jane]:
    print('{} is a {} year old {}'.format(*p))
```

    Representation: ('Bob', 30, 'male')
    
    Field by index: Jane
    
    Fields by index:
    Bob is a 30 year old male
    Jane is a 29 year old female


定义


```python
import collections

Person = collections.namedtuple('Person', 'name age')

bob = Person(name='Bob', age=30)
print('\nRepresentation:', bob)

jane = Person(name='Jane', age=29)
print('\nField by name:', jane.name)

print('\nFields by index:')
for p in [bob, jane]:
    print('{} is {} years old'.format(*p))
```

    
    Representation: Person(name='Bob', age=30)
    
    Field by name: Jane
    
    Fields by index:
    Bob is 30 years old
    Jane is 29 years old



```python
import collections

Person = collections.namedtuple('Person', 'name age')

pat = Person(name='Pat', age=12)
print('\nRepresentation:', pat)

# 不可变类型
pat.age = 21
```

    
    Representation: Person(name='Pat', age=12)



    ---------------------------------------------------------------------------

    AttributeError                            Traceback (most recent call last)

    <ipython-input-36-109d38f441b3> in <module>
          7 
          8 # 不可变类型
    ----> 9 pat.age = 21
    

    AttributeError: can't set attribute


非法字段名


```python
import collections

try:
#     class是关键字
    collections.namedtuple('Person', 'name class age')
except ValueError as err:
    print(err)

try:
#     出现重复的命名
    collections.namedtuple('Person', 'name age age')
except ValueError as err:
    print(err)
```

    Type names and field names cannot be a keyword: 'class'
    Encountered duplicate field name: 'age'



```python
import collections

# 设置rename参数，以对非法字段重新命名
with_class = collections.namedtuple(
    'Person', 'name class age',
    rename=True)
print(with_class._fields)

two_ages = collections.namedtuple(
    'Person', 'name age age',
    rename=True)
print(two_ages._fields)
```

    ('name', '_1', 'age')
    ('name', 'age', '_2')


指定属性


```python
import collections

Person = collections.namedtuple('Person', 'name age')

bob = Person(name='Bob', age=30)
print('Representation:', bob)
print('Fields:', bob._fields)
```

    Representation: Person(name='Bob', age=30)
    Fields: ('name', 'age')



```python
import collections

Person = collections.namedtuple('Person', 'name age')

bob = Person(name='Bob', age=30)
print('Representation:', bob)
print('As Dictionary:', bob._asdict())
```

    Representation: Person(name='Bob', age=30)
    As Dictionary: OrderedDict([('name', 'Bob'), ('age', 30)])



```python
import collections

Person = collections.namedtuple('Person', 'name age')

bob = Person(name='Bob', age=30)
print('\nBefore:', bob)
bob2 = bob._replace(name='Robert')
print('After:', bob2)
print('Same?:', bob is bob2)
```

    
    Before: Person(name='Bob', age=30)
    After: Person(name='Robert', age=30)
    Same?: False


## OrderedDict：记住向字典中增加键的顺序


```python
import collections

print('Regular dictionary:')
d = {}
d['a'] = 'A'
d['b'] = 'B'
d['c'] = 'C'

for k, v in d.items():
    print(k, v)

print('\nOrderedDict:')
d = collections.OrderedDict()
d['a'] = 'A'
d['b'] = 'B'
d['c'] = 'C'

for k, v in d.items():
    print(k, v)
```

    Regular dictionary:
    a A
    b B
    c C
    
    OrderedDict:
    a A
    b B
    c C


相等性


```python
import collections

print('dict       :', end=' ')
d1 = {}
d1['a'] = 'A'
d1['b'] = 'B'
d1['c'] = 'C'

d2 = {}
d2['c'] = 'C'
d2['b'] = 'B'
d2['a'] = 'A'

print(d1 == d2)

print('OrderedDict:', end=' ')

d1 = collections.OrderedDict()
d1['a'] = 'A'
d1['b'] = 'B'
d1['c'] = 'C'

d2 = collections.OrderedDict()
d2['c'] = 'C'
d2['b'] = 'B'
d2['a'] = 'A'

print(d1 == d2)
```

    dict       : True
    OrderedDict: False


重排


```python
import collections

d = collections.OrderedDict(
    [('a', 'A'), ('b', 'B'), ('c', 'C')]
)

print('Before:')
for k, v in d.items():
    print(k, v)

d.move_to_end('b')

print('\nmove_to_end():')
for k, v in d.items():
    print(k, v)

d.move_to_end('b', last=False)

print('\nmove_to_end(last=False):')
for k, v in d.items():
    print(k, v)
```

    Before:
    a A
    b B
    c C
    
    move_to_end():
    a A
    c C
    b B
    
    move_to_end(last=False):
    b B
    a A
    c C


## collections.abc：容器的抽象基类

Abstract Base Classes


|Class	|Base Class(es)	|API Purpose|
|:-:|:-:|:-:|
|Container	        |&nbsp;                     |基本容器操作符，如 in操作符          |
|Hashable	        |&nbsp;                     |增加了散列支持，可以为容器实例提供散列值|
|Iterable	        |&nbsp;                     |可以在容器内容上创建一个迭代器|
|Iterator	        |Iterable                   |这是容器内容上的一个迭代器|
|Generator      	|Iterator                   |为迭代器扩展了PEP 342的生成器协议|
|Sized	 	        | &nbsp;                    |添加容器的方法，知道它们有多大|
|Callable	        | &nbsp;                    |对于可以作为函数调用的容器|
|Sequence	        |Sized, Iterable, Container |支持检索单个项目，迭代和更改项目的顺序|
|MutableSequence	|Sequence                   |支持在创建实例后添加和删除项目|
|ByteString	        |Sequence	                |合并的API bytes和bytearray|
|Set	            |Sized, Iterable, Container |支持设置操作，例如交集和并集|
|MutableSet         |&nbsp;                     |添加在创建后操纵设置内容的方法|
|Mapping	        |Sized, Iterable, Container |定义使用的只读API dict|
|MutableMapping	    |Mapping	                |定义在创建映射后操作映射内容的方法|
|MappingView	    |Sized                      |定义用于从迭代器访问映射的视图API|
|ItemsView	        |MappingView, Set           |视图API的一部分|
|KeysView	        |MappingView, Set           |视图API的一部分|
|ValuesView	        |MappingView                |视图API的一部分                                |
|Awaitable	    	| &nbsp;                    |可以在await表达式中使用的对象的API ，例如协同程序   |
|Coroutine	        |Awaitable                  |用于实现协程协议的类的API                      |
|AsyncIterable	    | &nbsp;                    |与async for（PEP 492中定义）兼容的iterable的API|
|AsyncIterator	    |AsyncIterable              |异步迭代器的API|
