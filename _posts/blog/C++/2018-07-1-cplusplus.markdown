---
layout:     post
title:      "八大排序1"
date:       2018-7-1 
header-img: "img/post-bg-2.png"
categories: Cpp
tags: Blog
---


### 前言

所谓的排序就是整理数组中的记录，使之按照关键字递增或者递减的顺序排列起来！

比较常用的排序算法有：

1.直接插入排序

2.希尔排序

3.简单选择排序

4.堆排序

5.冒泡排序

6.快速排序

7.归并排序

8.基数排序

他们之间的关系下图所示：

![](\img\Blog\CPP\tu0.png)

---

### 直接插入排序

基本思想：将数组中的所有元素依次跟前面已经排好的元素相比较，如果选择的元素比已排序的元素小，则交换，直到全部元素都比较过。说白了就跟你码牌的原理很像。
因此，从上面的描述中我们可以发现，直接插入排序可以用两个循环完成：

1. 第一层循环：遍历待比较的所有数组元素
2. 第二层循环：将本轮选择的元素(selected)与已经排好序的元素(ordered)相比较。
   如果：selected > ordered，那么将二者交换

直接插入排序动态图：

![](\img\Blog\CPP\tu.gif)

### C++代码：

```
#include<iostream>
using namespace std;

void print_array(int a[],int n)//打印
{
	cout << "========" << endl;
	for (int i = 0; i < n; i++)
	{
		cout << a[ i ]<<" ";
	}
}
void sort_array(int a[], int n)//排序
{
	int i, j, temp;
	for (i = 1; i < n; i++)//第一层循环
	{
		temp = a[i];
		//第二层循环
		for (j = i - 1; j >= 0 && temp<a[j]; j--)
		{
			a[j + 1] = a[j];
		}
		a[j+1] = temp;
		print_array(a, 9);
	}

}

void main()
{
	int a[] = { 7, 3, 5, 8, 9, 1, 2, 4, 6 };
	cout << "============" << endl;
	print_array(a, 9);
	sort_array(a, 9);
    
	system("pause");
}
```

------



### 希尔排序

基本思想：将待排序数组按照步长gap进行分组，然后将每组的元素利用直接插入排序的方法进行排序；每次将gap折半减小，循环上述操作；当gap=1时，利用直接插入，完成排序。
同样的：从上面的描述中我们可以发现：希尔排序的总体实现应该由三个循环完成：

1. 第一层循环：将gap依次折半，对序列进行分组，直到gap=1
2. 第二、三层循环：也即直接插入排序所需要的两次循环。

当数组的初始状态基本有序时，希尔排序在效率上较直接插入排序有较大的提高，但由于分组的存在，相等元素可能会被分在不同的组，故希尔排序实不稳定的。

![](\img\Blog\CPP\tu3.png)

### C++代码：

```c++
#include<iostream>
using namespace std;

void print_array(int a[], int n)
{
	cout << "=======" << endl;
	for (int i = 0; i < n; i++)
	{
		cout << a[i] << " ";
	}
}
void sort_array(int a[], int n)
{
	int i, j, temp, h;

	for (h = n / 2; h>0; h = h / 2)//第一层
	{
		for (i = 1; i < n; i++)//第二层
		{
			temp = a[i];
			//第三层
			for (j = i - h; j >= 0 && temp < a[j]; j=j-h)
			{
				a[j + h] = a[j];
			}
			a[j+h] = temp;
		}
		print_array(a, 9);
	}
}
void main()
{
	int a[] = { 7, 3, 5, 8, 9, 1, 2, 4, 6 };
	cout << "========" << endl;
	print_array(a, 9);
	sort_array(a, 9);

	system("pause");
}
```



### 后记

每次更新两个算法，下期再会！！！