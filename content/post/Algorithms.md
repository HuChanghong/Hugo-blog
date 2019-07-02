---
title: "Algorithms"
date: 2019-07-02T14:37:30+08:00
draft: false
tags: ["Python","Algorithms"]
comments: true
share: true
---


[TOC]
# Algorithms

In this notebook, you will learn the following:

1. Big O Notation
2. Recursion
    - Fibonacci
    - Binary Search

## Array, List, String, Set, Dictionary
List Dictionary 这些都可以看做是一个数据的容器，他们存储数据的方式有不同。
对于数据，我们常用的操作有，插入、删除、查找 等。
不同的存储方式会导致在做这些操作的时候，时间上会有差别。这就是数据结构。

那么什么是算法？如何评估算法？

算法就是解决问题

* 问题是什么 Problem
* 我们有什么 Input
* 我们想要得到什么 Output
* 尝试最简单的方法 Simple Solution
* 看看如何改进 Develop Incrementally

Algorithm Analysis

| 时间复杂度 | 表示 |
| --- | --- |
| constent | O(1)|
| logarithmic | O(log(n)) |
| linear | O(n) |
| log linear | O(n\*log(n)) |
| quadratic | O(n^2) | 
| cubic | O(n^3) |
| exponential | O(2^n) |


## Big O Notation
大O表示法

>把我们运行的时间 running time 与 n 的增长关系用一个曲线画出来，如果存在 这样一个函数 k\*f(n)的曲线 当 n >= n0 时，有 k\*f(n) >  running time ，我们就说这个算法的运行时间是 Big O of f(n) 。

### $O(1)$
```python
def square(x):
    return x * x
```

### $O(n)$
```python
def find_max(l):
    import time
    start = time.time()
    if l == None:
        return None
    mx = l[0]
    for n in l:
        if n > mx:
            mx = n
            
    t = time.time() - start
    
    return mx, len(l), t
```

### $O(n^2)$




### Time Complexity & Space Complexity


时间复杂度&空间复杂度



## Searching and Sorting

比如：

**x > 1 , 求x的平方根y, 0 < y < x**

1. 设Low为0，High为x
2. 假设Guess是(Low+High)/2，如果Guess的平方非常接近x，那么y=g
3. 若，g\*g < x，L设定为Guess，然后重复第二步
4. 否则，g\*g > x，H设定为Guess，然后重复第二步

>按步骤，告诉计算机解决问题的方法

Q:比如最常见的排序，如何排序：

A:简单，用list.sort()  用 sorted(list)

Q:那么时间复杂度是多少呢： 

A:我知道，python里 sort 是O(n\*log(n))

Q:那么为什么是O(n\*log(n))呢？

A:额。。。

好吧我们下面来学习这些内容：

* Sort
* Binary Search
* Divide and Conquer
* Two Pointers
* Sliding Window
* Others
* Greedy
* Dynamic Programming \*


### Bubble Sort
冒泡排序
每次循环把当前需要比较的各项中最大的“冒泡”到最后
```python
# 主要逻辑
# 参数nums是一个list
for i in range(len(nums):
    for j in range(len(nums) - i -1):
        if a[j] > a[j+1]:
            a[j], a[j+1] = a[j+1], a[j]
```
时间复杂度： O(n^2)

>写完整：

```python
def _bubble_sort(array: list, reverse=False):
    for i in range(len(array)):
        # Get (i+1) largest in the correct position
        for j in range(1, len(array) - i):
            if array[j] < array[j - 1]:
                # swap
                array[j], array[j - 1] = array[j - 1], array[j]
    if reverse:
        # nums = nums[::-1]  # why this is not working?
        array.reverse()
    return len(array)
    
def bubble_sorted(array: list, array=False) -> list:
    ''' Bubble Sort '''
    array_copy = list(array)
    _bubble_sort(array_copy, reverse=reverse)
    return array_copy
```
时间复杂度的区分：
如何优化达到better performence

| case | Big O |
| --- | --- |
| worst case | O(n^2) |
| avg case | O(n^2) |
| best case | O(n) |

>如何让best case 达到 O(n)

什么情况是best case，当list 是一个 sorted list 时，就是best case。

意味着我们无需对list做任何操作，我们看看操作发生在哪儿？

```python
for j in range(1, len(array) - i): 
        if array[j] < array[j - 1]:
            array[j], array[j - 1] = array[j - 1], array[j]
```
我们在这里可以加入一个boolean型的标记，给标记一个初始值，如果发生了操作，也就是进入这个if的时候，改变标记的值。

