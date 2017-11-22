
# 流畅的 Python

## 第一章 Python 数据模型


```python
# coding: utf-8
import sys

# 查看 Python 版本信息
print(sys.version_info)
print(sys.version)
```

    sys.version_info(major=3, minor=6, micro=1, releaselevel='final', serial=0)
    3.6.1 |Anaconda custom (64-bit)| (default, May 11 2017, 13:25:24) [MSC v.1900 64 bit (AMD64)]
    

### 1.1 一摞 Python 风格的纸牌


```python
# 展示如何实现 __getitem__ 和 __len__ 这两个特殊方法
import collections

Card = collections.namedtuple('Card', ['rank', 'suit'])

class FrenchDeck:
    ranks = [str(n) for n in range(2, 11)] + list('JQKA')
    suits = 'spades diamonds clubs hearts'.split()
    
    def __init__(self):
        self._cards = [Card(rank, suit) for suit in self.suits for rank in self.ranks]

    def __len__(self):
        return len(self._cards)

    def __getitem__(self, position):
        return self._cards[position]
```

#### namedtuple 用以构建只有少数属性但是没有方法的对象，比如数据库条目


```python
beer_card = Card('7', 'diamonds')
beer_card
```




    Card(rank='7', suit='diamonds')



#### 查看一叠牌有多少张


```python
deck = FrenchDeck()
len(deck)
deck._cards
```




    [Card(rank='2', suit='spades'),
     Card(rank='3', suit='spades'),
     Card(rank='4', suit='spades'),
     Card(rank='5', suit='spades'),
     Card(rank='6', suit='spades'),
     Card(rank='7', suit='spades'),
     Card(rank='8', suit='spades'),
     Card(rank='9', suit='spades'),
     Card(rank='10', suit='spades'),
     Card(rank='J', suit='spades'),
     Card(rank='Q', suit='spades'),
     Card(rank='K', suit='spades'),
     Card(rank='A', suit='spades'),
     Card(rank='2', suit='diamonds'),
     Card(rank='3', suit='diamonds'),
     Card(rank='4', suit='diamonds'),
     Card(rank='5', suit='diamonds'),
     Card(rank='6', suit='diamonds'),
     Card(rank='7', suit='diamonds'),
     Card(rank='8', suit='diamonds'),
     Card(rank='9', suit='diamonds'),
     Card(rank='10', suit='diamonds'),
     Card(rank='J', suit='diamonds'),
     Card(rank='Q', suit='diamonds'),
     Card(rank='K', suit='diamonds'),
     Card(rank='A', suit='diamonds'),
     Card(rank='2', suit='clubs'),
     Card(rank='3', suit='clubs'),
     Card(rank='4', suit='clubs'),
     Card(rank='5', suit='clubs'),
     Card(rank='6', suit='clubs'),
     Card(rank='7', suit='clubs'),
     Card(rank='8', suit='clubs'),
     Card(rank='9', suit='clubs'),
     Card(rank='10', suit='clubs'),
     Card(rank='J', suit='clubs'),
     Card(rank='Q', suit='clubs'),
     Card(rank='K', suit='clubs'),
     Card(rank='A', suit='clubs'),
     Card(rank='2', suit='hearts'),
     Card(rank='3', suit='hearts'),
     Card(rank='4', suit='hearts'),
     Card(rank='5', suit='hearts'),
     Card(rank='6', suit='hearts'),
     Card(rank='7', suit='hearts'),
     Card(rank='8', suit='hearts'),
     Card(rank='9', suit='hearts'),
     Card(rank='10', suit='hearts'),
     Card(rank='J', suit='hearts'),
     Card(rank='Q', suit='hearts'),
     Card(rank='K', suit='hearts'),
     Card(rank='A', suit='hearts')]




```python
deck[0]
```




    Card(rank='2', suit='spades')




```python
deck[-1]
```




    Card(rank='A', suit='hearts')



#### 随机抽取一张纸牌 random.choice


```python
from random import choice

choice(deck)
```




    Card(rank='J', suit='spades')




```python
choice(deck)
```




    Card(rank='A', suit='spades')



#### 切片


```python
deck[:3]
```




    [Card(rank='2', suit='spades'),
     Card(rank='3', suit='spades'),
     Card(rank='4', suit='spades')]



#### 迭代


```python
for card in deck[:3]:
    print(card)
```

    Card(rank='2', suit='spades')
    Card(rank='3', suit='spades')
    Card(rank='4', suit='spades')
    

#### 反向迭代


```python
for card in reversed(deck[:3]):
    print(card)
```

    Card(rank='4', suit='spades')
    Card(rank='3', suit='spades')
    Card(rank='2', suit='spades')
    

#### 如果一个集合类型没有实现 __contains__ 方法，那么 in 运算符就会按顺序做一次迭代搜索


```python
Card('Q', 'hearts') in deck
```




    True




```python
Card('Q', 'beasts') in deck
```




    False



#### 排序


```python
suit_values = dict(spades=3, hearts=2, diamonds=1, clubs=0)
```


```python
def spades_high(card):
    rank_value = FrenchDeck.ranks.index(card.rank)
    return rank_value * len(suit_values) + suit_values[card.suit]
```


