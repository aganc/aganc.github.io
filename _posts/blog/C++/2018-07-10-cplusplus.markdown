---
layout:     post
title:      "八大排序4"
date:       2018-7-10 
header-img: "img/post-bg-1.png"
categories: Cpp
tags: 算法
---

### 归并排序

基本思想：

​        归并排序是采用分治法的一个典型的应用。它的基本操作是：将已有的子序列合并，达到完全有序的序列；即先使每个子序列有序，再使子序列段间有序。

归并排序其实要做两件事：

- 分解----将序列每次折半拆分

- 合并----将划分后的序列段两两排序合并
  因此，归并排序实际上就是两个操作，拆分+合并

  1.如何合并？
  L[first...mid]为第一段，L[mid+1...last]为第二段，并且两端已经有序，现在我们要将两端合成达到L[first...last]并且也有序。

- 首先依次从第一段与第二段中取出元素比较，将较小的元素赋值给temp[]

- 重复执行上一步，当某一段赋值结束，则将另一段剩下的元素赋值给temp[]

- 此时将temp[]中的元素复制给L[]，则得到的L[first...last]有序

   2.如何分解？
  在这里，我们采用递归的方法，首先将待排序列分成A,B两组；然后重复对A、B序列分组；直到分组后组内只有一个元素，此时我们认为组内所有元素有序，则分组结束。

  ![](\img\Blog\CPP\ru9.gif)

#### C++代码：

```c++
#include<iostream>
using namespace std;

void merge(int a[], int tem[], int start, int zhong, int end);
void msort(int a[], int tem[], int low, int high);

void merge_sort(int a[], int len)
{
	int *tem = NULL;
	tem = new int[len];
	if (tem != NULL)
	{
		msort(a,tem,0, len-1);
		delete[]tem;
	}
}

void msort(int a[], int tem[], int low, int high)
{
	if (low >= high)
		return;
	int mid = (low + high) / 2;
	msort(a, tem, low, mid);
	msort(a, tem, mid + 1, high);
	merge(a, tem, low, mid + 1, high);
}

void merge(int a[], int tem[], int start, int zhong, int end)
{
	int leftstart = start;
	int rightstart = zhong;
	int base = start;
	int midem = zhong;
	int eeeeee = end;
	while (leftstart<=zhong - 1 && rightstart<=end)
	{
		if (a[leftstart]<a[rightstart])
		{
			tem[base++] = a[leftstart++];
		}
		else
		{
			tem[base++] = a[rightstart++];
		}
	}

	while (leftstart<=zhong - 1)
	{
		tem[base++] = a[leftstart++];
	}
	while (rightstart<=end)
	{
		tem[base++] = a[rightstart++];
	}

	for (int i = 0; i < end+1; i++)
		a[i] = tem[i];
	for (int k = 0; k < 9; k++)
		cout << a[k] << " ";
	cout << "======" << endl;
}
void main()
{
	int a[9] = {8,6,1,3,5,2,7,4};
	for (int k = 0; k < 9; k++)
		cout << a[k] << " ";
	cout << "======" << endl;
	merge_sort(a, 9);
	for (int k = 0; k < 9; k++)
		cout << a[k] << " ";
	system("pause");
}
```

------

### 基数排序

基本思想：

基数排序算法的思想很有趣，它不依靠直接比较元素排序。而是采用分配式排序,单独处理元素的每一位。从最高位向最低位处理 称为:最高位优先（MSD）反之称为:最低位优先(LSD)。基数排序也称为桶排序。下面以最低位优先为例：

1.准备10个容器，编号0-9，对应数字0-9。 容器是有序的(按添加顺序)
2.然后按待排序元素的**某一位的数字**(比如:个位/十位/白位)将其存放到对应容器中（数字相同,如: 个位是数字1时, 就把这个元素放在1号桶），所有元素这样处理完后,
3.再从0号容器开始依次到9号容器, 将其中的元素顺序取出。所以容器内的元素收集合并**复制回原数组**，然后再从下一位开始…(比如个位处理完后, 再处理十位/百位....最高位)

这里假设数组元素都是3位数。从个位开始，将数组中的元素按个位数字放入对应的桶中，再从桶中顺序取出到数组，这是数组按个位数字有序排列，再以相同的逻辑处理十位和百位。最后数组中就是有序的了

![](\img\Blog\CPP\tu10.gif)

 

#### C++代码：

```c++
#include<iostream>
using namespace std;
int kth_digit(int a, int kth);
int digit_number(int num);
int find_max(int a[], int len);

void redix_sort(int a[], int len)//按位上数字排序
{
	int *temp[10];
	int count[10] = { 0, 0, 0, 0, 0, 0, 0, 0, 0, 0 };
	int max = find_max(a, len);
	int maxdigit = digit_number(max);
	for (int i = 0; i < 10; i++)
	{
		temp[i] = new int[len];
		memset(temp[i], 0, sizeof(int)*len);
	}
	for (int i = 0; i < maxdigit; i++)
	{
		memset(count, 0, sizeof(int)* 10);
		for (int j = 0; j < len; j++)
		{
			int xx = kth_digit(a[j],i);
			temp[xx][count[xx]] = a[j];
			count[xx]++;
		}
		int index = 0;
		for (int j = 0; j < 10; j++)
		{
			for (int jj = 0; jj < count[j]; jj++)
			{
				a[index++] = temp[j][jj];
			}
		}
	}
}

int kth_digit(int a, int kth)
 //返回number上第kth位的数字
{
	int k_num=a/(pow(10,kth));
	return k_num % 10;
}

int digit_number(int num)//返回number有几位
{
	int digit = 0;
	int flag = num;
	while (flag)
	{
		flag = flag / 10;
		digit++;
	}
	return digit;
}
int find_max(int a[], int len)//找到最大的number
{
	int maxnum = 0;
	for (int i = 0; i < len; i++)
	{
		if (a[i]>maxnum)
		{
			maxnum = a[i];
		}
	}
	return maxnum;
}
void main()
{
	int a[] = { 22, 32, 19, 53, 47, 29 };
	redix_sort(a, 6);
	for (int k = 0; k < 6; k++)
		cout << a[k] << " ";
	system("pause");
}
```

### 总结

每种算法的空间复杂度的总结如下图所示：

![](\img\Blog\CPP\tu1.png)

1.大量随机数下，使用快速排序最为理想。

2.当数据量较小时，选择直接插入排序或者冒泡排序差别不大。

3.考虑数据的类型，比如全部是正整数时，应考虑使用桶排序。



