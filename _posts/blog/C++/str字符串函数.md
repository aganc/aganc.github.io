---
layout:     post
title:      "字符串函数"
date:       2019-4-2 
header-img: "img/post-bg-3.png"
categories: Cpp
tags: C++
---



#### str字符串函数

1.str.append() 		 添加文本

如str1.append(str2);添加另一个字符串的某一段子串:

如str1.append(str2, 11, 7);添加几个相同的字符:

如str1.append(5, '.');注意,个数在前字符在后.上面的代码意思为在str1后面添加5个".".



2.str.substr()

主要功能是复制子字符串，要求从指定位置开始，并具有指定的长度。如果没有指定长度_Count或_Count+_Off超出了源字符串的长度，则子字符串将延续到源字符串的结尾。

参数：起始字符序号，剪切个数

string str1 = str.substr(1,3);

从第一位开始剪切3位

str为“abcd:，则str1为bcd



3.str.erase()

（1）erase(pos,n); 删除从pos开始的n个字符，比如erase(0,1)就是删除第一个字符
（2）erase(position);删除position处的一个字符(position是个string类型的迭代器)
（3）erase(first,last);删除从first到last之间的字符（first和last都是迭代器



4.memset（）

memset是计算机中C/C++语言初始化函数。作用是将某一块内存中的内容全部设置为指定的值， 这个函数通常为新申请的内存做初始化工作。

例1

char str[9];我们用memset给str初始化为“00000000”，用法如下

 memset(str,0,8); 注意，memset是逐字节 拷贝的。 

   例2

   int num[8];我们用memset给str初始化为{1,1,1,1,1,1,1,1}

memset(num,1,8);//这样是不对的一个int是4个字节的，8个int是32个字节，所以首先要赋值的长度就不应该为8而是32。因为memset是 逐字节 拷贝，以num为首地址的8字节空间都被赋值为1， 即一个int变为0X00000001 00000001 00000001 00000001，显然，把这个数化为十进制不会等于1的.