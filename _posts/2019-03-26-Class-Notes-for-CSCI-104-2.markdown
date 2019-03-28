---
layout: post
title:  "【Class Notes for CSCI 104】3 字符串与流"
date:   2019-03-26 +0800
---
## 字符串
字符的集合 
#### 在basic C中
需要在使用前需要分配足够的空间  
'\0'表示字符串的结束
#### 在C++中(string)
隐藏了一定的内存管理方法  
一些经常使用的运算符
- '+' appendingstring
- '=' assignment
- '==' comparision

在c中非常有用但c++中没有等价的实现  
strtok：用于字符串的分解

## 流
与数据交流的形式
- read: keyboard, files strings
- write: screen, files, strings  
基础是提取的或者要输出的一系列字符

-|cout|cerr
-|-|-
特点|将输出缓存|立即打印|
用途|用于真实的输出|通常用于输出debug信息

当对流输出重新导向的时候，cout的输出会该变，但是cerr仍然只会输出在屏幕上

输入流中：
- cin.fail()可以用于检测是否成功读入；注意一旦被set，需要显式地reset这个变量(通过cin.clear())，并且重新进行读入（因为会默认认为读入的数据不可靠）
- ">>"读取到下一个空白区域，包括spaces, tabs和新的一行
```
cout << "Please enter n:"
cin >> n;
cout << "Please enter m:"
cin >> m;
and enter “5 3” at the first prompt, it will read 5 into n and 3 into m.
```
- 想要读取spaces的话，不应该使用>>，而应该使用getline
```
注意，当读到文件的末尾时，要使用正确的flags来避免重复读取最后一行
```
- 如果想要清楚流中之后的几个字符，可以使用ignore来忽略随后的n个字符，默认是1个
### 文件流和字符串流
当到达文件的末尾时，经常会得到fail的条件，当使用getline时可能会造成一些困扰

注意，对流的操作再结束以后，应当及时的关闭他们，close()