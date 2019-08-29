---
layout:     post
title:      "数据结构-单链表"
date:       2018-7-15 
header-img: "img/post-bg-2.png"
categories: Cpp，数据结构
tags: Blog
---

### 链表结构：

- 链表是一种常见的数据结构——与数组不同的是：

  ​        1.数组首先需要在定义时声明数组大小，如果像这个数组中加入的元素个数超过了数组的长度时，便不能正确保存所有内容；链表可以根据大小需要进行拓展。

  ​        2.其次数组是同一数据类型的元素集合，在内存中是按一定顺序连续排列存放的；链表常用malloc等函数动态随机分配空间，用指针相连。

  ​    链表结构示意图如下所示：

  ![](\img\Blog\data_struct\tu1.png)

在链表中，每一个元素包含两个部分；数据部分和指针部分。数据部分用来存放元素所包含的数据，指针部分用来指向下一个元素。最后一个元素的指针指向NULL，表示指向的地址为空。整体用结构体来定义。

```c++
typedef	struct node
{
	int	data;
	node *next;
}node;
```

单链表中，每个结点只有一个指针，所有结点都是单线联系，除了末为结点指针为空外，每个结点的指针都指向下一个结点，一环一环形成一条线性链。

#### 链表的基本操作：

```c++
node *create()
{
	int	i = 0;
	node *head,*p,*q;
	int	x = 0;
	head = (node*)malloc(sizeof(node));

	while (1)
	{
		printf("please	input	the	data:");
		scanf("%d",&x);
		if (x == 0)
			break;
		p = (node*)malloc(sizeof(node));
		p->data = x;
		if (++i == 1)
		{
			head->next = p;
		}
		else
		{
			q->next = p;
		}
		q = p;
	}
	q->next = NULL;
	return	head;
}
//测长
int	length(node	*head)
{
	int	len=0;
	node*p;
	p = head->next;
	while (p != NULL)
	{
		len++;
		p = p->next;
	}
	return	len;
}
//打印
void	print(node	*head)
{
	node	*p;
	int	index = 0;
	if (head->next == NULL)
	{
		printf("link is	empty!\n");
	}
	p = head->next;
	while (p != NULL)
	{
		printf("the	%dth node is:%d\n",++index,p->data);
		p = p->next;
	}
}
//插入元素
node *insert_node(node*head, int	i, int	data)
{
	node*lb=head;
	int	j = 0;
	while (lb!= NULL&&j < i - 1)//找到我要插入的位置
	{
		lb = lb->next;
		j++;
	}
	if(lb!=NULL	&&	j==i-1)
	{
		node *newdata = (node*)malloc(sizeof(node));
		newdata->data = data;
		newdata->next = lb->next;
		lb->next = newdata;
	}
	
	return	head;
}
//删除元素
node *delect_node(node*head,int	i)
{
	node*lb = head;
	int	j = 0;
	while (lb != NULL&&j < i - 1)
	{
		lb = lb->next;
		j++;
	}
	if (lb != NULL&&lb->next != NULL)
	{
		node *item=NULL;
		item = lb->next;
		lb->next = item->next;
		delete item;
	}
	return	head;
}
//交换任意位置的两个数
node *swap_node(node*head, int	i, int	j)
{
	node*lb1 = head;
	node*lb2 = head;
	int	pos1=0;
	int	pos2 = 0;
	while (lb1 != NULL&&pos1 < i )
	{
		lb1 = lb1->next;
		pos1++;
	}
	printf("%d\n", lb1->data);
	while (lb2 != NULL&&pos2 < j)
	{
		lb2 = lb2->next;
		pos2++;
	}
	printf("%d\n", lb2->data);

	if (lb1 != NULL&&lb2 != NULL)
	{
		int	temp_data = lb1->data;
		lb1->data = lb2->data;
		lb2->data = temp_data;
	}
	return	head;
}
//节点查找
node* search_node(node* head, int	pos)
{
	node* p = head->next;
	if (pos == 0)
		return	head;
	while (--pos)
	{
		if ((p = p->next) == NULL)
			break;
	}
	return	p;
}
//反转链表
node*reverse(node*head)
{
	node *p, *q, *l;
	p = head->next;//第一个节点
	q = p->next;//第二个节点
	p->next = NULL;
	while (q != NULL)
	{
		l = q->next;
		q->next = p;
		p = q;
		q = l;
	}
	head->next = p;
	return	head;
}
//寻找中间元素
node* search_mid(node* head)
{
	node	*current=NULL, *middle=NULL;
	current = middle = head->next;
	int	i=0, j=0;
	while (current != NULL)
	{
		if (i / 2 > j)
		{
			j++;
			middle = middle->next;
		}
		i++;
		current = current->next;
	}
	return	middle;
}
//正向排序
node* insertsort(void)
{
	int	data = 0;
	struct node* head = NULL;
	struct node	*New, * cur, * pre;

	while(1)
	{
		printf("请输入一个数");
		scanf("%d",&data);
		if (data == 0)
			break;
		New = (struct node*)malloc(sizeof(struct node));
		New->data = data;
		New->next = NULL;

		if (head == NULL)
		{
			head = New;
			
			printf("%d", head->data);
			continue;
			
		}		
		if (New->data <= head->data)
		{
			New->next = head;
			head = New;
			
			continue;
		}
		cur = head;
		while(New->data > cur->data && cur->next != NULL)
		{
			pre = cur;
			cur = cur->next;
		}
		if (New->data <= cur->data)
		{
			pre->next = New;
			New->next = cur;
		}
		else
		{
			cur->next = New;
		}
	}
	node* p = head;

	while (p != NULL)
	{
		printf("%d\n", p->data);
		p = p->next;
	}
	return	head;
}
void main()
{
	node*head = create();
	printf("length:%d\n", length(head));
	head = insert_node(head, 2, 5);
	print(head);
	head = delect_node(head, 2);
	print(head);
	head = swap_node(head, 1, 3);
	print(head);
	printf("\n");
	reverse(head);
	print(head);
	printf("\n");
	node*	u=search_mid(head);
	printf("mid:%d\n", u->data);
    
	node*u=insertsort();
	system("pause");
}
```



------

### 总结

链表反转的思想：利用一个辅助指针，存储遍历过程中当前指针指向的下一个元素，然后将当前节点的元素的指针反转，利用已经存储的指针继续遍历。