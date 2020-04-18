---
layout: post
title: 排序算法简析和JS的实现
description: 排序算法的简析，以及使用js实现排序算法
tag: sort、排序算法、冒泡排序、选择排序、插入排序、希尔排序、快速排序、堆排序
category: 总结、技术
---
## 排序算法

排序算法是学习与工作中最常见的算法之一，常见的排序算法包括冒泡排序、选择排序、插入排序、希尔排序、快速排序、堆排序等。

| 排序算法 | 时间复杂度(平均) | 时间复杂度(最坏) | 时间复杂度(最好) | 空间复杂度  | 稳定性 |
| -------- | ---------------- | ---------------- | ---------------- | ----------- | ------ |
| 冒泡排序 | O(n^2)           | O(n^2)           | O(n)             | O(1)        | 稳定   |
| 选择排序 | O(n^2)           | O(n^2)           | O(n^2)           | O(1)        | 不稳定 |
| 插入排序 | O(n^2)           | O(n^2)           | O(n)             | O(1)        | 稳定   |
| 希尔排序 | O(n^1.3)         | O(n^2)           | O(n)             | O(1)        | 不稳定 |
| 快速排序 | O(nlog2(n))      | O(n^2)           | O(nlog2(n))      | O(nlog2(n)) | 不稳定 |
| 堆排序   | O(nlog2(n))      | O(nlog2(n))      | O(nlog2(n))      | O(1)        | 不稳定 |

注释：

时间复杂度：对数据进行排序操作的总次数。可以反映当n变化时的规律。

空间复杂度：对数据进行排序需要占用的计算机内存空间。可以反映当n变化时对空间占用情况的变化规律。

稳定性：相同数据的值，经过排序后，顺序是否会发生变化。例如当需要排序的数据中有两个值相等的数据a,b且a=b，当经过上述排序算法进行排序后，a和b的位置是否会进行交换，顺序变为b,a，当顺序进行交换时，则为不稳定。

## 冒泡排序

冒泡排序（Bubble Sort）也是一种简单直观的排序算法。它重复地走访要排序的数列，一次比较两个元素，如果他们的顺序错误就把他们交换过来。走访数列的工作是重复地进行直到没有再需要交换，也就是说该数列已经排序完成。这个算法的名字由来是因为越小的元素会经由交换慢慢“浮”到数列的顶端。

![](/images/20151109sortalgorithm/bubble-sort.gif)

### 算法描述：

1.比较相邻的两个元素，如果第一个元素大于第二个元素，则两个元素进行交换。

2.对每一个相邻的元素做同样的操作，从第一对到结尾的最后一对，这样在最后的元素应该会是最大的数。

3.针对所有的元素重复以上的步骤，除了最后一个元素。

4.重复步骤1-步骤3，直到完成排序。

### 代码实现：

```javascript
function bubbleSort(arr) {
  var len = arr.length;
  for(var i = 0; i < len - 1; i++) {
    for(var j = 0; j < len - 1 - i; j++) {
      // 相邻元素两两对比
      if(arr[j] > arr[j+1]) {
        // 元素交换
        var temp = arr[j+1];
        arr[j+1] = arr[j];
        arr[j] = temp;
      }
    }
  }
  return arr;
}
```

## 选择排序

选择排序(Selection-sort)是一种简单直观的排序算法。它的原理是每次在未排序的序列中寻找最小（大）的元素，然后将其存放到已排序序列中的末尾，以此类推，直到所有元素均排序完毕。 

![](/images/20151109sortalgorithm/selection-sort.gif)

### 算法描述

1.首先在未排序序列中找到最小（大）元素，存放到已排序序列的起始位置。

2.再从剩余未排序序列中寻找最小（大）元素，存放到已排序序列的末尾。

3.重复步骤二，直到排序完成。

### 代码实现

```javascript
function selectionSort(arr) {
  var len = arr.length;
  var minIndex, temp;
  for(var i = 0; i < len - 1; i++) {
    minIndex = i;
    for(var j = i + 1; j < len; j++) {
      // 寻找最小的元素
      if(arr[j] < arr[minIndex]) {
        // 保存最小元素的索引
        minIndex = j;
      }
    }
    temp = arr[i];
    arr[i] = arr[minIndex];
    arr[minIndex] = temp;
  }
  return arr;
}
```

## 插入排序

插入排序（Insertion Sort）是一种最简单直观的排序算法。它的工作原理是通过构建有序的序列，对于未排序的数据，在已排序的序列中从后向前扫描，找到相应位置并插入。

![](/images/20151109sortalgorithm/insertion-sort.gif)

### 算法描述

1.从第一个元素开始，将第一个元素当做已被排序。

2.从下一个元素开始，取出该元素，然后在已排序的序列中从后往前扫描，插入到合适的位置。

3.重复步骤2，直到排序完成。

### 代码实现

```javascript
function insertionSort(arr) {
  var len = arr.length;
  var preIndex, current;
  for(var i = 1; i < len; i++) {
    preIndex = i - 1;
    current = arr[i];
    while(preIndex >= 0 && arr[preIndex] > current) {
      arr[preIndex + 1] = arr[preIndex];
      preIndex--;
    }
    arr[preIndex + 1] = current;
  }
  return arr;
}
```