换言之，如果在第一个循环，i = 0 的时候，遍历list都没做任何操作，那么就说明这是一个 sorted list，意味着不需要再进行后续的任何循环，可以break，时间复杂度降为 O(n)。当然，在任意一次内部循环结束后发现 is_sorted 都会触发break，同样会提高算法的效率。

```python
for i in range(len(array)): # n pass
        is_sorted = True # initialize is_sorted
        for j in range(1, len(array) - i): 
            if array[j] < array[j - 1]:
                # swap
                array[j], array[j - 1] = array[j - 1], array[j]
                is_sorted = False
        if(is_sorted): break             
```

### Insertion Sort
插入排序

将list中的项一个一个拿出来，然后插入一个 sort list 中，当然这个 sort list 最初是空的。
有点像打扑克牌的时候摸牌，后摸的牌插入之前排好序的牌组中。

```python
# 主要逻辑
# a[i] 就是我拎在手上要插入的一张牌，先跟a[j=0](最小的)比较，所以是从左插入
# i 是从1 开始的，因为a[0]不需要插入，最初手上是空的，直接摸就可以
for i in range(1, len(list)):
    for j in range(i):
        if a[i] < a[j]:
            a[i], a[j] = a[j], a[i]
```
当然就好像打扑克的人是左撇子的时候会用另外的插入方法，可以左插入自然也可以右插入：
```python
for i in range(1, len(list)):
    # 右插入
    for j in range(i-1, -1, -1):
        if a[i] < a[j]:
            a[i], a[j] = a[j], a[i]
```
完整的代码：
```python
def insert_sort(array):
    for i in range(1, len(array)):
        for j in range(i):
            if array[i] < array[j]:
                array[i], array[j] = array[j], array[i]
    return len(array)
```

>对于小数组，比如20 个以下的， insert sort 的速度是很快的，所以常用于小数组的排序。虽然它的时间复杂度也是O(n^2)，但是实际运行要比冒泡排序快。为什么？



### Select Sort
选择排序

在list中设一个minindex，初始值为下标0，记录这个初始值为最小值，然后逐个去比较list中的值，发现更小的就记录下标，轮询完毕就把记录到的最终值，交换到minindex下标的位置。下一轮minindex+1

我把它称为”我指到你，你就自己跳到碗里来“

它不是每次发现更小的就立马交换位置，而是在每一轮中挑出最小值，然后交换到minindex的位置。

```python
# 主要逻辑
for i in range(len(array)):
    minidx = i
    for j in range(i+1, len(array)):
        if array[minidx] > array[j]:
            minidx = j
    array[i], array[minidx] = array[minidx], array[i]
```
### Merge Sort

归并排序

采用归并的操作，主要思想是分治法（divide and conquer），也就是递归。

归并就是把已有的 sorted list 合并，得到新的 sorted list。 一个 unsorted list 首先进行拆分，直到拆成两两一对的sublist（最小单元），每个sublist排序成为一个 sorted list ，然后合并（merge）。

>merge这个操作是怎么做的？

1. 两个sorted sublist 分别取一个element， a[i] 和 b[j]（当然是从第一个取（a[0],b[0]））进行比较，把较小的值写入一个临时list，这个值就是两个sorted sublist中元素的最小值。
2. 然后被选中的那个sublist下标后移（比如a[i] < b [j] ， 那么 a[i+1]），继续下一轮比较，直到其中一个数列的下标超过len。
3. 最后把另一个数列尚未被比较的下标的元素依次写入临时数组。排序完毕


```python
# merge的主要逻辑
c = []
i, j, k = 0, 0, 0
# 比较两个sublist
while i < len(a) and j < len(b):
    if a[i] < b[j]:
        c[k] = a[i]
        i += 1
        k += 1
    else:
        c[k] = b[j]
        k += 1
        j +=1
# 如果sublist a 没取完
while i < len(a):
    c[k] = a[i]
    i += 1
    k += 1
# 如果sublist b 没取完
while j < len(b):
    c[k] = b[j]
    j += 1
    k += 1
```

>写一个漂亮的 merge 函数：

```python
def _merge(a: list, b: list) -> list:
    c = []
    while len(a) > 0 and len(b) > 0:
        # 加入了相等的处理逻辑
        if a[0] <= b[0]:
            c.append(a[0])
            a.remove(a[0])
        else:
            c.append(b[0])
            b.remove(b[0])
    
    if len(a) == 0:
        c += b
    else:
        c += a
    return c
```



> divide这个操作是怎么做的？

