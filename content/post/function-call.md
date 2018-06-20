---
author: "Raymond Shaw"
date: 2016-12-18
title: "function-call"
tags: [
  "C++",
  "STL",
]
categories: [
  "Development",
  "C++",
  "index",
]
---


# function-call

Dec 28, 2016 | Hits

# 1.2 仿函数（function call操作符）

 从函数指针到仿函数。

## 1.1 函数调用操作符（C++语法中的左右括号）也可以被重载。
`许多STL算法都提供了两个版本，一个用于一般状况（例如排序时以递增方式排列），一个用于特殊状况（例如排序时由使用者指定以何种特殊关系进行排列）。像这种情况，需要用户指定某个条件或策略，而条件或策略的背后由一整组操作构成，便需要某种特殊的东西来代表这“一整组操作”。`
`代表“一整组操作”的，是函数。在旧式C语言中，欲将函数当做参数传递，唯有通过函数指针才能达成。例子如下：`

``` C++
 #include <cstdlib>
 #include <iostream>
 using namespace std;

 int fcmp(const void* elem1, const void* elem2);

 int main(){
     int ia[10] = {32, 97, 62, 22, 43, 25, 52, 59, 54, 9};

     for (int i=0; i<10; i++)
         cout << ia[i] << " ";

     cout << endl;

     qsort(ia, sizeof(ia)/sizeof(int), sizeof(int), fcmp);

     for (int i=0; i<10; i++)
         cout << ia[i] << " ";

     return 0;
 }

 int fcmp(const void* elem1, const void* elem2){
     const int* i1 = (const int*)elem1;
     const int* i2 = (const int*)elem2;

     if(*i1 < *i2)
         return -1;
     else if(*i1 == *i2)
         return 0;
     else if(*i1  *i2)
         return 1;    
 }
``` 

　　`函数指针的缺点：无法持有自己的状态（局部状态），也无法达到组件技术中的可适配性——也就是无法再将某些修饰条件加诸于其上而改变其状态。`

## 1.2 为此，引入仿函数的概念。STL算法的特殊版本所接受的所谓“条件”或“策略”或“一整组操作”，都以仿函数形式呈现。
　　 针对某个class进行operator()重载，它就成为一个仿函数。
　　 　　 　　 例子如下：

``` C++
#include <iostream>
using namespace std;

// 由于将operator()重载了，因此plus成为了一个仿函数
template <class T>
struct PLUS {
    T operator() (const T& x, const T& y) const {return x + y;}
};

// 由于将operator()重载了，因此minus成为了一个仿函数
template <class T>
struct MINUS {
    T operator() (const T& x, const T& y) const {return x - y;}
};

int main(){
    // 产生仿函数对象
    PLUS<int> plusObj;
    MINUS<int> minusObj;

    // 使用仿函数，就像使用一般函数一样
    cout << plusObj(3, 5) << endl;
    cout << minusObj(3, 5) << endl << endl;

    // 直接产生仿函数的临时对象（第一对小括号），并调用（第二对小括号）
    cout << PLUS<int>() (7, 6) << endl;
    cout << MINUS<int>() (7, 6) << endl;
}
```

　　 上述的 PLUS 和 MINUS 已经非常接近STL的实现了，唯一的差别在于它缺乏“可适配能力”。

## 1.3 如果要成为一个可适配的仿函数呢？
　　`将在读完第8章再进行总结。`