## 希尔排序

希尔排序（Shell Sort）是简单插入排序的改进版。它与插入排序的不同之处在于，它会优先比较距离较远的元素。希尔排序又叫缩小增量排序。

![](/images/20151109sortalgorithm/shell-sort.gif)

### 算法描述

0.现将整个待排序的序列分割成若干个子序列并分别进行直接插入排序。

1.选择一个增量序列t1，t2，…，tk，其中ti>tj，tk=1。

2.按增量序列个数k，对序列进行k趟排序。

3.每趟排序，根据对应的增量ti，将待排序列分割成若干长度为m的子序列，分别对各子表进行直接插入排序。仅增量因子为1时，整个序列作为你一个表来处理，表长度即为整个序列的长度。

### 代码实现

```javascript
function shellSort(arr) {
  var len = arr.length;
  for(var gap = Math.floor(len / 2); gap > 0; gap = Math.floor(gap / 2)) {
    // 注意：这里和动图演示的不一样，动图是分组执行，实际操作是多个分组交替执行
    for(var i = gap; i < len; i++) {
      var j = i;
      var current = arr[i];
      while(j - gap >= 0 && current < arr[j - gap]) {
         arr[j] = arr[j - gap];
         j = j - gap;
      }
      arr[j] = current;
    }
  }
  return arr;
}
```

## 快速排序

快速排序（Quick Sort）的基本思想：通过一趟排序将待排记录分隔成独立的两部分，其中一部分记录的关键字均比另一部分的关键字小，则可分别对这两部分记录继续进行排序，以达到整个序列有序。

![](/images/20151109sortalgorithm/quick-sort.gif)

### 算法描述

1.从数列中挑出一个元素，称为“基准”。

2.重新排序数列，所有元素比基准值小的摆放在基准前面，所有元素比基准值大的摆放在基准的后面（相同的数可以放到任意一边）。在这个分区退出之后，该基准就处于数列的中间位置。这个称为分区操作。

3.递归地把小于基准值元素的子数列和大于基准值元素的子数列进行排序。

### 代码实现

```javascript
function quickSort(arr, left, right) {
  var len = arr.length,
      partitionIndex,
      left = typeof left != 'number' ? 0 : left,
      right = typeof right != 'number' ? len - 1 : right;
 
  if(left < right) {
    partitionIndex = partition(arr, left, right);
    quickSort(arr, left, partitionIndex-1);
    quickSort(arr, partitionIndex+1, right);
  }
  return arr;
}

// 分区操作
function partition(arr, left ,right) {
  var pivot = left, // 设定基准值（pivot）
      index = pivot + 1;
  for(var i = index; i <= right; i++) {
    if(arr[i] < arr[pivot]) {
      swap(arr, i, index);
      index++;
    }       
  }
  swap(arr, pivot, index - 1);
  return index-1;
}

function swap(arr, i, j) {
  var temp = arr[i];
  arr[i] = arr[j];
  arr[j] = temp;
}
```

## 堆排序

堆排序（Heapsort）是指利用堆这种数据结构所设计的一种排序算法。堆积是一个近似完全二叉树的结构，并同时满足堆积的性质：即子结点的键值或索引总是小于（或者大于）它的父节点。

![](/images/20151109sortalgorithm/heap-sort.gif)

### 算法描述

1.将初始待排序关键字序列(R1,R2….Rn)构建成大顶堆，此堆为初始的无序区。

2.将堆顶元素R[1]与最后一个元素R[n]交换，此时得到新的无序区(R1,R2,……Rn-1)和新的有序区(Rn),且满足R[1,2…n-1]<=R[n]

3.由于交换后新的堆顶R[1]可能违反堆的性质，因此需要对当前无序区(R1,R2,……Rn-1)调整为新堆，然后再次将R[1]与无序区最后一个元素交换，得到新的无序区(R1,R2….Rn-2)和新的有序区(Rn-1,Rn)。不断重复此过程直到有序区的元素个数为n-1，则整个排序过程完成。

### 代码实现

```javascript
var len; // 因为声明的多个函数都需要数据长度，所以把len设置成为全局变量

function buildMaxHeap(arr) { // 建立大顶堆
  len = arr.length;
  for(var i = Math.floor(len/2); i >= 0; i--) {
    heapify(arr, i);
  }
}
 
function heapify(arr, i) { // 堆调整
  var left = 2 * i + 1,
      right = 2 * i + 2,
      largest = i;
 
  if(left < len && arr[left] > arr[largest]) {
    largest = left;
  }
 
  if(right < len && arr[right] > arr[largest]) {
    largest = right;
  }
 
  if(largest != i) {
    swap(arr, i, largest);
    heapify(arr, largest);
  }
}
 
function swap(arr, i, j) {
  var temp = arr[i];
  arr[i] = arr[j];
  arr[j] = temp;
}
 
function heapSort(arr) {
  buildMaxHeap(arr);
 
  for(var i = arr.length - 1; i > 0; i--) {
    swap(arr, 0, i);
    len--;
    heapify(arr, 0);
  }
  return arr;
}
```

