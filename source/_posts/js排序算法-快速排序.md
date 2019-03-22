---
title: js排序算法-快速排序
date: 2019-03-21 16:20:22
categaries: 算法
tags: 算法
---

## 快速排序

### 概念：

 (1) 首先，从数组中选择中间一项作为主元。

 (2) 创建两个指针，左边一个指向数组第一个项，右边一个指向数组最后一个项。移动左指针直到我们找到一个比主元大的元素，接着，移动右指针直到找到一个比主元小的元素，然后交

换它们，重复这个过程，直到左指针超过了右指针。这个过程将使得比主元小的值都排在主元之前，而比主元大的值都排在主元之后。这一步叫作划分操作。

 (3) 接着，算法对划分后的小数组（较主元小的值组成的子数组，以及较主元大的值组成的子数组）重复之前的两个步骤，直至数组已完全排序。

复杂度为：O(nlogn)

代码实现：

```js 
var quickSort = function(array) {
 quick(array, 0, array.length - 1);
};

var swap = function(array, index1, index2){
  var aux = array[index1];
  array[index1] = array[index2];
  array[index2] = aux;
};

var quick = function(array, left, right){
  var index; //{1}
  if (array.length > 1) { //{2}
    index = partition(array, left, right); //{3}partition函数返回值将赋值给index
    if (left < index - 1) { //{4}如果子数组存在较小值的元素
      quick(array, left, index - 1); //{5}则对该数组重复这个过程
    }
    if (index < right) { //{6}
      quick(array, index, right); //{7}如果存在子数组存在较大值，我们也将重复快速排序过程
    }
  }
};

var partition = function(array, left, right) {
  var pivot = array[Math.floor((right + left) / 2)], //{8}我们选择中间项作为主元
      i = left, //{9}，初始化为数组第一个元素
      j = right; //{10}初始化为数组最后一个元素。
  while (i <= j) { //{11}只要left和right指针没有相互交错
    while (array[i] < pivot) { //{12}首先，移动left指针直到找到一个元素比主元大
      i++;
    }
    while (array[j] > pivot) { //{13}对right指针，我们做同样的事情，移动right指针直到我们找到一个元素比主元小。
      j--;
    }
    if (i <= j) { //{14}当左指针指向的元素比主元大且右指针指向的元素比主元小，并且此时左指针索引没有右指针索引大
      swap(array, i, j); //{15}我们交换它们，然后移动两个指针，并重复此过程
      i++;
      j--;
    }
  }
  return i; //{16}
};

var array = [3,5,1,6,4,7,2];
quickSort(array);
console.log(array);// [1, 2, 3, 4, 5, 6, 7]
```

 可以点击下面链接进行查看：

 See the Pen 快速排序 by rachel (@pprachel) on CodePen.

 

实现过程：
![image](https://img2018.cnblogs.com/blog/913399/201901/913399-20190115141107795-1431129836.png)


 

 下面的示意图展示了对有较小值的子数组执行的划分操作（注意7 和6 不包含在子数组之内）：


![image](https://img2018.cnblogs.com/blog/913399/201901/913399-20190115141219930-1857660716.png)
 

 接着，我们继续创建子数组，请看下图，但是这次操作是针对上图中有较大值的子数组（有1 的那个较小子数组不用再划分了，因为它仅含有一个项）。
![image](https://img2018.cnblogs.com/blog/913399/201901/913399-20190115141251219-149566665.png)
 

 子数组（[2, 3, 5, 4] ）中的较小子数组（[2, 3] ）继续进行划分（算法代码中的行{5} ）：


![image](https://img2018.cnblogs.com/blog/913399/201901/913399-20190115141825771-794720346.png)
 

 然后子数组（[2, 3, 5, 4] ）中的较大子数组（[5, 4] ）也继续进行划分（算法中的行{7} ），示意图如下：

![image](https://img2018.cnblogs.com/blog/913399/201901/913399-20190115141901800-1116729451.png)

 最终，较大子数组[6, 7] 也会进行划分（partition ）操作，快速排序算法的操作执行完成。

 

可以通过下面这个网站看动态的过程：https://visualgo.net/zh/sorting

优化：

1.即挑选基准元素时，先把第一个元素、最后一个元素和中间一个元素挑出来，这三个元素中大小在中间的那个元素就被认为是基准元素。https://www.jianshu.com/p/fc342a9ffb58

2.三路快排： https://www.jianshu.com/p/fc342a9ffb58
