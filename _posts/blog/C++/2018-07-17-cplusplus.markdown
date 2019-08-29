---
layout:     post
title:      "数据结构-队列和栈"
date:       2018-7-17 
header-img: "img/post-bg-1.png"
categories: Cpp，数据结构
tags: Blog
---

### 队列：

- 队列的定义

  队列(Queue)：也是运算受限的线性表。是一种先进先出(First In First Out ，简称FIFO)的线性表。只允许在表的一端front进行插入，而在另一端rear进行删除。
      队首(front) ：允许进行删除的一端称为队首。
      队尾(rear) ：允许进行插入的一端称为队尾。
  例如：排队购物。操作系统中的作业排队。先进入队列的成员总是先离开队列。

   队列中没有元素时称为空队列。在空队列中依次加入元素a1, a2, …, an之后，a1是队首元素，an是队尾元素。显然退出队列的次序也只能是a1, a2, …, an ，即队列的修改是依先进先出的原则进行的，如图所示。

  ![](\img\Blog\data_struct\tu2.jpg)

  

  队列的实现可以使用链表和数组，我们使用单链表实现队列。

  ```c++
  #include<iostream>
  using namespace std;
  
  typedef struct _Node
  {
  	int data;
  	struct _Node* next;
  }node;
  typedef struct _Queue
  {
  	node* front;
  	node* rear;
  }MyQueue;
  
  MyQueue* creatmyqueue()//创建队列
  {
  	MyQueue* q = (MyQueue*)malloc(sizeof(MyQueue));
  	q->front = NULL;
  	q->rear = NULL;
  	return q;
  }
  
  MyQueue* enqueue(MyQueue* q,int data)//入队
  {
  	node* newp = NULL;
  	newp = (node*)malloc(sizeof(node));
  	newp->data = data;
  	newp->next = NULL;
  	if (q->rear == NULL)
  	{
  		q->front = q->rear = newp;
  	}
  	else
  	{
  		q->rear->next = newp;
  		q->rear = newp;
  	}
  	return q;
  }
  
  MyQueue* dequeue(MyQueue* q)//出队
  {
  	node* pnode = NULL;
  	pnode = q->front;
  	
  	if (pnode == NULL)
  	{
  		printf("empty queue!");
  	}
  	else
  	{
  		q->front = q->front->next;
  		if (q->front == NULL)
  		{
  			q->front = NULL;
  		}
  		free(pnode);
  	}
  	return q;
  }
  
  int getlength(MyQueue* q)//测长
  {
  	int nlen = 0;
  	node* pnode = q->front;
  	while (pnode != q->rear)
  	{
  		nlen++;
  		pnode = pnode->next;
  	}
  	return nlen+1;
  }
  
  void pprint(MyQueue* q)//打印
  {
  	node* pnode = q->front;
  	while (pnode != q->rear)
  	{
  		cout << pnode->data << " ";
  		pnode = pnode->next;
  	}
  	cout << pnode->data;
  }
  void main()
  {
  	int nlen = 0;
  	MyQueue* hp = creatmyqueue();
  	enqueue(hp, 1);
  	enqueue(hp, 2);
  	enqueue(hp, 3);
  	enqueue(hp, 4);
  	nlen = getlength(hp);
  	printf("%d", nlen);
  	cout << endl;
  	pprint(hp);
  	dequeue(hp);
  	cout << endl;
  	pprint(hp);
  	system("pause");
  }
  ```



------

### 栈

栈是一种特殊的线性表。其特殊性在于限定插入和删除数据元素的操作只能在线性表的一端进行。如下所示：

![](\img\Blog\data_struct\tu3.jpg)

举个例子：你在洗碗把洗好的碗编号为1、2、、、n依次摞起来，1号在最下面，向上编号依次增加，然后再从上到下把碗放好，这样的话，先被洗的碗，就后被放好。

这回我们用数组的结构体实现栈：

```c++
#include<stdio.h>
#include<stdlib.h>
#include<memory.h>
#define N 100

struct stack//定义一个栈的结构体
{
	int data[N];
	int top;//标识栈顶；
};
typedef struct stack Stack;//Stack别名；
```

```c++
#include"stack.h"
void init(Stack * p)//初始化；
{
	p->top = -1;//代表为空；
	memset(p->data, 0, sizeof(int)*N);//数据清零；
}
int isempty(Stack *p)
{
	if (p->top==-1)
	{
		return 1;
	}
	else
	{
		return 0;
	}
}//判断是否为空；
int isfull(Stack *p)
{
	if (p->top==N-1)
	{
		return 1;
	}
	else
	{
		return 0;
	}
}//判断溢栈；
int gettop(Stack *p)
{
	return p->data[p->top];
}//得到栈顶；
void push(Stack *p, int key)
{
	if (isfull(p)==1)
	{
		return;
	}
	else
	{
		p->top += 1;
		p->data[p->top] = key;//压入数据
	}
}//插入数据；
void pop(Stack *p)
{
	if (isempty(p)==1)
	{
		return;
	}
	else
	{
		p->top -= 1;		
	}
}//吐出；
void show(Stack * p)
{
	if (isempty(p)==1)
	{
		return;
	}
	else
	{
		for (int i = 0; i < p->top; i++)
		{
			printf("%d",p->data[i]);
		}
		printf("/n");
	}
}//显示栈；
```

```c++
#define  _CRT_SECURE_NO_WARNINGS
#include"stack.h"
void main()//模拟递归的过程；
{
	int num;
	scanf("%d", &num);
	Stack mystack;
	init(&mystack);//初始化；
	while (num)//十进制转二进制；
	{
		push(&mystack, num % 2);
		num = num / 2;
	}
	while (!isempty(&mystack))//栈的先进后出实现逆序！
	{
		printf("%d", gettop(&mystack));//得到栈顶的值；
		pop(&mystack);//吐出；
	}
	system("pause");
}
void main1()
{
	int a[10] = { 1, 2, 3, 4, 5, 6, 7, 8, 9, 0 };
	Stack mystack;
	init(&mystack);//初始化；
	for (int i = 0; i < 10; i++)
	{
		push(&mystack,i);//压入数据；
	}
	while (!isempty(&mystack))
	{
		printf("%d",gettop(&mystack));//得到栈顶；
		pop(&mystack);//吐出数据；
	}
	system("pause");
}
```

------

### 总结

整了这么多，你可能都不爱看，直接上干货：

队列和栈有什么区别：

1）操作的名称不同，

队列：入队和出队，栈：进栈和出栈

2）可操作的方向不同：

队列：队尾入队，队头出队；栈：进栈和出栈都是在栈顶进行的

3）操作的方法不同：

队列：FIFO先进先出;        栈：FILO先进后出

