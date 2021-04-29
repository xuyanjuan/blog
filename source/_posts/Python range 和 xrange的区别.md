---
title: Python range和xrange的区别

date: 2018/5/15 10:46:25

updated: 2018/5/15 10:46:25

categories:
 - python
 
tags:
 - python
---
![20200826102636292.jpg](https://i.loli.net/2021/02/01/mGWJefdCR9EaIlk.jpg)

## Python range和xrange的区别

- Python3 range() 函数返回的是一个可迭代对象（类型是对象），而不是列表类型， 所以打印的时候不会打印列表。

- Python3 list() 函数是对象迭代器，可以把range()返回的可迭代对象转为一个列表，返回的变量类型为列表。

- Python2 range() 函数返回的是列表。

- Python2 xrange() 用法与range完全相同，所不同的是生成的不是一个数组，而是一个生成器。

**Python3中取消了 Xrange()的使用，结合成为了 range()**

## range
>函数说明：range([start,] stop[, step])，根据start与stop指定的范围以及step设定的步长，生成一个序列。

**range示例**
```
>>> range(5)
[0, 1, 2, 3, 4]
>>> range(1,5)
[1, 2, 3, 4]
>>> range(0,6,2)
[0, 2, 4]
```
## xrange
>函数说明：用法与range完全相同，所不同的是生成的不是一个数组，而是一个生成器。

xrange示例
```
>>> xrange(5)
xrange(5)
>>> list(xrange(5))
[0, 1, 2, 3, 4]
>>> xrange(1,5)  
xrange(1, 5)
>>> list(xrange(1,5))
[1, 2, 3, 4]
>>> xrange(0,6,2)
xrange(0, 6, 2)
>>> list(xrange(0,6,2))
[0, 2, 4]
```
## 区别
**由上面的示例可以知道：要生成很大的数字序列的时候，用xrange会比range性能优很多，因为不需要一上来就开辟一块很大的内存空间，这两个基本上都是在循环的时候用**
```
for i in range(0, 100): 
print i 
for i in xrange(0, 100): 
print i 
```
**这两个输出的结果都是一样的，实际上有很多不同，range会直接生成一个list对象**
```
a = range(0,100) 
print type(a) 
print a 
print a[0], a[1] 
```
**输出结果**
```
<type 'list'>
[
0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 
26, 27, 28, 29, 30, 31, 32, 33, 34, 35, 36, 37, 38, 39, 40, 41, 42, 43, 44, 45, 46, 47, 48, 49, 50, 
51, 52, 53, 54, 55, 56, 57, 58, 59, 60, 61, 62, 63, 64, 65, 66, 67, 68, 69, 70, 71, 72, 73, 74, 75, 
76, 77, 78, 79, 80, 81, 82, 83, 84, 85, 86, 87, 88, 89, 90, 91, 92, 93, 94, 95, 96, 97, 98, 99
]
0 1
```
**而xrange则不会直接生成一个list，而是每次调用返回其中的一个值**
```
a = xrange(0,100) 
print type(a) 
print a 
print a[0], a[1] 
```
**输出结果**
```
<type 'xrange'>
xrange(100)
0 1
```
**所以xrange做循环的性能比range好，尤其是返回很大的时候，尽量用xrange吧，除非你是要返回一个列表**


