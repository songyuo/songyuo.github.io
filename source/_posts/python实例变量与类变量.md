---
title: python实例变量与类变量
date: 2019-09-16 16:26:40
tags:
- python基础
categories:
- 理论基础
---

简单记一下：

- 类变量绑定到类，实例变量绑定到具体实例
- 类变量在类中的函数体外定义，实例变量一般在\_\_init\_\_()中用self定义，但是似乎也可以定义在\_\_init\_\_()之外的函数体中
- 类变量可以通过类名来访问，也可以通过实例来访问，实例变量只能通过具体实例访问
- 存疑：如果类变量在具体实例中重新赋值（在类中或者类外），那么就会产生一个同名的实例变量绑定到该实例，类变量仍然不变
- 存疑：如果实例通过调用类变量改变它的值而不是对其重新赋值，那么类变量就会发生改变

# 实验

```python
class Test:
    a = 1
    b = 1
    c = [1]
    d = [1]
    e = '1,2,3'

    def __init__(self):
        self.a = 2
 
if __name__ == '__main__':
    demo = Test()
```

## 1. 在类的定义块中覆盖类变量：

```python
    print(Test.a)
    print(demo.a)
    demo.a = 123
    print(Test.a)
    print(demo.a)
    Test.a = 1234
    print(Test.a)
    print(demo.a)
```

结果：

```python
1
2
1
123
1234
123
```

- 用self定义重名变量后，会生成一个重名实例变量，通过实例变量访问会访问到重名实例变量，通过类才能访问到类变量

- 用具体实例重新赋值不改变类变量的值，用类重新赋值不改变具体实例中的值

## 2. 在类的定义块之外覆盖类变量

覆盖不可变数据类型：

```python
    print(Test.b)
    print(demo.b)
    demo.b = 123
    print(Test.b)
    print(demo.b)
```

结果：

```python
1
1
1
123
```

覆盖可变数据类型：
```pyton
    demo.c = 123
    print(Test.c)
    print(demo.c)
```

```python
[1]
123
```



## 3. 用实例调用类变量改变其值

调用可变数据类型：

```python
    demo.d.append(2)
    print(Test.d)
```

结果：

```python
[1, 2]
```

调用不可变数据类型：

```python
    demo.e.split(',')
    print(Test.e)

```

结果：

```python
1,2,3
```

并没有变化