这里就要用到数学归纳法了，没错，就是递归。

1. 对list进行切分取一个大约在中间的index， m = len(list) // 2 ，然后list 就被分为两个sublist（list[:m] 和 list[m:]）
2. 对两个sublist再分别进行第一步操作（递归调用），进入递归 --> 直到拆分成单个元素list时返回单元素list本身（递归都需要一个base）。
3. 对返回值（单元素list）进行 merge 操作，merge后的值返回给上一级递归（解递归）。直到把第一步的两个sublist 进行 merge 操作后返回一个完整的 sorted list 。

```python

def _merge_sorted(nums: list) -> list:
    # base
    if len(nums) <= 1:
        return nums
    # recursion
    m = len(nums) // 2
    a = _merge_sorted(nums[:m])
    b = _merge_sorted(nums[m:])
    return _merge(a, b)
```

> 最后我们把这两个函数做一个漂亮的打包，加上反向排序的功能，这两个函数名前都加上了 _ ，我是把它视为工厂函数的。

```python
# Wrapper
def merge_sorted(nums: list, reverse=False) -> list:
    nums = _merge_sorted(nums)
    if reverse:
        nums = nums[::-1]
    return nums
```

> merge sort 的时间复杂度是多少？

`O(n\*log(n))`

这个算法用到了递归，也是一个二叉树的层级划分，每个层级的时间复杂度也就是 merge 操作的时间复杂度是 O(n) 。层级是多少层呢，log(n) 层。 所以时间复杂度是  `O(n\*log(n))`

> 下面我们可以回答之前的问题了，`l.sort()` 和 `sorted()` 的时间复杂度为什么是 `O(n\*log(n))` ?

因为python用的sort 算法是 merge sort 和 insert sort 的混合。

主要使用 merge sort ，当 拆分到足够小时， 例如拆分到 len = 20 ， 使用 insert sort 。

> merge sort 的空间复杂度？

之前的 bubble sort /  insert sort / select sort 都没有使用更多额外的内存空间，所以他们的空间复杂度都是 O(1)。 merge sort 需要借用一个 len = n 临时数组，所以空间复杂度是`O(n)`。

### Quick Sort

快速排序

这里有两个概念：

* `pivot`  支点
* `partition`   分治

1. 首先把list的第一项`list[0]`作为`pivot`，定义一个下标 ` i `  为从左数除`list[0]`外第一个（初始为list[1]），定义一个下标 ` j ` 为从右数第一个（初始为list[-1]）
2. i标签向右移动，同时寻找到第一个比`pivot`项值大的元素，停在那里。list[i] > list[pivot] then stop
3. j标签向左移动，同时寻找到第一个比`pivot`项值小的元素，停在那里。list[j] > list[pivot] then stop
4. 交换i下标和j下标的元素的值  list[i], list[j] = list[j], list[i]
5. 进入下一轮移动寻找，直到 找到的 list[i] ， list[j]  的下标 i > j （交叉），此时停止互相交换，把此时j下标和pivot下标的元素值互换 list[pivot], list[j] = list[j], list[pivot]
6. 这时候会发现，此时list[pivot]已经被排在了正确的位置（左边的值都小于它，右边的值都大于它）
7. 对list[pivot]左边和右边的list分别再次进行从第一步开始的操作，进入递归。
8. `base` 当递归至 list[pivot] 左边和右边都只有一个元素，已经完成一个最小化的sorted list，然后解递归。

```python
# 一轮完整的 partition 逻辑
def partition(self, alist, first, last):
    pivotvalue = alist[first]
    leftmark = first + 1
    rightmark = last
    
    done = False
    while not done:
        while leftmark <= rightmark and alist[leftmark] < pivotvalue:
            leftmark += 1
        while rightmark >= leftmark and alist[rightmark] > pivotvalue:
            rightmark -= 1
            
        if leftmark > rightmark:
            done = True
        else:
            alist[leftmark], alist[rightmark] = alist[rightmark], alist[leftmark]
    
    alist[first], alist[rightmark] = alist[rightmark], alist[first]
    
    return rightmark
```

```python
# 相关的递归逻辑
def sort(self, alist):
    self.helper(alist, 0, len(alist)-1)
    
def helper(self, alist, first, last):
    if(first < last): # base
        splitpoint = self.partition(alist, first, last)
        # recursion
        self.helper(alist, first, splitpoint-1)
        self.helper(alist, splitpoint+1, last)
```

> 来看一个漂亮简洁的递归写法

