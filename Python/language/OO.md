## 类函数, 成员函数, 静态函数

问题1: 如何在一个类中定义一些常量，每个对象都可以方便访问这些常量而不用重新构造?



问题2: 如果一个函数不涉及到访问修改这个类的属性，而放到类外面有点不恰当，怎么做才能更优雅呢？



问题3: 既然类是一群相似的对象的集合，那么可不可以是一群相似的类的集合呢？



```python

class Document():
    
    WELCOME_STR = 'Welcome! The context for this book is {}.'
    
    def __init__(self, title, author, context):
        print('init function called')
        self.title = title
        self.author = author
        self.__context = context
    
    # 类函数
    @classmethod
    def create_empty_book(cls, title, author):
        return cls(title=title, author=author, context='nothing')
    
    # 成员函数
    def get_context_length(self):
        return len(self.__context)
    
    # 静态函数
    @staticmethod
    def get_welcome(context):
        return Document.WELCOME_STR.format(context)


empty_book = Document.create_empty_book('What Every Man Thinks About Apart from Sex', 'Professor Sheridan Simove')


print(empty_book.get_context_length())
print(empty_book.get_welcome('indeed nothing'))

########## 输出 ##########

init function called
7
Welcome! The context for this book is indeed nothing.
```

一般而言，静态函数可以用来做一些简单独立的任务，既方便测试，也能优化代码结构。

而类函数的第一个参数一般为 ==cls==，表示必须传一个类进来。类函数最常用的功能是实现不同的 init 构造函数，

## 抽象函数, 抽象类



```python

from abc import ABCMeta, abstractmethod

class Entity(metaclass=ABCMeta):
    @abstractmethod
    def get_title(self):
        pass

    @abstractmethod
    def set_title(self, title):
        pass

class Document(Entity):
    def get_title(self):
        return self.title
    
    def set_title(self, title):
        self.title = title

document = Document()
document.set_title('Harry Potter')
print(document.get_title())

entity = Entity()

########## 输出 ##########

Harry Potter

---------------------------------------------------------------------------
TypeError                                 Traceback (most recent call last)
<ipython-input-7-266b2aa47bad> in <module>()
     21 print(document.get_title())
     22 
---> 23 entity = Entity()
     24 entity.set_title('Test')

TypeError: Can't instantiate abstract class Entity with abstract methods get_title, set_title
```

