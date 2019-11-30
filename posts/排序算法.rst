   .. title: 五种排序算法
   .. slug: sort-algorithms
   .. date: 2018-05-15 20:53:29 UTC+08:00
   .. tags: sort, algorithms
   .. category: programming
   .. link:
   .. description:
   .. type: text

冒泡排序
--------

这个算法的名字由来是因为越小的元素会经由交换慢慢“浮”到数列的顶端。

Bubble sort, sometimes referred to as sinking sort, is a simple sorting algorithm that repeatedly steps through the list, compares adjacent pairs and swaps) them if they are in the wrong order.

物理模拟的理论源头.

.. code:: ipython

   # compare to get the small and swap
   # repeat
   def bubble_sort1(arr):
       for _ in range(len(arr)): #cumbersome loop
           # core logic 
           for i in range(1, len(arr)):
               if arr[i] < arr[i-1]:
                   temp = arr[i-1] #目标位置.
                   arr[i-1] = arr[i]
                   arr[i] = temp #swap的逻辑可以先写后面的两行。

   def bubble_sort2(arr):
       #上浮和下沉, 轻质物体向上浮动
       while True:
           swap_counter = 0
           for i in range(1, len(arr)):
               if arr[i] < arr[i-1]: #move smaller up
                   arr[i-1], arr[i] = arr[i], arr[i-1]
                   swap_counter += 1
           if swap_counter == 0:
               return
   import random
   import numpy as np
   arr = [i for i in range(21, 31)]
   arr = np.arange(10, 21)
   random.shuffle(arr)
   print(arr)
   bubble_sort2(arr)
   print(arr)

插入排序
--------

.. code:: ipython

   #psudocodes
   Move the pivot entry to a temporary location leaving a hole in List
   while (there is a name above the hole and that name is greater than the pivot):
   move the name above the hole down into the hole
   leaving a hole above the name
   Move the pivot entry into the hole in List.
   # 向已经排序好的数组中插入
   #解决方案只保留一项, 越少越好.
   #假设有序数组
   #in-place的实现
   def insertion_sort(arr):#这个思路是最清晰的.
       for i in range(1, len(arr)):
           #将当前的pivot拿出
           pivot = arr[i] #Move the pivot to a temporay location,
           hole = i # and leave a hole
           #找到比当前值大的,然后下沉. hole>0是因为第一项已经排好了.
           while hole > 0 and arr[hole-1] > pivot:
               #当上面有更大的,下沉, 因为是排序好的，所以想象lock整体下沉。
               arr[hole] = arr[hole-1] #下沉填充hole
               hole  = hole- 1 #leave the hole,两步操作.
           arr[hole] = pivot #insert sort就是这么来的, 也是交换位置.
           # 最后将hole腾挪出来.
       return arr

选择排序
--------

选择排序是原理最简单的排序 Psuedocodes: 从剩下的序列中选择最小值， 逐个浮上来。

.. code:: ipython

   #要输出的是new_arr
   def selection_sort3(arr):　# 基本思路．
       arr = list(arr)
       new_arr = []
       while arr:
           minimum = min(arr)
           new_arr.append(arr.pop(minimum))
       return new_arr

   def selection_sortA(nums): #selection排序的核心是从subarray中找打最小值.
       for i in range(len(nums)-1): #从subarray的第一项开始
           min_idx = i #设置min_idx的初始值．
           for j in range(i+1, len(nums)):#从subarray中选出最小值
               if nums[j] < nums[min_idx]: #不断更新
                   min_idx = j #找到最小值的index,只进行index操作.
           #循环终止后, i是当前值,
           #swap the current value and the minial value
           temp = nums[i]
           nums[i] = nums[min_idx]
           nums[min_idx] = temp

归并排序
--------

.. code:: ipython

   #psuedocodes方案
   from heapq import merge
   def merge_sort1(m): #Postoder
       #LRN遍历,
       if len(m) <= 1:
           return m
       mid = len(m) // 2
       left = merge_sort1(m[:mid])
       right = merge_sort1(m[mid:])
       return list(merge(left, right))

   #step by step soluthon
   def merge(left, right):
       result = []
       i, j = 0, 0
       while i < len(left) and j < len(right):
           if left[i] < right[j]:
               result.append(left[i])
               i += 1
           else:
               result.append(right[j])
               j += 1
       while i < len(left):
           result.append(left[i])
           i += 1
       while j < len(right):
           result.append(right[j])
           j += 1
       return result

   def mergeSort(L):
       if len(L) < 2:
           return L[:]
       else:
           left = mergeSort(L[:mid])
           right = mergeSort(L[mid:])
           return merge(left, right)
       #我的时间管理可能应该是tree的结构.

    #in-place merge sort
   def merge_inplace(A, start, mid, end) -> "None":
       left = A[start:mid]
       i = 0
       j = 0

       for c in range(start, end): #c for cur
           if j >= len(right) or (i < len(left) and left[i] < right[j]): #注意shortcuit的问题.
               A[c] = left[i]
               i = i + 1
           else:
               A[c] = right[j]
               j = j + 1

   def mergeSort_inplace(A, lo, hi) -> "None":
       if lo < hi - 1:
           mid = (lo + hi) // 2
           mergeSort_inplace(A,lo,mid)
           mergeSort_inplace(A,mid,hi)
           merge_inplace(A, lo, mid, hi)e(left, right)
       #我的时间管理可能应该是tree的结构.

    #in-place merge sort
   def merge_inplace(A, start, mid, end) -> "None":
       left = A[start:mid]
       i = 0
       j = 0

       for c in range(start, end): #c for cur
           if j >= len(right) or (i < len(left) and left[i] < right[j]): #注意shortcuit的问题.
               A[c] = left[i]
               i = i + 1
           else:
               A[c] = right[j]
               j = j + 1

   def mergeSort_inplace(A, lo, hi) -> "None":
       if lo < hi - 1:
           mid = (lo + hi) // 2
           mergeSort_inplace(A,lo,mid)
           mergeSort_inplace(A,mid,hi)
           merge_inplace(A, lo, mid, hi)

归并排序是一种稳定的排序方法。和选择排序一样，归并排序的性能不受输入数据的影响，但表现比选择排序好的多，因为始终都是O(nlogn）的时间复杂度。代价是需要额外的内存空间。 归并算法的代价就是需要额外的存储空间.

快速排序
--------

.. code:: ipython

   def quicksort(arr):
       if len(arr) < 2:
           return arr #base case
       else:
           pivot = arr[0]
           less = [i for i in arr[1:] if i <= pivot] # filter
           more = [i for i in arr[1:] if i > pivot] # filter
           return quicksort(less) + [pivot] + quicksort(more)

Publish
-------

.. code:: ipython

   import subprocess
   cmd ="pandoc --wrap=none 排序算法.org  -o ~/Public/nikola_post/posts/排序算法.rst"
   subprocess.run(cmd, shell=True)

