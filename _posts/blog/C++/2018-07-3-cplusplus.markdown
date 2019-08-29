---
layout:     post
title:      "八大排序2"
date:       2018-7-3 
header-img: "img/post-bg-3.png"
categories: Cpp，算法
tags: Blog
---


### 简单选择排序

简单选择排序的基本思想：比较+交换。

1. 从待排序序列中，找到关键字最小的元素；
2. 如果最小元素不是待排序序列的第一个元素，将其和第一个元素互换；
3. 从余下的 N - 1 个元素中，找出关键字最小的元素，重复(1)、(2)步，直到排序结束。
   因此我们可以发现，简单选择排序也是通过两层循环实现。
   第一层循环：依次遍历序列当中的每一个元素
   第二层循环：将遍历得到的当前元素依次与余下的元素进行比较，符合最小元素的条件，则交换

![](\img\Blog\CPP\tu4.gif)

### C++代码：

```
#include<iostream>
using namespace std;

void select_sort(int data[], int len)
{
	for (int i = 0;i < len;i++)
	{
		int temp = data[i];
		int flag = i;
		for (int j = i + 1;j < len;j++)
		{
			if (data[j] < temp)
			{
				temp = data[j];
				flag = j;
			}
		}
		data[flag] = data[i];
		data[i] = temp;

	}

}
void main()
{
	int data[9] = { 54,38,96,23,15,72,60,45,83 };
	select_sort(data, 9);
	for (int i = 0;i < 9;i++)
		cout << data[i] << " ";
	system("pause");
}
```



---

### 堆排序

首先我们先要了解堆：

堆是具有以下性质的完全二叉树：每个结点的值都大于或等于其左右孩子结点的值，称为大顶堆；或者每个结点的值都小于或等于其左右孩子结点的值，称为小顶堆。如下图：

![](\img\Blog\CPP\tu5.png)

​                                         大堆顶和小堆顶

基本思想：将待排序序列构造成一个大顶堆，此时，整个序列的最大值就是堆顶的根节点。将其与末尾元素进行交换，此时末尾就为最大值。然后将剩余n-1个元素重新构造成一个堆，这样会得到n个元素的次小值。如此反复执行，便能得到一个有序序列了。

实现过程：

步骤一 构造初始堆。将给定无序序列构造成一个大顶堆

1.假设给定无序序列结构如下：

![](\img\Blog\CPP\1.11.png)

2.此时我们从最后一个非叶子结点开始（叶结点自然不用调整，第一个非叶子结点 arr.length/2-1=5/2-1=1，也就是下面的6结点），从左至右，从下至上进行调整。

![](\img\Blog\CPP\1.2.png)

3.找到第二个非叶节点4，由于[4,9,8]中9元素最大，4和9交换。

![](\img\Blog\CPP\1.3.png)

这时，交换导致了子根[4,5,6]结构混乱，继续调整，[4,5,6]中6最大，交换4和6。

![](\img\Blog\CPP\1.4.png)

此时，我们就将一个无需序列构造成了一个大顶堆。

步骤二 将堆顶元素与末尾元素进行交换，使末尾元素最大。然后继续调整堆，再将堆顶元素与末尾元素交换，得到第二大元素。如此反复进行交换、重建、交换。

1.将堆顶元素9和末尾元素4进行交换

![](\img\Blog\CPP\1.5.png)

2.重新调整结构，使其继续满足堆定义

![](\img\Blog\CPP\1.6.png)

3.再将堆顶元素8与末尾元素5进行交换，得到第二大元素8.

![](\img\Blog\CPP\1.7.png)

后续过程，继续进行调整，交换，如此反复进行，最终使得整个序列有序.

![](\img\Blog\CPP\1.8.png)

再简单总结下堆排序的基本思路：

　　a.将无需序列构建成一个堆，根据升序降序需求选择大顶堆或小顶堆;

　　b.将堆顶元素与末尾元素交换，将最大元素"沉"到数组末端;

　　c.重新调整结构，使其满足堆定义，然后继续交换堆顶元素与当前末尾元素，反复执行调整+交换步骤，直到整个序列有序

### C++代码：

```c++
#include<iostream>

using namespace std;

int heapsize = 0;
void maxheap(int a[], int index);

void buildmaxheap(int a[], int length)
{
	heapsize = length;
	for (int i = (length >> 1); i >= 0; i--)
	{
		maxheap(a, i);
	}

}
int Left(int num){ return (num << 1) + 1; }
int Right(int num){ return (num << 1) + 2; }

void maxheap(int a[], int index)
{
	int largest = 0;
	int left = Left(index);
	int right = Right(index);

	if (left <= heapsize&&a[left] > a[index])
		largest = left;
	else
		largest = index;
	
	if (right <= heapsize&&a[right] > a[largest])
		largest = right;

	if (largest != index)
	{
		swap(a[index], a[largest]);
		maxheap(a, largest);
	}
}

void heap_sort(int a[],int length)
{
	buildmaxheap(a, (length - 1));

	for (int i = (length - 1); i >= 1; i--)
	{
		swap(a[0], a[i]);
		heapsize--;
		maxheap(a, 0);
	}
}

void main()
{
	int a[] = { 45, 68, 20, 39, 88, 97, 46, 59 };
	heap_sort(a, 8);
	for (int k = 0; k < 8; k++)
		cout << a[k] << "  ";

	system("pause");
}
```



### 后记

每次更新两个算法，下期再会！！！