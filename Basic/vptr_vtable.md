## 1.基础理论

为了实现虚函数，C ++使用一种称为虚拟表的特殊形式的后期绑定。该虚拟表是用于解决在动态/后期绑定方式的函数调用函数的查找表。虚拟表有时会使用其他名称，例如“vtable”，“虚函数表”，“虚方法表”或“调度表”。

虚拟表实际上非常简单，虽然用文字描述有点复杂。首先，**每个使用虚函数的类（或者从使用虚函数的类派生）都有自己的虚拟表**。该表只是编译器在编译时设置的静态数组。虚拟表包含可由类的对象调用的每个虚函数的一个条目。此表中的每个条目只是一个函数指针，指向该类可访问的派生函数。

其次，编译器还会添加一个隐藏指向基类的指针，我们称之为vptr。vptr在创建类实例时自动设置，以便指向该类的虚拟表。与this指针不同，this指针实际上是编译器用来解析自引用的函数参数，vptr是一个真正的指针。

因此，它使每个类对象的分配大一个指针的大小。这也意味着vptr由派生类继承，这很重要。

# void*

从int * 到 char *的转化不兼容，但是编译能通过。

将void *和其他类型的指针相互赋值不出现警告，这在c语言中是允许的。

在c++中类型要求很高，不允许不同类型指针之间的相互赋值，void *指针例外(任何类型指针都可以赋值给void指针，但是反之不行。也就是说，如果需要void指针类型转化成其他指针类型，需要将此NULL类型强制转换。

那么c/c++中void指针的有什么用呢？

 \- . 当参数可以是任意类型的指针的时候，就用void *

 \- . 使用void *指针之前要进行强制转换。

4.使用void指针要小心===

-. void指针不能进行算法操作



## 2.实现与内部结构

下面我们来看自动与手动操纵vptr来获取地址与调用虚函数！

开始看代码之前，为了方便大家理解，这里给出调用图：

![base](img/base.jpg)

代码全部遵循标准的注释风格，相信大家看了就会明白，不明白的话，可以留言！

```c++
/**
 * @file vptr1.cpp
 * @brief C++虚函数vptr和vtable
 * 编译：g++ -g -o vptr vptr1.cpp -std=c++11
 * @author 光城
 * @version v1
 * @date 2019-07-20
 */

#include <iostream>
#include <stdio.h>
using namespace std;

/**
 * @brief 函数指针
 */
typedef void (*Fun)();

/**
 * @brief 基类
 */
class Base
{
    public:
        Base(){};
        virtual void fun1()
        {
            cout << "Base::fun1()" << endl;
        }
        virtual void fun2()
        {
            cout << "Base::fun2()" << endl;
        }
        virtual void fun3(){}
        ~Base(){};
};

/**
 * @brief 派生类
 */
class Derived: public Base
{
    public:
        Derived(){};
        void fun1()
        {
            cout << "Derived::fun1()" << endl;
        }
        void fun2()
        {
            cout << "DerivedClass::fun2()" << endl;
        }
        ~Derived(){};
};
/**
 * @brief 获取vptr地址与func地址,vptr指向的是一块内存，这块内存存放的是虚函数地址，这块内存就是我们所说的虚表
 *
 * @param obj
 * @param offset
 *
 * @return 
 */
Fun getAddr(void* obj,unsigned int offset)
{
    cout<<"======================="<<endl;
    void* vptr_addr = (void *)*(unsigned long *)obj;  //64位操作系统，占8字节，通过*(unsigned long *)obj取出前8字节，即vptr指针
    printf("vptr_addr:%p\n",vptr_addr);
    
    /**
     * @brief 通过vptr指针访问virtual table，因为虚表中每个元素(虚函数指针)在64位编译器下是8个字节，因此通过*(unsigned long *)vptr_addr取出前8字节，
     * 后面加上偏移量就是每个函数的地址！
     */
    void* func_addr = (void *)*((unsigned long *)vptr_addr+offset);
    printf("func_addr:%p\n",func_addr);
    return (Fun)func_addr;
}
int main(void)
{
    Base ptr;
    Derived d;
    Base *pt = new Derived(); // 基类指针指向派生类实例
    Base &pp = ptr; // 基类引用指向基类实例
    Base &p = d; // 基类引用指向派生类实例
    cout<<"基类对象直接调用"<<endl;
    ptr.fun1();
    cout<<"基类引用指向基类实例"<<endl;
    pp.fun1(); 
    cout<<"基类指针指向派生类实例并调用虚函数"<<endl;
    pt->fun1();
    cout<<"基类引用指向派生类实例并调用虚函数"<<endl;
    p.fun1();
    
    // 手动查找vptr 和 vtable
    Fun f1 = getAddr(pt, 0);
    (*f1)();
    Fun f2 = getAddr(pt, 1);
    (*f2)();
    delete pt;
    return 0;
}
```

运行结果：

```
基类对象直接调用
Base::fun1()
基类引用指向基类实例
Base::fun1()
基类指针指向派生类实例并调用虚函数
Derived::fun1()
基类引用指向派生类实例并调用虚函数
Derived::fun1()
=======================
vptr_addr:0x401130
func_addr:0x400ea8
Derived::fun1()
=======================
vptr_addr:0x401130
func_addr:0x400ed4
DerivedClass::fun2()
```

我们发现C++的动态多态性是通过虚函数来实现的。简单的说，通过virtual函数，指向子类的基类指针可以调用子类的函数。例如，上述通过基类指针指向派生类实例，并调用虚函数，将上述代码简化为：

```c++
Base *pt = new Derived(); // 基类指针指向派生类实例
cout<<"基类指针指向派生类实例并调用虚函数"<<endl;
pt->fun1();
```

其过程为：首先程序识别出fun1()是个虚函数，其次程序使用pt->vptr来获取Derived的虚拟表。第三，它查找Derived虚拟表中调用哪个版本的fun1()。这里就可以发现调用的是Derived::fun1()。因此pt->fun1()被解析为Derived::fun1()!