注意：这种写法需要额外的空间！
```python
# factory
def _quick_sorted(nums: list) -> list:
    # base
    if len(nums) <= 1:
        return nums
    
    # recursion
    pivot = nums[0]
    left_nums = _quick_sorted(i for i in nums[1:] if nums[i] < pivot)
    right_nums = _quick_sorted(i for i in nums[1:] if nums[i] > pivot)
    
    return left_nums + [pivot] + right_nums # 需要空间复杂度
    
# wrapper
def quick_sorted(nums: list, reverse=False) -> list:
    nums = _quick_sorted(nums)
    if reverse:
        nums = nums[::-1]
    return nums
    
```

> quick sort 的时间复杂度

| case | Big O |
| --- | --- |
| worst case | O(n^2) |
| avg case | O(n\*log(n)) |
| best case | O(n\*log(n))|

>quick sort 的时间复杂度有点神奇， 如果给一个sorted list 让它排序，会出现 worst case，而给一个“比较乱”的list ，却是avg case，这个跟递归形成的二叉树的形状有关系，越好看的二叉树，算的越快，越丑的二叉树，算的越慢（长偏了的）。

## Divide and Conquer
分治法

- **Divide and Conqure (*Recursion*)**
>Recursion in computer science is a method where the solution to a problem depends on solutions to smaller instances of the same problem (as opposed to iteration). The approach can be applied to many types of problems, and recursion is one of the central ideas of computer science.

### Fibonacci

Fibonacci 的递归算法：
```python
def fibonacci(n):
    if n == 0:
        return 0
    elif n == 1:
        return 1
    else:
        return fibonacci(n-1) + fibonacci(n-2)
```
就像数学思想中的 数学归纳法：

首先有一个 `Base` ，作为一个起点， 然后不断递推。

>那么上述算法的时间复杂度是多少？

O(2^n)

怎么理解：

当我们需要计算一个f(10)的时候，我们会计算一个f(9)和f(8)，然后f(9)需要计算一个f(8)和f(7)。注意这里，f(8)已经需要被计算2次，以此类推，f(7)需要被计算4次...f(1)需要被计算 2^(10-1) 次，f(0)需要被计算2^10

>时间复杂度更低的实现：不用递归

```python
def fibonacci2(n):
    assert(n>=0)
    a, b = 0, 1
    for i in range(1, n+1):
        a, b = b, a+b
    return a
```
时间复杂度为：
O(n)

>如果用递归，如何优化时间复杂度：

思路： 把算过的结果存起来

```python
def fibonacci3(n):
    assert(n>=0)
    if(n <= 1):
        return(n, 0)
    (a, b) = fibonacci3(n-1)
    return (a+b, a)
```

>但是递归算法在Python不同的版本中都会有次数的上限：为了防止无穷无尽的开辟新的内存空间。

RecursionError： 递归次数超过上限。 （2960次，Python版本：3.7.3）

### Binary Search

Question: Given a sorted list of numbers, find the index of a specific value in the list. If no such value, return -1.

**Solution 1**: Brute Force

暴力枚举：是否有序都可以用

时间复杂度 O(n)

```python
def search(num_list, val):
    # If empty
    if num_list == None:
        return -1
    
    for i in range(0, len(num_list)):
        if (num_list[i] == val):
            return i
    return -1
```

**Solution 2**: Binary Search

因为是一个有序的list，所以可以通过对比大小进行二分查找：

1. list取大约中间的下标mid = (low + high)/2，用value跟需要找的值比较，相等则返回mid
2. 若value小，缩减下一轮比较list下标范围，low= mid+1
3. 否则，缩减下一轮比较list下标范围，high= mid-1
4. 终止条件：当下标缩减至l>h，说明全找完了，没找到，返回-1


同样的逻辑可以有以下两种写法：

```python
# Binary Search (recursive)
def bi_serach_re(num_list, val):
    def bi_search(l, h):
        # Not found
        if l > h:
            return -1
        
        # Check mid
        mid = (l + h) // 2
        if (num_list[mid] == val):
            return mid;
        elif (num_list[mid] < val):
            return bi_search(mid + 1, h)
        else:
            return bi_search(l, mid - 1)
        
    return bi_search(0, len(num_list))
```


```python
# Binary Search (iterative)
def bi_search_iter(num_list, val):
    l = 0
    h = len(num_list)
    while (l <= h):
        mid = (l + h) // 2
        if (num_list[mid] == val):
            return mid
        elif (num_list[mid] < val):
            l = mid + 1
        else:
            h = mid - 1
    return -1
```






