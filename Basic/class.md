```cpp
/* 类前向声明.cpp */
/*
使用前向引用声明虽然可以解决一些问题，但它并不是万能的。需要注意的是，
尽管使用了前向引用声明，但是在提供一个完整的类声明之前，不能声明该类的对象，
也不能在内联成员函数中使用该类的对象。请看下面的程序段：
*/

//第一种
#include <iostream>
class Fred; //前向引用声明
class Barney {
  Fred x; //错误：类Fred的声明尚不完善
};
class Fred {
  Barney y;
};

//第二种
class Fred; //前向引用声明

class Barney {
public:
  void method() {
    x->yabbaDabbaDo(); //错误：Fred类的对象在定义之前被使用
  }

private:
  Fred *x; //正确，经过前向引用声明，可以声明Fred类的对象指针
};

class Fred {
public:
  void yabbaDabbaDo();

private:
  Barney *y;
};

/*
总结：当使用前向引用声明时，只能使用被声明的符号，而不能涉及类的任何细节。
*/
```

# 构造顺序

一、类对象成员的构造
先构造成员
再构造自身（调用构造函数）
二、派生类构造函数
派生类可能有多个基类，也可能包括多个成员对象，在创建派生类对象时，派生类的构造函数除了要负责本类成员的初始化外，还要调用基类和成员对象的构造函数，并向它们传递参数，以完成基类子对象和成员对象的建立和初始化。

**派生类只能采用构造函数初始化列表的方式向基类或成员对象的构造函数传递参数**，形式如下：

派生类构造函数名(参数表):基类构造函数名(参数表),成员对象名1(参数表),…{
    //……
}

三、构造函数和析构函数调用次序
**派生类对象的构造**

- 先构造基类
- 再构造成员
- 最后构造自身（调用构造函数）

基类构造顺序由派生层次决定：**最远的基类最先构造**
成员构造顺序和定义顺序符合
析构函数的析构顺序与构造相反



一、公有继承
1.基类中protected的成员
类内部：可以访问，
类的使用者：不能访问，
类的派生类成员：可以访问，
2.派生类不可访问基类的private成员
3.派生类可访问基类的protected成员
4.派生类可访问基类的public成员

基类              public继承         派生类

public                 ->                  public

protected          ->                  protected

private                ->                 不可访问

二、私有继承
派生类也不可访问基类的private成员

基类              private继承       派生类

public                 ->                  private

protected          ->                  private

private                ->                 不可访问

三、保护继承
派生方式为protected的继承称为保护继承，在这种继承方式下，
基类的public成员在派生类中会变成protected成员，
基类的protected和private成员在派生类中保持原来的访问权限
注意点：当采用保护继承的时候，由于public成员变为protected成员，因此类的使用者不可访问！而派生类可访问！

基类            protected继承     派生类

public                 ->                  protected

protected          ->                  protected

private                ->                 不可访问

四、派生类对基类成员的访问形式
1.通过派生类对象直接访问基类成员 
2.在派生类成员函数中直接访问基类成员 
3.通过基类名字限定访问被重载的基类成员名  

1.基类与派生类对象的关系 

基类对象与派生类对象之间存在赋值相容性。包括以下几种情况：
把派生类对象赋值给基类对象。
把派生类对象的地址赋值给基类指针。
用派生类对象初始化基类对象的引用。
反之则不行，即不能把基类对象赋值给派生类对象；不能把基类对象的地址赋值给派生类对象的指针；也不能把基类对象作为派生对象的引用。



一、构造函数和析构函数的构造规则

1、派生类可以不定义构造函数的情况 
当具有下述情况之一时，派生类可以不定义构造函数。
基类没有定义任何构造函数。
基类具有缺省参数的构造函数。
基类具有无参构造函数。
2、派生类必须定义构造函数的情况 
当基类或成员对象所属类只含有带参数的构造函数时，即使派生类本身没有数据成员要初始化，它也必须定义构造函数，并以构造函数初始化列表的方式向基类和成员对象的构造函数传递参数，以实现基类子对象和成员对象的初始化。 
3、派生类的构造函数只负责直接基类的初始化 

C++语言标准有一条规则：如果派生类的基类同时也是另外一个类的派生类，则每个派生类只负责它的直接基类的构造函数调用。
这条规则表明当派生类的直接基类只有带参数的构造函数，但没有默认构造函数时（包括缺省参数和无参构造函数），它必须在构造函数的初始化列表中调用其直接基类的构造函数，并向基类的构造函数传递参数，以实现派生类对象中的基类子对象的初始化。
这条规则有一个例外情况，当派生类存在虚基类时，所有虚基类都由最后的派生类负责初始化。

