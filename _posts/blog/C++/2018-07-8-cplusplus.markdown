---
layout:     post
title:      "八大排序3"
date:       2018-7-8 
header-img: "img/post-bg-4.png"
categories: Cpp
tags: 算法
---

### 冒泡排序

基本思想：

1. 将序列当中的左右元素，依次比较，保证右边的元素始终大于左边的元素；（ 第一轮结束后，序列最后一个元素一定是当前序列的最大值；）

2. 对序列当中剩下的n-1个元素再次执行步骤1。

3. 对于长度为n的序列，一共需要执行n-1轮比较

   ![](/img/Blog/CPP/tu7.gif)

#### C++代码：

```c++
#include<iostream>
using namespace std;

void main()
{
	int i, j, n=9;
	int a[] = { 7, 3, 5, 8, 9, 1, 2, 4, 6 };
	for (i = 0; i < n-1; i++)
	{
		for (j = 0; j < n-i - 1; j++)
		{
			if (a[j + 1] < a[j])//从小到大排列
			{
				int temp = a[j];
				a[j] = a[j + 1];
				a[j + 1] = temp;
			}
		}
		for (int k = 0; k < n; k++)
		{
			cout << a[k] << " ";
		}
		cout << "/n" << endl;
	}

	for (int k = 0; k < n; k++)
	{
		cout << a[k] << " ";
	}

	system("pause");
}
```

------

### 快速排序

快速排序的基本思想：挖坑填数+分治法

1. 从序列当中选择一个基准数(pivot)
   在这里我们选择序列当中第一个数最为基准数

2. 将序列当中的所有数依次遍历，比基准数大的位于其右侧，比基准数小的位于其左侧

3. 重复步骤1.2，直到所有子集当中只有一个元素为止。
   用伪代码描述如下：
   1．i =L; j = R; 将基准数挖出形成第一个坑a[i]。
   2．j--由后向前找比它小的数，找到后挖出此数填前一个坑a[i]中。
   3．i++由前向后找比它大的数，找到后也挖出此数填到前一个坑a[j]中。
   4．再重复执行2，3二步，直到i==j，将基准数填入a[i]中，再对a[i]左右两个区间进行递归排序

   ![](/img/Blog/CPP/tu8.gif)

   快速排序,说白了就是给基准数据找其正确索引位置的过程，本质就是把基准数大的都放在基准数的左边,把比基准数小的放在基准数的右边,这样就找到了该数据在数组中的正确位置. 

#### C++代码：

```c++
#include<iostream>
using namespace std;

void quick_sort(int a[], int low, int high)
{
	int i, j, base;
	if (low < high)
	{
		base = a[low];
		i = low;
		j = high;
		while (i < j)
		{
			while (i < j&&a[j] >= base)
				j--;
			if (i < j)
				a[i++] = a[j];
			while (i < j&&a[i] <= base)
				i++;
			if (i < j)
				a[j--] = a[i];
		}
		a[i] = base;
		quick_sort(a, low, i - 1);
		quick_sort(a, i+1, high);
	}
}
void main()
{
	int a[] = { 22, 33, 55, 32, 11, 5, 16, 79, 54, 23 };
	quick_sort(a, 0, 9);
	for (int k = 0; k < 9; k++)
	{
		cout << a[k]<<"  ";
	}
	system("pause");
}
```

### 后记

每次更新两个算法，下期再会！！！

