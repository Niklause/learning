## map()函数

map() 是python内置的高级函数，它接收一个函数f和一个list，并通过把函数f依次作用在list的每个元素上，得到一个新的list并返回。

例如对于list[1, 2, 3, 4, 5, 6, 7, 8, 9]

如果希望把list的每个元素都作为平方，
我们之前的做法是

``` python
def f(x):
	return x * x
for x in range(0,len(list)):
	list[x] = f(list[x])
```

如果用map()函数：

则只需要

``` python
def f(x):
	return x * x
print map(f,[1,2,3,4,5,6,7,8,9]) 
# 输出结果 [1,4,9,16,25,36,49,64,81]
# tips:map()函数不改变原有的list，而是返回一个新的list 相当于对原list进行了深复制
```

例子：

假设用户输入的英文名字不规范，没有按照首字母大写，后续字母小写的规则，请利用map()函数，把一个list（包含若干不规范的英文名字）变成一个包含规范英文名字的list：

输入：['adam', 'LISA', 'barT']
输出：['Adam', 'Lisa', 'Bart']

``` python
def format_name(s):
	return s.capitalize()
print map(format_name, ['adam', 'LISA', 'barT'])
```

结果

```
>>> 
['Adam', 'Lisa', 'Bart']
```
同时 map()方法中可以加入多个参数

```python

def fun(x,y,z):
	return x*y*z
print map(fun,[1,2,3],[1,2,3],[1,2,3])

# 输出结果 [1,8,27]
```

### lambda表达式

lambda表达式 类似于一个匿名函数，通常是在需要一个函数，但是又不想费神去明明一个函数的情况下使用

``` python
s = [1,2,3]
print map(lambda x:x+1,s)

# 输出结果(2,3,4)
```

我们来看 lambda 这是一个关键字 表示是一个匿名函数

x :x +1 冒号之前的相当于fun(x)的参数，即入参

冒号之后的 x + 1 相当于返回参数 即return x + 1

tips:lambda表达式不仅可以用在这里很多地方都可以使用 如果以后遇到会再详细说明

``` python
>>> f = lambda x,y,z:x+y+z
>>> f(1,2,3)
6
```

```
L = [(lambda x: x**2),  
    (lambda x: x**3),  
    (lambda x: x**4)]  
print L[0](2),L[1](2),L[2](2)
# 4,8,16
```

tips: 有些地方可能会不显示 lambda但是 尽量不要这么用

```python
print [x+y for x in range(5) if x%2 == 0 for y in range(10) if y%2 ==1]
```

这个代码可以这么认为

``` python
list = []
for x in range(5):
	for y in range(10):
		if (x%2 ==0 and y %2 ==1):
			list.add(x + y)
print list
```

如上
