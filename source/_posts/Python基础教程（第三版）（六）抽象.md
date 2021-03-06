---
title: Python基础教程（第三版）（六）抽象
date: 2019-06-07 21:55:05
tags:
- 学习笔记
- python基础
categories:
- 理论基础
---

*一个菜鸡的挣扎*
*就总结下*
and *如果有大佬不小心看到了发现了错误，就欢迎指正*

# 6.1懒惰是一种美德
通过创建函数以调用之可以减少代码量

# 6.2 抽象和结构

> 抽象是程序能够被人理解的关键所在（无论对编写程序还是阅读来说，这都至关重要）

函数封装了人不需要关心的实现细节，从而更容易被使用和理解
# 6.3 自定义函数

##  6.3.1 给函数编写文档
在def后面添加字符串，相当于给整个函数添加注释，以确保被人理解
 __doc_ _是函数的一个属性，可用它来访问函数的文档字符串
我自己的练习：

```python
def lalala(x):
    """就瞎写的"""
    print(x)

print(lalala.__doc__)
```
结果：
![运行结果](17108100-67e3852c05c06c7c.png)
这里书上说用单引号，但是pycharm中会有提示，要让用连续的三个双引号
help()函数可访问有关函数的信息，其中会包含函数的文档字符串

```python
In [1]: help(round)
Help on built-in function round in module builtins:

round(...)
    round(number[, ndigits]) -> number

    Round a number to a given precision in decimal digits (default 0 digits).
    This returns an int when called with one argument, otherwise the
    same type as the number. ndigits may be negative.
```
## 6.3.2 其实并不是函数的函数
就是有的函数没有return 或者return后面没有指定值，这么做将会返回None
# 6.4 参数魔法
## 6.4.1 值从哪里来 略

## 6.4.2 我能修改参数吗
可变参数可以，不可变参数不可以
如果不想让可变参数的值在调用函数后发生变化，可以向函数中传递切片或者用copy()传递副本（如果有copy()的话）
自己的练习：

```python
def change(n):
    n[0] = 'a'


x = [1, 2, 3]
```
```python
change(x)
print(x)
```
结果;
![](17108100-4c15f297e1019ed1.png)

```python
change(x[:])
print(x)
```
结果：
![](17108100-06682b1d3f3a17d1.png)

```python
change(x.copy())
print(x)
```
结果：
![](17108100-7d8ec6306fb82aa0.png)
如果想让不可变参数发生变化，可以使之重新指向返回的值，或者干脆把它放在列表中

## 6.4.3 关键字参数和默认值
这个部分之前看过了，少总结下

> 像这种用名称指定的参数称为关键字参数，主要优点是有助于澄清各个参数的作用。

关键字参数在传入时的顺序与定义时的不一样也没关系，反正有关键字程序不会认错
可以为关键字参数设置默认值，如果在调用该参数没有传入，就使用默认值
**位置参数不可定义在关键字参数后面！**
关键字好像只能是字符串，我的试验：

```python
def test(*a, **b):
    print(a, b)

b = {1: 'a', 2:'b'}
test(1, **b)
```
然后会报错：

![](17108100-e6ca9fbc25ff2eae.png)

## 6.4.4 收集参数
带星号的参数会收集多余的值，放在元组中
**一个星号不会收集关键字参数！若想收集，可带两个星号，这样会得到字典而非数组**
位置参数最好不要放在星号参数后面，虽然指定名称也可以调用
## 6.4.5 分配参数

类似于序列解包，可大致理解为与收集参数相反的操作

# 6.5 作用域

- vars()函数 ，返回作用域中看不见的字典，最好不要使用它修改字典中的值，因为其结果是不确定的
- 在函数中读取全局变量而非访问它，一般不会造成任何问题，但这样通常会不小心写出Bug
- 如果局部变量与全局变量重名，必要时可使用globals（）['变量名']来访问它，globals()类似于vars（），也返回一个字典
- 在函数中定义变量时，可在前面加入global 来声明它是全局变量
- 作用域嵌套，可在函数中再定义函数，并用return返回这个函数

**在函数中访问全局变量：**
```python
# 在函数中访问全局变量
y = '全局'
def test(x):
    print(x+y)
test('局部')
```
结果：![在函数中访问全局变量](17108100-743464065e7b5bbf.png)
**PS：做作业时发现np.array不用gobal关联也会变化，于是猜测，可变变量似乎不用关联也会变化**

**如果局部变量与全局变量重名：**
```python
# 如果局部变量与全局变量重名
y = '全局'
def test(y):
    print(y+globals()['y'])
test('局部')
```
结果同上
**重新关联全局变量:**
```python
# 重新关联全局变量
y = '全局'
def test():
    global y
    y = '我变了'
test()
print(y)
```
结果：![重新关联全局变量](17108100-a52362afc531f6e6.png)
**关于作用域嵌套：**
```python
In [2]: def first(x):
   ...:     def second(y):
   ...:         return y**x
   ...:     return second
   ...: fun = first(2)
   ...: fun(2)
Out[2]: 4
```
# 6.6 递归
这个就不说了
# 函数式编程
- 主要说了map，filter，reduce，lambda
- 在较新的python版本中，可以用列表推导代替map和filter
- map:对列表中所有元素执行函数
```python
In [3]: list(map(str,range(10)))
Out[3]: ['0', '1', '2', '3', '4', '5', '6', '7', '8', '9']

In [4]: # 与下面等价

In [5]: [str(i) for i in range(10)]
Out[5]: ['0', '1', '2', '3', '4', '5', '6', '7', '8', '9']
```
- filter: 对序列中元素执行函数，若为结果真，把它添加到最终要返回的列表中
```python
In [6]: def  fun(x):
   ...:     return x.isalnum()
   ...: list(filter(fun,['fdas','fdsa3','1*&']))
Out[6]: ['fdas', 'fdsa3']
# 与[x for x in [....] if x.isalnum] 等价
```
- reduce: 这个还是看例子
```python
In [1]: from functools import reduce
In [2]: reduce(lambda x,y: x+y, [1,2,3,4,5,6,7,8,9])
Out[2]: 45 
```
lambda的作用很明显了
## 补充：

> 通常，不能给外部作用域内的变量赋值，但如果一定要这样做，可使用关键字nonlocal。这个关键字的用法与global很像，让你能够给外部作用域（**非全局作用域**）内的变量赋值。

# 总结
抽象、函数定义、函数传参、作用域、函数式编程（主要是函数式编程工具，用以代替定义函数）
完了，希望不是在做无用功

