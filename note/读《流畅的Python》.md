

#### 什么样的自定义类算是序列

> “Python 已经内置了从一个序列中随机选出一个元素的函数 random.choice”
>
> 摘录来自: [巴西] Luciano Ramalho. “流畅的Python”。 iBooks. 

前文是在讲魔术方法，自定义了一个类

```python
import collections
Card = collections.namedtuple('Card',['rank','suit'])
class FrenchDeck:
    ranks = [str(n) for n in range(2,11)] + list('JQKA')
    suits = 'spades diamonds clubs hearts'.split()

    def __init__(self):
        self._cards = [Card(rank,suit) for suit in self.suits for rank in self.ranks]

    def __len__(self):
        return len(self._cards)

    def __getitem__(self, item):
        return self._cards[item]
```

可以创建一个空类，或者从该类注释掉部分代码，最后发现三个函数都不可少。

那我可以这样总结，自定义序列需要声明`__len__`和`__getitem__`函数

*题外话*

```python
self._cards = [Card(rank,suit) for suit in self.suits for rank in self.ranks]
```

列表推导式真是太妙了，而且还是双层循环



