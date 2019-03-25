---
layout: post
title:  "【Class Notes for CSCI 104】2 内存及分配"
date:   2019-03-25 +0800
---
## 关于内存的理解
>在物理层面，电脑内存包含了大量的*flip flops*。

>每一个flip flop抱恨了一些*transistors*，并且能否存储1bit

>单独的flip flops可以被一个unique identifier(唯一识别符)访问，所以我们可以读取并重写他们

**因此我们可以把所有的电脑内存思考为一个巨大的，用于存放bits的，可以被读取和重写数组**

因为作为人类我们不善于在bit这个层面进行思考和运算，因此我们把这些bits整理成更大的组合，使得他们可以用来表示数字。

例如，每8bits表示1 byte，在bytes之上还有words(又是是16bits，有时是32bits)。

因此更多的时候我们概念上的将电脑内存是为一个巨大的，由bytes组成的数组

### 内存存放
1. 被我们程序使用的所有的变量和其他数据
2. 程序的代码，包括操作系统的代码

### 内存分配
一般编译器的操作系统共同作业，来对我们的内存进行管理，但是如果能理解底层内存的运作方式对我们是有好处的

当编译代码时，编译器通常为检测原始数据类型，并提前计算他们将会占用的内存，需求的内存大小将会从*栈（stack）*中取出被分配给这个程序
> **stack**: This space is called the stack space because as functions get called, their memory gets added on top of existing memory. As they terminate, they are removed in a LIFO (last in, first out) order.

例如考虑下面的声明

```
int n;
int x[10];
double m;
```
编译器将会立刻计算出这段代码需要
```math
4+4*10+8=52bytes^2
```
之后编译器将会和操作系统交互，在stack中申请必要数量的bytes用于变量的存储，在上述例子中，编译器了解到的内存表示如下：

编译器知道每个变量对应内存的地址，实际上，当我们写下n的时候，它将会被翻译成类似于"memory address 4127963"一样的代码  

当调用其他函数的时候，每一个函数会得到它自身申请的一段自己的stack空间；它将自己左右的**本地变量**存放在自己的stack空间中，但是有一个*program counter*会记录这段空间是在哪个位置申请的。这样当函数**调用结束**的时候，函数申请的内存会被释放给其他任务使用，同时，对于**静态分配**的变量，编译器将会安排好这些特殊的变量
![image](/img/data_structure/0001.jpg)

## Scoping of Variables(变量的作用域)
- **global variables**可以被任何函数和程序中的代码块访问
- **local variable** 只存在于生成它的函数或代码块中，被存储在**stack**中。一旦函数或者代码块运行结束，变量就会被析构

当一个函数被临时挂起（suspended）的时候，内部的变量会被存储在stack中，但是不能被访问
```
void foo (int x)
{
int y;
// do some stuff
}
void bar (int n)
{
int m;
foo (n+m);
// do more stuff
}
Here, when bar calls foo, the function foo cannot access the variables n or m. As soon as foo finishes,
the variables x and y are deallocated, and permanently lost. bar now resumes and has access to n and m,
but not to x or y.
```

## 动态分配
有时不知道一个变量到底需要多少内存，因此需要进行动态分配
```
int n;
cin>>n;
// create an array a of n integers
```
在编译时，编译器不知道数组需要多大的内存，因此它不会再stack中给变量分配空间。与之相对，我们的程序需要在运行时(run-time)显式（explicitly）地向操作系统申请正确的空间。**这样的内存会从heap(堆)中被分配**

Static allocation| Dynamic allocation
---|---
Size must be known at compile time| Size may be unknown at compile time
Performed at compile time| Performed at run time
Assigned to the stack |Assigned to the heap
First in last out |No particular order of assignment

## Pointer(指针)
> **A pointer** is an “integer” that points to a location in memory — specifically, it is an address of a byte

### 一般变量与指针的交互
1. 找到变量在内存中的位置
2. 将指针指向的位置作为一个变量
```
int* p, *q; //Pointers to two integers
int i, j;
i = 5; j = 10;
p = &i; //Obtain the address of i and save it to p
cout << p; //Prints the value of p (address of i)
cout << *p; //Prints the value of the integer that p points to (which is the value of i)
*p = j; // Overwrites the value of the location that p points to (so, i) with the value of j
*q = *p; // Overwrites the value of the location that q points to with the one that p points to
q = p; // Overwrites the pointer p with q, so they now point to the same location
```
NULL空指针(location 0)，未初始化的指针变量
**p=NULL**会得到运行时错误。在C++11中nullptr是NULL的替代
```
注意，当定义指针的时候，如果没有初始化最好使用int *p = NULL, *q = NULL
这样可以避免我们误更改其他位置的数据
```
之前说pointer是一个integers，实际上在算法上有些地方是不同的。  
例如一个指向int的指针p，当写p+1的时候实际上编译器认为我们想要指向下一个integer的数据，因此p+1就会移动4个bytes
> When you have a **void***, then addition really does refer to adding individual bytes. For all others, when you write p+k for a pointer p and integer k, this references memory location p+k*size(<type>), where <type> is the type of the pointer p.

## 动态内存分配
### C style: malloc free  
void * malloc(unsigned int size)  
申请size bytes的内存，返回内存的指针，如果失败，就返回NULL

void free(void* pointer)
释放pointer处的内存

```
int n;
int* b;
cin >> n;
b = (int*) malloc(n*sizeof(int));
for (int i=0; i<n; i++)
cin >> b[i];
```
更好的编程习惯：
- 在初始化前时常判断b==NULL
- 在析构后(free)，立刻将b设置为NULL

```
b[i]等价于*((int*)((void*)b+sizeof(int)))
```

### c++ style: new delete
## 内存泄漏
```
double *x;
...
x = (double*) malloc(100*sizeof(double));
...
x = (double*) malloc(200*sizeof(double)); // We need a bigger array now!
...
free(x);
```
在执行第二次分配的时候，我们会丢失第一次分配的内存的地址，并且在free时，我们只会析构第二次分配的内存。对于第一次分配的内存丢失，我们称为内存泄漏。

好的版本
```
double *x;
...
x = (double*) malloc(100*sizeof(double));
...
free(x);
x = NULL;
x = (double*) malloc(200*sizeof(double));
...
free(x);
x = NULL;
```

