---
layout:     post
title:      "数据结构-二叉树"
date:       2018-7-21 
header-img: "img/post-bg-4.png"
categories: Cpp，数据结构
tags: Blog
---

### 二叉排序树

- 二叉排序树，又叫二叉查找树，它或者是一棵空树；或者是具有以下性质的二叉树：
  1. 若它的左子树不空，则左子树上所有节点的值均小于它的根节点的值；
  2. 若它的右子树不空，则右子树上所有节点的值均大于它的根节点的值；
  3. 它的左右子树也分别为二叉排序树

  ![](\img\Blog\data_struct\tu4.png)

  了解了二叉排序树，下面就是二叉排序树的增删减查基本操作：

  ```c++
  class Node
  {
  public:
  	int data;
  	Node *parent;
  	Node *left;
  	Node *right;
  public:
  	Node() :data(-1), parent(nullptr), left(nullptr), right(nullptr){};
  	Node(int num) :data(num), parent(nullptr), left(nullptr), right(nullptr){};
  };
  
  class Tree
  {
  public:
  	Tree(int num[],int len);
  	void insertNode1(int data);
  	void insertNode(int data);
  	Node *searchNode(int data);
  	void deleteNode(int data);
  	void inordertree();
  private:
  	void insertNode(Node* current, int data);
  	Node *searchNode(Node* current, int data);
  	void deleteNode(Node* current);
  	void inordertree(Node* current);
  private:
  	Node* root;
  };
  
  Tree::Tree(int num[],int len)//创建
  {
  	root = new Node(num[0]);
  	for (int i = 1; i < len; i++)
  	{
  		insertNode1(num[i]);
  	}
  }
  
  void Tree::insertNode1(int data)
  //插入节点操作(非递归的方法)；
  {
  	Node *p, *par;
  	Node *newnode = new Node(data);
  	p = par = root;
  	while (p != NULL)
  	{
  		par = p;
  		if (data > p->data)
  			p = p->right;
  		else if (data < p->data)
  			p = p->left;
  		else if (data == p->data)
  		{
  			delete newnode;
  			return;
  		}
  	}
  	newnode->parent = par;
  	if (par->data>newnode->data)
  		par->right = newnode;
  	else
  		par->left = newnode;
  }
  void Tree::insertNode(int data)//递归插入；
  {
  	if (root != NULL)
  	{
  		insertNode(root, data);
  	}
  }
  void Tree::insertNode(Node* current, int data)
  {
  	if (data > current->data)
  	{
  		if (current->right == NULL)
  		{
  			current->right = new Node(data);
  			current->right->parent = current;
  		}
  		else
  		{
  			insertNode(current->right, data);
  		}
  
  	}
  	else if (data<current->data)
  	{
  		if (current->left == NULL)
  		{
  			current->left = new Node(data);
  			current->left->parent = current;
  		}
  		else
  		{
  			insertNode(current->left,data);
  		}
  
  	}
  	return;
  }
  
  Node* Tree::searchNode(Node* current, int data)
  {
  	if (data < current->data)
  	{
  		if (current->left == nullptr)
  		{
  			return nullptr;
  		}
  		return searchNode(current->left, data);
  	}
  	else if (data>current->data)
  	{
  		if (current->right == nullptr)
  		{
  			return nullptr;
  		}
  		return searchNode(current->right,data);
  	}
  	return current;
  }
  
  void Tree::deleteNode(int data)
  {
  	Node *current = nullptr;
  
  	current = searchNode(data);
  	if (current != nullptr)
  	{
  		deleteNode(current);
  	}
  }
  void Tree::deleteNode(Node* current)
  {
  	if (current->left != nullptr)
  		deleteNode(current->left);
  	if (current->right != nullptr)
  		deleteNode(current->right);
  
  	if (current->parent == nullptr)
  	{
  		delete current;
  		root = nullptr;
  		return;
  	}
  	if (current->parent->data > current->data)
  		current->parent->left = nullptr;
  	else
  		current->parent->right = nullptr;
  	delete current;
  }
  ```

  扯了这么多，还是来点常考的：

  中序遍历：左→根→右

  先序遍历：根→左→右

  后序遍历：左→右→根

  下面全用递归的方法完成对二叉树的三种遍历方式：

  ```c++
  #include <iostream>
  #pragma comment(lib,"ws2_32.lib")
  using namespace std;
  class Node
  {
  public:
  	int data;
  	Node *parent;
  	Node *left;
  	Node *right;
  public:
  	Node() :data(-1), parent(nullptr), left(nullptr), right(nullptr){};
  	Node(int num) :data(num), parent(nullptr), left(nullptr), right(nullptr){};
  };
  
  class Tree
  {
  public:
  	Tree(int num[], int len);
  	void insertNode1(int data);
  	void insertNode(int data);
  	void inordertree();
  	void preordertree();
  	void postordertree();
  private:
  	void insertNode(Node* current, int data);
  	void inordertree(Node* current);
  	void preordertree(Node* current);
  	void postordertree(Node* current);
  private:
  	Node* root;
  };
  
  Tree::Tree(int num[], int len)
  {
  	root = new Node(num[0]);
  	for (int i = 1; i < len; i++)
  	{
  		insertNode1(num[i]);
  	}
  }
  
  void Tree::insertNode1(int data)
  {
  	Node *p, *par;
  	Node *newnode = new Node(data);
  	p = par = root;
  	while (p != nullptr)
  	{
  		par = p;
  		if (data > p->data)
  			p = p->right;
  		else if (data < p->data)
  			p = p->left;
  		else if (data == p->data)
  		{
  			delete newnode;
  			return;
  		}
  	}
  	newnode->parent = par;
  	if (par->data	>	newnode->data)
  		par->left = newnode;
  	else
  		par->right = newnode;
  }
  
  void Tree::insertNode(int data)
  {
  	if (root != nullptr)
  	{
  		insertNode(root, data);
  	}
  }
  
  void Tree::insertNode(Node* current, int data)
  {
  	if (data > current->data)
  	{
  		if (current->right == nullptr)
  		{
  			current->right = new Node(data);
  			current->right->parent = current;
  		}
  		else
  		{
  			insertNode(current->right, data);
  		}
  
  	}
  	else if (data<current->data)
  	{
  		if (current->left == nullptr)
  		{
  			current->left = new Node(data);
  			current->left->parent = current;
  		}
  		else
  		{
  			insertNode(current->left, data);
  		}
  
  	}
  	return;
  }
  
  //中序遍历
  void Tree::inordertree()
  {
  	if (root == nullptr)
  		return;
  	inordertree(root);
  }
  void Tree::inordertree(Node* current)
  {
  	if (current != nullptr)
  	{
  		inordertree(current->left);
  		cout << current->data << " ";
  		inordertree(current->right);
  	}
  }
  //先序遍历
  void Tree::preordertree()
  {
  	if (root == nullptr)
  		return;
  	preordertree(root);
  }
  void Tree::preordertree(Node* current)
  {
  	if (current != nullptr)
  	{
  		cout << current->data << " ";
  		preordertree(current->left);
  		preordertree(current->right);
  	}
  }
  //后序遍历
  void Tree::postordertree()
  {
  	if (root == nullptr)
  		return;
  	postordertree(root);
  }
  void Tree::postordertree(Node* current)
  {
  	if (current != nullptr)
  	{
  		postordertree(current->left);
  		postordertree(current->right);
  		cout << current->data << " ";
  	}
  }
  ```

  ```c++
  #include "treehead.h"
  #pragma comment(lib,"ws2_32.lib")
  using namespace std;
  void main()
  {	
  	int num[] = {5,3,7,2,4,6,8,1};
  	Tree tree(num, 8);
  	tree.inordertree();
  	printf("\n");
  	tree.preordertree();
  	printf("\n");
  	tree.postordertree();
  	system("pause");
  }
  ```

------

### 总结

从结果中我们可以看出：

中序遍历的结果就是二叉排序树的排序结果。