```python
for card in sorted(deck, key=spades_high):
    print(card)
```

    Card(rank='2', suit='clubs')
    Card(rank='2', suit='diamonds')
    Card(rank='2', suit='hearts')
    Card(rank='2', suit='spades')
    Card(rank='3', suit='clubs')
    Card(rank='3', suit='diamonds')
    Card(rank='3', suit='hearts')
    Card(rank='3', suit='spades')
    Card(rank='4', suit='clubs')
    Card(rank='4', suit='diamonds')
    Card(rank='4', suit='hearts')
    Card(rank='4', suit='spades')
    Card(rank='5', suit='clubs')
    Card(rank='5', suit='diamonds')
    Card(rank='5', suit='hearts')
    Card(rank='5', suit='spades')
    Card(rank='6', suit='clubs')
    Card(rank='6', suit='diamonds')
    Card(rank='6', suit='hearts')
    Card(rank='6', suit='spades')
    Card(rank='7', suit='clubs')
    Card(rank='7', suit='diamonds')
    Card(rank='7', suit='hearts')
    Card(rank='7', suit='spades')
    Card(rank='8', suit='clubs')
    Card(rank='8', suit='diamonds')
    Card(rank='8', suit='hearts')
    Card(rank='8', suit='spades')
    Card(rank='9', suit='clubs')
    Card(rank='9', suit='diamonds')
    Card(rank='9', suit='hearts')
    Card(rank='9', suit='spades')
    Card(rank='10', suit='clubs')
    Card(rank='10', suit='diamonds')
    Card(rank='10', suit='hearts')
    Card(rank='10', suit='spades')
    Card(rank='J', suit='clubs')
    Card(rank='J', suit='diamonds')
    Card(rank='J', suit='hearts')
    Card(rank='J', suit='spades')
    Card(rank='Q', suit='clubs')
    Card(rank='Q', suit='diamonds')
    Card(rank='Q', suit='hearts')
    Card(rank='Q', suit='spades')
    Card(rank='K', suit='clubs')
    Card(rank='K', suit='diamonds')
    Card(rank='K', suit='hearts')
    Card(rank='K', suit='spades')
    Card(rank='A', suit='clubs')
    Card(rank='A', suit='diamonds')
    Card(rank='A', suit='hearts')
    Card(rank='A', suit='spades')
    

#### 分析

通过实现 __len__ 和 __getitem__ 这两个特殊方法，FrenchDeck 就跟一个 Python 自有的序列数据类型一样，可以体现出 Python 的核心语言特性（例如迭代和切片）。同时这个类还可以用于标准库中诸如 random.choice, reversed 和 sorted 这些函数。另外，对合成的运用使得 __len__ 和 __getitem__ 的具体实现可以代理给 self._cards 这个列表对象。

### 1.2 如何使用特殊方法

通过内置函数（例如 len, iter, str 等）来使用特殊方法是最好的选择，这些内置函数不仅会调用特殊方法，通常还会提供额外的好处，而且对于内置的类来说，它们的速度更快。


```python
# 一个简单的二维向量类
# coding: utf-8

from math import hypot

class Vector:
    def __init__(self, x=0, y=0):
        self.x = x
        self.y = y
        
    def __repr__(self):
        """内置函数，把一个对象用字符串的形式表达出来以便确认。
        """
        return "Vector(%r, %r)" % (self.x, self.y)
    
    def __abs__(self):
        # hypot() 返回欧几里德范数 sqrt(x*x + y*y)
        return hypot(self.x, self.y)
    
    def __bool__(self):
        # return bool(abs(self))
        return bool(self.x or self.y)
    
    def __add__(self, other):
        x = self.x + other.x
        y = self.y + other.y
        return Vector(x, y)
    
    def __mul__(self, scalar):
        return Vector(self.x * scalar, self.y * scalar)
```


```python
v1 = Vector(2, 4)
v2 = Vector(2, 1)
v1 + v2
```




    Vector(4, 5)




```python
v = Vector(3, 4)
abs(v)
```




    5.0




```python
v * 3
```




    Vector(9, 12)




```python
abs(v * 3)
```




    15.0




```python
print(bool(Vector(0, 0)))
print(bool(Vector(0, 1)))
```

    False
    True
    

__repr__ 和 __str__ 区别在于，后者是 str 函数被使用，或是在用 print 函数打印一个对象的时候才被调用的，并且它返回的字符串对终端用户更好。
__add__ : +, __mul__: * 中缀运算符的基本原则就是不改变操作对象，而是产出一个新的值。默认情况下，我们自己所定义的类的实例总被认为是真的，除非这个类对 bool 或者 len 函数有自己的实现。bool(x) 的背后是调用 x.bool()的结果。

### 1.3 特殊方法一览

1.跟运算符无关；2.跟运算符相关
增量赋值运算符则是一种把中缀运算符变为赋值运算符的捷径。

### 1.4 为什么 len 不是普通方法

1.实用胜于纯粹。2.不能让特例特殊到开始破坏既定规则。

### 1.5 本章小结

1.通过实现特殊方法，自定义数据类型可以表现得跟内置类型一样，从而让我们写出更具表达力的代码——更具 Python 风格的代码。

2.Python 对象的一个基本要求就是它得有合理的字符串表示形式。__repr__ 方便我们调试和记录日志，__str__ 用来展示给终端用户。

3.对序列数据类型的模拟是特殊方法用的最多的地方。

4.Python 通过运算符重载这一模式提供了丰富的数值类型。