总结：
（1）当有多个基类时，将按照它们在继承方式中的声明次序调用，与它们在构造函数初始化列表中的次序无关。当基类A本身又是另一个类B的派生类时，则先调用基类B的构造函数，再调用基类A的构造函数。

（2）当有多个对象成员时，将按它们在派生类中的声明次序调用，与它们在构造函数初始化列表中的次序无关。

（3）当构造函数初始化列表中的基类和对象成员的构造函数调用完成之后，才执行派生类构造函数体中的程序代码。





多继承下的二义性：在多继承方式下，派生类继承了多个基类的成员，当两个不同基类拥有同名成员时，容易产生名字冲突问题。

虚拟继承引入的原因：重复基类，派生类间接继承同一基类使得间接基类（Person）在派生类中有多份拷贝，引发二义性。

1、虚拟继承virtual inheritance的定义
语法
class derived_class : virtual […] base_class
虚基类virtual base class
被虚拟继承的基类
在其所有的派生类中，仅出现一次

2、虚拟继承的构造次序
  虚基类的初始化与一般的多重继承的初始化在语法上是一样的，但构造函数的调用顺序不同；
  若基类由虚基类派生而来，则派生类必须提供对间接基类的构造（即在构造函数初始列表中构造虚基类，无论此虚基类是直接还是间接基类）
  调用顺序的规定：
先调用虚基类的构造函数，再调用非虚基类的构造函数
若同一层次中包含多个虚基类,这些虚基类的构造函数按它们的说明的次序调用
若虚基类由非基类派生而来,则仍然先调用基类构造函数,再调用派生类构造函数
3、虚基类由最终派生类初始化 
在没有虚拟继承的情况下，每个派生类的构造函数只负责其直接基类的初始化。但在虚拟继承方式下，虚基类则由最终派生类的构造函数负责初始化。
在虚拟继承方式下，若最终派生类的构造函数没有明确调用虚基类的构造函数，编译器就会尝试调用虚基类不需要参数的构造函数（包括缺省、无参和缺省参数的构造函数），如果没找到就会产生编译错误。

```cpp
#include <iostream>
using namespace std;
class A {
public:
  void vf() { cout << "I come from class A" << endl; }
};
class B : public A {};
class C : public A {};
/**
class B : virtual public A {};
class C : virtual public A {};
*/
class D : public B, public C {};

int main() {
  D d;
  d.vf(); // error
  
  return 0;
}
```

```cpp
/* 派生类初始化.cpp */
#include <iostream>
using namespace std;
class A {
  int a;

public:
  A(int x) {
    a = x;
    cout << "Virtual Bass A..." << endl;
  }
};
class B : virtual public A {
public:
  B(int i) : A(i) { cout << "Virtual Bass B..." << endl; }
};
class C : virtual public A {
  int x;

public:
  C(int i) : A(i) {
    cout << "Constructing C..." << endl;
    x = i;
  }
};
class ABC : public C, public B {
public:
  //虚基类由最终派生类初始化
  ABC(int i, int j, int k)
      : C(i), B(j), A(i) // L1，这里必须对A进行初始化
  {
    cout << "Constructing ABC..." << endl;
  }
};
int main() {
  ABC obj(1, 2, 3);
  
  return 0;
}
```



```cpp
/* 虚基类调用次序(重要).cpp */
//重要!!!
#include <iostream>
using namespace std;
class A {
  int a;

public:
  A() { cout << "Constructing A" << endl; }
};
class B {
public:
  B() { cout << "Constructing B" << endl; }
};
class B1 : virtual public B, virtual public A {
public:
  B1(int i) { cout << "Constructing B1" << endl; }
};
class B2 : public A, virtual public B {// D中B类就不会再call，但是A还是会，因为不是vi
public:
  B2(int j) { cout << "Constructing B2" << endl; }
};
class D : public B1, public B2 {
public:
  D(int m, int n) : B1(m), B2(n) { cout << "Constructing D" << endl; }
  A a;
};

int main() {
  D d(1, 2);
  
  return 0;
}
Constructing B
Constructing A
Constructing B1
Constructing A
Constructing B2
Constructing A
Constructing D
```

