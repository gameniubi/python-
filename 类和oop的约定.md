# 类和oop的约定

类是一个很好的聚合，但在编程中不一定使用类，我们不应将不需要的代码放入类中，优化类的代码，增强可读性

- srp单一职责原则(一个类只做一件事)
- 评估方法和代码单元适合性
- 找到重复或复制的代码将其拆分(DRY)

## 使用装饰器实现get，set操作

```python
class Square:

    def __init__(self):
        self._side = None

    @property
    def side(self):
        return self._side

    @side.setter
    def side(self, side: int):
        assert side >= 0, '边长不为负数'
        self._side = side

    @property
    def area(self):
        return self.side * self.side

    @side.deleter
    def side(self):
        self._side = 0


if __name__ == '__main__':
    square = Square()
    square.side = 10
    print(square.area)
    del square.side
    print(square.side, square.area)
    
    
#100
#0 0

```

##  公共属性与私有属性

在python中，python编辑器并没有严格要求公共属性、私有属性等区别。而只是使用命名规则进行语义上的约束

如value表示公有属性，_value为私有属性，__value为保护属性

## is 与 ==之间的差别

```python
a = [1, 2, 3]
b = [1, 2, 3]
c = a
print(a == b, b == c, a == c)  # 值比较
print(a is b, b is c, a is c)  # 地址比较
print(id(a), id(b), id(c))


/usr/bin/python3 /Users/bilisheep/program/pycharmProgram/test/1.py 
True True True
False False True
4312389952 4312273216 4312389952

进程已结束,退出代码0

```

注意要考虑小整数等的常量问题

## super()的使用

使用super辅助日志记录

```python
class Person:
    def __getattribute__(self, item):
        print("get attribute:", item)
        return super(Person, self).__getattribute__(item)

    def __setattr__(self, key, value):
        print("set key:", key, "value:", value)
        return super(Person, self).__setattr__(key, value)


person = Person()
person.name = "me"
print(person.name)





/usr/bin/python3 /Users/bilisheep/program/pycharmProgram/test/1.py 
set key: name value: me
get attribute: name
me

进程已结束,退出代码0

```



## MRO与super

在单继承的情况下，方法的调用顺序代码执行顺序很容易理解。但python是一个多继承语言，那么在多继承下如何理解

python使用c3算法来计算执行顺序，但依旧不建议使用多重继承。这会使代码变得更加复杂难以维护。我们可以使用组合进行拼接，增加代码可读性。

