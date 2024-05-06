# item 1 :  \*p++、(\*p)++和\*++p

1. `*p++`---先取指针p指向的值（数组第一个元素），再将指针p自增1；后面的p就是加1后的
   *号和++（单目运算）两个处于同一优先级，结合方向是自右向左，但是前提是当++在变量前面的时候才处理同一优先级，当++在变量之后时，++的优先级可以看成最低级的，比`逗号运算符`的优先级还低。

2. `(*p)++`---先取指针p指向的值（数组第一个元素），再将该值自增1；

3. `*++p`---先将指针p自增1（此时指向数组第二个元素），再取出该值；

4. `++*p`---先取指针p指向的值（数组第一个元素），再将该值自增1；



# item 2 ： NULL、0 和 nullptr

1. NULL表示空指针。它实际上是个为0的int、long类整型值

2. C++ 11新增类型nullptr_t，它只有一个值nullptr, 可以避免整型和指针的误用。



# item 3 : C++ 虚函数和多态

1. 虚函数是在基类中使用virtual声明的函数。当在编写类函数代码的时候，不确定调用的基类的类函数代码还是派生类的类函数代码，即需要在运行时刻确定（动态绑定），所以定义为"虚"函数。
2. 当在基类中没有具体的实现、但要求每个派生类必须实现此类函数时，需要纯虚函数。其在基类中声明但不实现，且在函数原型后必须加"=0"，
     如 virtual void function()=0;
3. 多态就是多种形态、动态绑定。多态和虚函数紧密相连，其意味着：当对象调用类函数时，根据对象类型来执行不同的函数。
4.  更多需要注意的点：
      1. 在很多情况下，基类本身生成对象是不合情理的。例如，动物作为一个基类可以派生出老虎、孔雀等子类，但动物本身生成对象明显不合常理，
         所以需要纯虚函数以实现多态。含有纯虚函数的类称为抽象类，它不能生成对象；
      2. 友元不是成员函数，只有成员函数才可以是虚拟的，因此友元不能是虚拟函数；
      3. 析构函数需要定义为虚函数。如果基类析构是虚函数，则多态时delete基类指针所指派生类对象，会先调用子类析构再调用父类析构。否则只会调用父类的析构函数



# item 4 ：内存四区

1. 代码区：你所写的所有代码都会放入到代码区中，代码区的特点是共享（频繁执行）和只读（安全）

2. 全局区：全局变量、静态变量、常量（如字符串常量）

   2.1. data区：主要存放的是已经初始化的全局变量、静态变量和常量

   2.2. bss区： 未初始化的全局变量、静态变量，这些未初始化的数据在程序执行前会自动被系统初始化为0或者NULL

   2.3. 常量区：存放的是常量，如const修饰的全局变量、字符串常量等。

3. 栈区：栈是一种先进后出的内存结构，由编译器自动分配释放，存放函数的参数值、返回值、局部变量等。在程序运行过程中实时加载和释放，因此，局部变量的生存周期为申请到释放该段栈空间.

4. 堆是一个大容器，它的容量要远远大于栈，但没有栈那样先进后出的顺序。用于动态内存分配。堆在内存中位于BSS区和栈区之间。一般由程序员分配和释放，若程序员不释放，程序结束时由操作系统回收



# item 5 : debug and release

1. Debug版本和Release版本其实就是优化级别的区别，Debug称为调试版本，编译的结果通常包含有调试信息，没有做任何优化，方便开发人员进行调试，Release称为发布版本，不会携带调试信息，同时编译器对代码进行了很多优化，使代码更小，速度更快，发布给用户使用，给用户使用以更好的体验。但[Release模式编译](https://www.zhihu.com/search?q=Release模式编译&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"answer"%2C"sourceId"%3A1720297063})比Debug模式花的时间也会更多

2. Debug模式会多申请一部分空间，分布在内存块的前后，用于存放调试信息。

3. 对于未初始化的变量，Debug模式下会默认对其进行初始化，而Release模式则不会，所以就有个常见的问题，[局部变量](https://www.zhihu.com/search?q=局部变量&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"answer"%2C"sourceId"%3A1720297063})未初始化时。
4. Debug模式下可以使用assert，运行过程中有异常现象会及时crash，Release模式下模式下不会编译[assert](https://www.zhihu.com/search?q=assert&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"answer"%2C"sourceId"%3A1720297063})
5. Release模式下定义NDEBUG宏，[预处理器](https://www.zhihu.com/search?q=预处理器&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"answer"%2C"sourceId"%3A1720297063})就是根据对应宏来判断是否开启assert的
6. 数据溢出问题，在一个函数中，存在某些从未被使用的变量，且函数内存在数据溢出问题，在Debug模式下可能不会产生问题，因为不会对该变量进行优化，它在栈空间中还是占有几个字节，但是Release模式下可能会出问题，Release模式下可能会优化掉此变量，栈空间相应变小，数据溢出就会导致栈内存损坏



# item 6 ：GDB

- 第一步：使用 **gcc/g++ -g test1.c test2.c -o test** 编译链接
- 第二步：使用 **gdb test** 启动gdb调试
- 第三步：使用 **break(简写b) testx.c:linenumber** 或者 **break(简写b) testx.c:functionname**来给特定文件的特定行或者特定函数处添加断点
- 第四步：使用 **run(简写r)** 来运行程序，程序会在断点处停住
- 第五步：此时若是多线程程序，则会有多个线程同时运行，每个线程都会有自己的运行堆栈，可以使用**info threads** 来查看所有的线程情况， 使用**thread ID**来切换线程， 使用 **backtrace(bt)**来查看每个线程的调用堆栈可以来了解运行逻辑

**注意：** 在多线程程环境下，break设置断点默认是给所有线程设置，所以在调试多线程时，可能因为CPU时间片在不同线程之间的切换导致其他线程运行到更早的断点处，所以为了只让当前线程运行，可以设置 **set scheduler-locking on**来锁定当前线程，让其他线程不运行！

```
break(b) + [文件名]:[行号/函数名] thread [threadID/all] ， 加断点
info break(b)，显示所有断点信息
disable/enable 断点号， 让特定断点失效或重启，若不加断点号，则针对所有断点
next(n)， 跳到下一行，若是函数也不进入
step(s)，跳进函数
until(u) + 行号， 跳到指定行
continue(c)，程序继续运行至下一个断点处停住
finish(fi)，结束当前调用函数，到上一层函数调用处
return，强制函数忽略还没有执行的语句并返回
print(p) + 变量/*array@len， 打印变量值或者len长度的array数组的值
display + 变量， 每次都自动显示变量的值
watch + 变量， 监视变量的值，只要改变，就会显示出来
layout + src, 一边gdb调试，一边看运行到哪块源代码，很方便
```

关于多线程调试：

1. **set scheduler-locking on** 可以用来锁定当前线程，只观察这个线程的运行情况， 当锁定这个线程时， 其他线程就处于了暂停状态， 也就是说你在当前线程执行 next、step、until、finish、return 命令时，其他线程是不会运行的。
2. **set scheduler-locking step** 也是用来锁定当前线程，当且仅当使用 next 或 step 命令做单步调试时会锁定当前线程， 如果你使用 until、finish、return 等线程内调试命令，但是它们不是单步命令，所以其他线程还是有机会运行的。相比较 on 选项值，step 选项值给为单步调试提供了更加精细化的控制，因为通常我们只希望在单步调试时，不希望其他线程对当前调试的各个变量值造成影响。
3. **set scheduler-locking off** 用于关闭锁定当前线程



# item  7 : STL 线程安全

首先，STL容器都是非线程安全的！

(1)第一点：自己实现相关的简单的数据结构，其中在所有的成员方法中，都获取锁、释放锁，在push和pop时还要配合使用信号量或者条件变量

(2)第二点：使用STL自带的容器，首先要避免会扩容的容器，如果要使用会扩容的容器，需提前固定好容器大小，然后不适用push或insert，用[]来代替，否则会出现其他线程迭代器失效的情况，例如vector，使用resize 提前固定大小(reserve只是预留空间可以push_back，但是不能[]访问大于end_的位置)。 对不会扩容或者固定大小的扩容容器，此时要对容器的每个操作都要封装出一个对应的函数，在每个函数内用锁搭配信号量或者条件变量实现



reserve是预留空间大小，在vector初始化的时候，如果调用vector(){...}，则begin_=end_，也就是size() = end_-begin_ = 0, 但是cap_ = 16; 若调用vector(n){...}或者vector(n, v)， 此时end_ = begin_ + n, cap_ = begin_ + max(n, 16)， reserve预留的空间可以干嘛？？ 当你push_back的时候，其是判断 end_ != cap_则插入，否则构造新的空间再插入，所以reserve的空间可以用来插入 新的元素，但是！！，当你调用operator[]来访问元素时，其判断是 n < size(),而size返回end_ - begin_， 也就是访问的时候并不能访问超过end_的在reserve空间内的元素！！

resize是改变vector内实际元素的个数，毕竟size()函数也是返回vector中的实际元素的个数end_ - begin_，其改变的过程中有三点：（1）当resize的个数小于size()时，先remove和destroy多余的对象，cap_不变； （2）当resize的个数大于等于size()并且小于cap_时，其会插入value对象，cap_不变；（3）当resize的个数大于cap_时，分配新的空间，然后复制Old元素插入新的元素，并拥有一个更大的cap_



# item 8 ：allocator

new和delete将内存申请释放和对象构造析构联系在一起，同时如果定义数组的话，那么整块内存的对象的构造析构都是一样的。
那么，为了在一块内存上按需构造函数，需要将内存申请和对象构造分离。所以出现了allocator,头文件\<memory>

是原始内存



# item 9 ：const

                C++ const与指针
               
        1. 当const在*的前面时，允许修改指针p的位置，而不允许修改*p的值
        2. 当const在*的后面时，指针p不能变，*p可以变
        3. 当const在*的前后都有时，指针p不能变，*p也不可以变
        4. 顶层const表示const定义的变量本身是个常量， 底层const表示定义变量所指对象是一个常量
        https://www.jianshu.com/p/fbbcf11100f6
        http://wangwlj.com/2018/01/06/CPP_06/
        
                C++ const与成员函数
        https://www.cnblogs.com/wintergrass/archive/2011/04/15/2015020.html
        https://blog.csdn.net/audience_fzn/article/details/80455236
        const放在返回值类型前面修饰返回值为常量，放在函数后面修饰该函数为常量函数。
        在类中将成员函数修饰为const，const修饰this指针指向的对象，这也就保证调用这个const成员函数的对象在内部不会被改变
        表明在该函数体内，不能修改对象的数据成员而且不能调用非const函数。
        为什么不能调用非const函数？因为非const函数可能修改数据成员，const成员函数是不能修改数据成员的，
        所以在const成员函数内只能调用const函数
        众所周知，在相同参数及相同名字的情况下，const是可以构成函数重载的，但const成员函数不能更改任何非静态成员变量;
    
        类中二函数都存在的情况下：
    
        const对象默认调用const成员函数，非const对象默认调用非const成员函数；
    
        若非const对象想调用const成员函数，则需显式转化，如(const Student&)obj.getAge();
    
        若const对象想调用非const成员函数，同理const_cast<Student&>(constObj).getAge();(注意：constObj要加括号)
    
        类中只有一函数存在的情况下：
    
        非const对象可以调用const成员函数或非const成员函数；
    
        const对象只能调用const成员函数,直接调用非const函数时编译器会报错；
        
        1）const成员函数可以访问非const对象的非const数据成员、const数据成员，也可以访问const对象内的所有数据成员；
    
        2）非const成员函数可以访问非const对象的非const数据成员、const数据成员，但不可以访问const对象的任意数据成员；
    
        3）作为一种良好的编程风格，在声明一个成员函数时，若该成员函数并不对数据成员进行修改操作，应尽可能将该成员函数声明为const 成员函数。



# item 10 ：cout

cout 的计算顺序是从右向左，输出为从左向右

作为输出流，有个缓冲区，先从右到左读入buffer，再从左到右弹出



# item 11 ：explicit

```
#include <iostream>
using namespace std;

/*******************************************
                    C++ explicit

    按默认规定，传一个参数的构造函数也定义了一个隐式转换,
    多个参数，只有一个无默认值，其他参数都有默认值的情况也是这样

**********************************************/

#include <iostream>

using namespace std;

class Rational {

public:
	// explicit Rational(int numerator = 0, int denominatoro = 1) :a(numerator), b(denominatoro) { cout << "a = " << a << "b" << b << endl; }  //如果限制隐式转换，oneEight * 2编译不过
	Rational(int numerator = 0, int denominatoro = 1) :a(numerator), b(denominatoro) { cout << "a = " << a << "b" << b << endl; }
	const Rational operator*(const Rational &rhs)
	{
		return Rational(a*rhs.a, b*rhs.b);
	}
private:
	int a, b;
};

int main()
{
	Rational oneEight(1, 8);
	Rational result = oneEight * 2;  //相当于oneEight.operator*(2),编译器知道传递了一个int，而函数需要的是Rational,而它又发现构造函数支持隐式转换，所以先调用构造函数，再进行运算
	// 等价于 const Rantional tmp(numerator=2), result = oneEight * tmp;
	return 0;
}
```



# item 12 : extern "C"

[extern "C"实现C++与C语言的混编，gcc会根据文件后缀.c和.cpp来实现不同编译风格，.c按照C语言的编译风格即变量和函数名字不会变，.cpp会根据C++语言的编译风格即变量和函数会根据所在范围类型、函数名字、函数参数个数和类型来区分，以实现重载，在C++中想使用C语言实现的代码时，使用extern "c"来包裹这部分代码，告诉C++编译器这部分在外部C语言文件中，应该以C语言编译的方式去链接寻找！]

extern可以置于变量或者[函数](http://www.bc-cn.net/Article/Search.asp?Field=Title&ClassID=&keyword=%BA%AF%CA%FD)前，以标示变量或者[函数](http://www.bc-cn.net/Article/Search.asp?Field=Title&ClassID=&keyword=%BA%AF%CA%FD)的定义在别的文件中，提示编译器遇到此变量和[函数](http://www.bc-cn.net/Article/Search.asp?Field=Title&ClassID=&keyword=%BA%AF%CA%FD)时在其他模块中寻找其定义。

extern是C/C++语言中表明函数和全局变量**作用范围**（可见性）的关键字 .
它告诉编译器，其**声明**的函数和变量可以在本模块或其它模块中使用。
1。对于extern变量来说，仅仅是一个变量的声明，其并不是在定义分配内存空间。如果该变量定义多次，会有连接错误
2。通常，在模块的头文件中对本模块提供给其它模块引用的函数和全局变量以关键字extern声明。也就是说c文件里面定义，如果该函数或者变量与开放给外面，则在h文件中用extern*加以声明。所以外部文件只用include该h文件就可以了。而且编译阶段，外面是找不到该函数的，但是不报错。link阶段会从定义模块生成的目标代码中找到此函数。
3。对应的关键字是static，被它修饰的全局变量和函数只能在本模块中使用。



# item 13 : 编译过程

C编译过程主要包括预编译、编译、汇编、链接四大过程

1. 预编译就是将你写好的源文件进行预处理，主要是对以'#'开始（没有';'结束）的语句和注释进行处理,gcc -E hello.c -o a.i；
2. 编译就是对预处理之后的源文件处理成汇编语言， gcc -S hello.c -o a.s;
3. 汇编就是对编译之后生成的汇编语言文件处理成机器二进制，生成目标文件 gcc -c a.s -o a.o
4. 链接就是对汇编生成的各目标文件和需要的库链接在一起生成可执行文件 gcc a.c -I 要链接的目录要链接的目录 x1.o x2.0.. 上述四个步骤可直接简化成 gcc hello.c -o a



预处理

```
gcc -E test.c -o test.i 或 gcc -E test.c
```

编译为汇编代码

```
gcc -S test.i -o test.s
```

汇编

```
gcc -c test.s -o test.o
```

链接

对于上一小节中生成的test.o，将其与Ｃ标准输入输出库进行连接，最终生成程序test

```
gcc test.o -o test
```



检错

```
gcc -Wall illcode.c -o illcode
```

所以，在编译程序时带上-Werror选项，那么GCC会在所有产生警告的地方停止编译，迫使程序员对自己的代码进行修改，如下：

```
gcc -Werror test.c -o test
```



有一个include文件夹，里面包含mysql connectors的头文件，还有一个lib文件夹，里面包含二进制so文件libmysqlclient.so

其中inclulde文件夹的路径是/usr/dev/mysql/include,lib文件夹是/usr/dev/mysql/lib

##### 编译成可执行文件

首先我们要进行编译test.c为目标文件，这个时候需要执行

```
gcc –c –I /usr/dev/mysql/include test.c –o test.o
```

##### 链接

最后我们把所有目标文件链接成可执行文件:

```
gcc –L /usr/dev/mysql/lib –lmysqlclient test.o –o test
```

Linux下的库文件分为两大类分别是动态链接库（通常以.so结尾）和静态链接库（通常以.a结尾），二者的区别仅在于程序执行时所需的代码是在运行时动态加载的，还是在编译时静态加载的。

默认情况下， GCC在链接时优先使用动态链接库，只有当动态链接库不存在时才考虑使用静态链接库，如果需要的话可以在编译时加上-static选项，强制使用静态链接库。



# item 14 ：lambda

捕获值

1. 空，没有任何使用函数对象参数。
2. =，函数体内可以使用lambda所在作用范围所有可见局部变量（包括lambda所在类的this） 值传递
3. &，函数体内可以使用lambda所在作用范围所有可见局部变量（包括lambda所在类的this）引用传递
4. this，函数体内可以使用lambda所在类的成员变量
5. =(or &)， a，&b： a按值，b按引用 其他按值（or 按引用）



# item 15 ： 异常

```cpp
try
{
    throw 0;
}
catch(int e)
{
    if(e==0)
        break;
}

```



# item 16 : 断言

1. assert   断言，运行时判断，无要求，比较方便

2. static_assert  静态断言，编译时判断，需要判断的表达式是常量，即在编译时即可运算的，所以判断的范围比较局限





# item 17



C++ 关于指针变量所占内存空间大小的问题
    https://blog.csdn.net/cool_oyty/article/details/8078632

C++ string、char*、char[],sizeof不是函数，更像宏
    https://blog.csdn.net/lanchunhui/article/details/50738498





# item 18 ： 虚函数表

 C++ 虚函数表
    https://blog.csdn.net/lihao21/article/details/50688337
    虚表是在编译阶段就确定的，每个元素是一个虚函数指针，此时的虚表属于类，不属于对象
    每个对象都有一个虚表指针 *_vptr指向类的虚表，虚表指针在生成对象时自动生成
    普通的函数即非虚函数，其调用并不需要经过虚表，所以虚表的元素并不包括普通函数的函数指针
    虚函数调用需要经过虚表
    继承时会继承虚表，更新重写的虚函数 https://bbs.csdn.net/topics/390454343
    所以会实现多态



# item 19 ：指针和引用

https://www.cnblogs.com/dolphin0520/archive/2011/04/03/2004869.html





# item 20 ：vector

 Vector对象是如何增长的：
      为了支持快速访问，vector将元素连续存储。在连续存储的前提下，且容器的大小是可变的，考虑向vector或string中添加元素会发生什么？
    如果没有空间容纳新元素，容器不可能简单地将它添加到内存中其他位置---因为内存必须连续存储。
      容器必须分配新的内存空间来保存已有元素和新元素，将已有元素从旧位置移动到新空间中，然后添加新元素，释放旧存储空间。
      如果我们每次添加一个新元素，vector就执行一次这样的内存分配和释放操作，性能会慢到不能接受。
      为了避免这种代价，标准库采用了可以减少容器空间重新分配次数的策略。当不得不获取新的内存空间时，vector和string的实现通常会
    分配比新的空间需求更大的内存空间。容器预留这些空间作为备用，可用来保存更多新元素，这样，就不需要每次添加新元素都重新分配内存了。



# item 21 ： 继承

 C++ 继承
        
    1. 创建派生类时，会先调用基类构造函数，再调用派生类构造函数
    2. 销毁派生类时，会先调用派生类析构，再调用基类析构
    3. 析构函数需要定义为虚函数，这样在实现多态时才能调用子类的析构函数；否则只会调用父类的析构函数
        非多态时，满足1和2
    4. Fruit *f;不会调用构造函数
    5.继承访问控制
             基类中      继承方式             子类中
    
          public     ＆ public继承        => public
          public     ＆ protected继承     => protected  
          public     ＆ private继承       => private
    
          protected  ＆ public继承        => protected
          protected  ＆ protected继承     => protected  
          protected  ＆ private继承       => private
    
          private    ＆ public继承        => 子类无权访问
          private    ＆ protected继承     => 子类无权访问
          private    ＆ private继承       => 子类无权访问



# item 22

// 标示符含义：
// override，表示此虚函数必定“重写”了基类中的对应虚函数。
// final，(1)作用在虚函数：表示此虚函数已处在“最终”状态，后代类必定不能重写这个虚函数。
//        (2)作用在类：表示此类必定不能被继承
// 编译器将帮你检查是否“必定”
//所以，实现多态(只有继承中的virtual重写才能实现多态(取决于等号右边new出来的对象)，无virtual的重写和继承中的重载都是隐藏hide，取决于等号左边)
//就必须要有虚函数，override重写必须要是虚函数，不是虚函数应该叫隐藏而不是重写





# map 和 unordered_map 的区别



1. map

   map是基于红黑树实现。红黑树作为一种自平衡二叉树，保障了良好的最坏情况运行时间，即它可以做到在O(log n)时间内完成查找，插入和删除，在对单次时间敏感的场景下比较建议使用map做为容器。比如实时应用，可以保证最坏情况的运行时间也在预期之内。

   另红黑树是一种二叉查找树，二叉查找树一个重要的性质是有序，且中序遍历时取出的元素是有序的。对于一些需要用到有序性的应用场景，应使用map。

2. unordered_map

   unordered_map是基于hash_table实现，一般是由一个大vector，vector元素节点可挂接链表来解决冲突来实现。hash_table最大的优点，就是把数据的存储和查找消耗的时间大大降低，几乎可以看成是常数时间；而代价仅仅是消耗比较多的内存。然而在当前可利用内存越来越多的情况下，用空间换时间的做法是值得的

二者的应用场景：

**在需要有序性或者对单次查询有时间要求的应用场景下，应使用map，其余情况应使用unordered_map。**





# 默认参数

https://blog.csdn.net/vlily/article/details/7247888



# 静态绑定和动态绑定

```
#include <iostream>

using namespace std;

class B {
public:
	void mf() { cout << "mfB" << endl; }  
	virtual void name(int i)
	{
		cout << "nameB" << " " << i << endl;
	}
	virtual void sex()
	{
		cout << "sexB" << endl;
	}
};

class D : public B
{
public:
	void mf(int i) { cout << "mfD" << endl; }
	virtual void name(double i)
	{
		cout << "nameD" << " " << i << endl;
	}
	virtual void sex()
	{
		cout << "sexD" << endl;
	}
};
int main()
{
	// 只有虚函数的重写（也就是多态）才是动态绑定，其行为取决于动态类型
	// 其他诸如重写的虚函数的默认参数、虚函数的覆盖、非虚函数的重写和覆盖都是静态绑定，只会调用静态类型的行为
	
	//pb1的静态类型是B*，动态类型是B*
	//pb2的静态类型是B*，动态类型是D*
	D d;
	B b;
	B *pb1 = &b;  
	B *pb2 = &d; //pb2的动态类型是D*，静态类型是B*，所以静态绑定的行为取决于静态类型

	// 很显然，pb1,pb2的静态类型是B*,而mf是非虚函数的覆盖（重写也一样），所以是静态绑定，只会调用B*的方法，而不管动态类型是什么
	pb1->mf();
	pb2->mf();

	// pb1,pb2的静态类型是B*,而name是虚函数的覆盖，所以是静态绑定，只会调用B*的方法，而不管动态类型是什么
	pb1->name(2);
	pb2->name(3);

	// pb1,pb2的静态类型是B*,而sex是虚函数的重写，所以是动态绑定，pb1调用动态类型B*的方法，pb2调用动态类型D*的方法
	pb1->sex();
	pb2->sex();

	return 0;
}
```



# 重载运算符

重载->



# 迭代器失效

[迭代器失效](https://www.cnblogs.com/fnlingnzb-learner/p/9300073.html)分三种情况考虑，也是分三种数据结构考虑，分别为数组型，链表型，树型数据结构。

**数组型数据结构：**该数据结构的元素是分配在连续的内存中，insert和erase操作，都会使得删除点和插入点之后的元素挪位置，所以，插入点和删除掉之后的迭代器全部失效，也就是说insert(*iter)(或erase(*iter))，然后在iter++，是没有意义的。解决方法：erase(*iter)的返回值是下一个有效迭代器的值。 iter =cont.erase(iter);

其实，vector扩容也会迭代器失效

**链表型数据结构：**对于list型的数据结构，使用了不连续分配的内存，删除运算使指向删除位置的迭代器失效，但是不会失效其他迭代器.解决办法两种，erase(*iter)会返回下一个有效迭代器的值，或者erase(iter++).

**树形数据结构：** 使用红黑树来存储数据，插入不会使得任何迭代器失效；删除运算使指向删除位置的迭代器失效，但是不会失效其他迭代器.erase迭代器只是被删元素的迭代器失效，但是返回值为void，所以要采用erase(iter++)的方式删除迭代器。

注意：经过erase(iter)之后的迭代器完全失效，该迭代器iter不能参与任何运算，包括iter++,*ite



# 虚函数很慢

[虚函数为什么那么慢??](https://mp.weixin.qq.com/s?__biz=MzkyODE5NjU2Mw==&mid=2247485056&idx=1&sn=5a3e5b93f6d8872aa5fdfeee4e509a9f&chksm=c21d343cf56abd2a6abfaef8caff16dd6fb835bdaf3d4901f55e26eea45a059411c1d1bd94ea&mpshare=1&scene=1&srcid=0218A758oDsFaQngFjuWy7zZ&sharer_sharetime=1613619319357&sharer_shareid=054214e3287ede8cff93de9018c6d7da#rd)

正常的函数调用：

1. 复制栈上的一些寄存器，以允许被调用的函数使用这些寄存器；
2. 将参数复制到预定义的位置，这样被调用的函数可以找到对应参数；
3. 入栈返回地址；
4. 跳转到函数的代码，这是一个编译时地址，因为编译器/链接器硬编码为二进制；
5. 从预定义的位置获取返回值，并恢复想要使用的寄存器。

而虚函数调用与此完全相同，唯一的区别就是编译时不知道函数的地址，而是：

1. 从对象中获取虚表指针，该指针指向一个函数指针数组，每个指针对应一个虚函数；
2. 从虚表中获取正确的函数地址，放到寄存器中；
3. 跳转到该寄存器中的地址，而不是跳转到一个硬编码的地址。

- 虚函数通常通过虚函数表来实现，在虚表中存储函数指针，实际调用时需要间接访问，这需要多一点时间。
- 然而这并不是虚函数速度慢的主要原因，真正原因是编译器在编译时通常并不知道它将要调用哪个函数，所以它不能被内联优化和其它很多优化，因此就会增加很多无意义的指令（准备寄存器、调用函数、保存状态等），而且如果虚函数有很多实现方法，那分支预测的成功率也会降低很多，分支预测错误也会导致程序性能下降。



# 棱形继承和虚继承

https://blog.csdn.net/c_base_jin/article/details/86036185



# 内存对齐

内存对齐主要遵循以下原则:
1. 对于结构体的各个成员，第一个成员的偏移量是0，排列在后面的成员其当前偏移量必须是当前成员类型的整数倍
2. 结构体内所有数据成员各自内存对齐后，结构体本身还要进行一次内存对齐，保证整个结构体占用内存大小是结构体内最大数据成员的最小整数倍
3. 如程序中有#pragma pack(n)预编译指令，则所有成员对齐以n字节为准(即偏移量是n的整数倍)，不再考虑当前类型以及最大结构体内类型

其实这里有点不严谨，编译器在编译的时候是可以指定对齐大小的，实际使用的有效对齐其实是取指定大小和自身大小的最小值，一般默认的对齐大小是4。
再回到上面的例子，如果默认的对齐大小是4，结构体a的其实地址为0x0000，能够被最宽的数据成员大小(这里是int， 大小为4，有效对齐大小也是4)整除，
故char a的从0x0000开始存放占用一个字节即0x0000~0x0001，然后是int b，其大小为4，故要满足2，需要从0x0004开始，所以在char a后填充三个字节，
因此a对齐后占用的空间是0x0000~0x0003，b占用的空间是0x0004~0x0007, 然后是short c其大小是2，故从0x0008开始占用两个字节，即0x0008~0x0009。
此时整个结构体占用的空间是0x0000~0x0009， 占用10个字节，10%4 ！= 0,
不满足第三个原则，所以需要在后面补充一个字节，即最后内存对齐后占用的空间是0x0000~0x000B，一共12个字节。

https://levphy.github.io/2017/03/23/memory-alignment.html
https://www.zhihu.com/question/27862634
https://www.cnblogs.com/Miranda-lym/p/5197805.html  struct\union\class都一样

那么为什么需要内存对齐呢？
---》结构体的对齐明明浪费了更多的空间，但是却提高了内存系统性能！
最重要的考虑是提高内存系统性能。为了简化电路设计，实际上计算机会将字节组成一个大点的格子，32 位机器，就以 4 个字节作为一个格子。
64 位机器，就以 8 字节作为一个格子。在 64 位机上，就算读取 1 字节的数值，也需要读取整个 8 字节的格子。例如，假设计算机总是从内存中取8个字节，
如果一个double数据的地址对齐成8的倍数，那么一个内存操作就可以读或者写，但是如果这个double数据的地址没有对齐，
数据就可能被放在两个8字节块中，那么我们可能需要执行两次内存访问，才能读写完成。显然在这样的情况下，是低效的。所以需要字节对齐来提高内存系统性能。
在有些处理器中，如果需要未对齐的数据，可能不能够正确工作甚至crash



# 移动语义

1.左值和右值：https://blog.csdn.net/xuyuqingfeng953/article/details/51058236
			https://www.jianshu.com/p/d19fc8447eaa  本篇返回值和形参传递调用拷贝构造函数和编译器返回值优化那里讲的很牛逼
2.左值引用和右值引用：https://blog.csdn.net/qianyayun19921028/article/details/80875002
	为什么要右值引用，右值引用在你需要使用寄存器中的值的时候可以进行右值引用。寄存器的刷新速度很快，
没有右值引用的话就需要将寄存器中的值拷贝到内存中，在进行使用，这是很浪费时间的。
	所以右值引用和移动语义主要是提升程序性能的。
	常量左值引用却是个奇葩，它可以算是一个“万能”的引用类型
	                                                     **核心文，讲的非常全面深入**下面
3. 移动语义：移动构造函数和移动赋值函数  https://www.jianshu.com/p/d19fc8447eaa https://segmentfault.com/a/1190000016041544 



# 特种成员函数

```cpp
#include <iostream>
#include <array>
using namespace std;


class Point {
public:
	Point() = default;  //在自定义了下面的构造函数后，如果没有此声明，则不会生成默认构造，直接Point p或p{}会出错，此时想要继续使用默认构造，可以使用=default来声明我还要继续使用你编译器默认声明的东西
	Point(double value) :x(value), y(value) {}
	constexpr Point(const double &xValue, const double &yValue) noexcept
		: x(xValue), y(yValue){}

	Point(const Point& other)  //自声明了复制构造函数之后，类不会在自动生成复制构造函数。在声明了移动操作之后，则默认复制构造函数会被删除，但使用=default会复活，
	{                          //在声明了复制赋值运算符或者析构函数的条件下，仍然生成复制构造函数的行为已被废弃（不是不允许）。复制赋值运算符和此过程一样。
		x = other.x;
		y = other.y;
	}
	//两种移动操作仅在不包括任何自定义复制、移动或析构存在的情况下才会生成
	constexpr double xValue() const noexcept { return x; }
	constexpr double yValue() const noexcept { return y; }

	inline void setX(double newX) noexcept { x = newX; }
	inline void setY(double newY) noexcept { y = newY; }
private:
	double x, y;
};

int main()
{
	//int sz = 0;
	//const auto arraysize = sz;  //const必须接收一个右值或者已经初始化的值(非const也可)，否则编译出错
	////std::array<int, arraysize> data1;  //const值不是编译期可知的量，不能用于定义数组尺寸
	//
	//const int sz2 = 10;								
	//constexpr auto arraysize2 = sz2; //constexpr必须接收一个右值或者已经初始化的const值(非const不行)，否则编译出错
	//std::array<int, arraysize2> data2;  //constexpr是编译期可知的量，可以用于定义数组尺寸

	constexpr Point p1(9.4, 25.0);  //只有加了constexpr才能使用constexpr构造函数返回的编译常量对象，此处不加，依旧不能在编译期知道p1的值

	Point p2;
}
```





# 拷贝

https://blog.csdn.net/qq_29344757/article/details/76037255  复制构造函数和深拷贝、浅拷贝
https://blog.csdn.net/u010700335/article/details/39830425

C++中类的拷贝有两种：深拷贝，浅拷贝：当出现类的等号赋值时，即会调用拷贝函数

这次深拷贝的对象res1是个右值（ResourceOwner(“res1”)的返回值），其实它马上就要被回收了。所以本身是不需要独享资源的。我把问题描述再重复一次，这次应该就好理解了：&&用来区分右值，这样在这个右值 1）是一个构造函数或赋值函数的参数，和2）对应的类包含指针，并指向一个动态分配的资源（内存）时，就可以在函数内避免深拷贝。如果深拷贝右值的资源不合理，那什么操作是合理的呢？答案是Move继续讨论move语义。解决方法很简单，如果参数是右值，就不拷贝，而是直接“搬”资源。我们先把赋值函数用右值引用重载下：ResourceOwner& operator=(ResourceOwner&& other) {
  theResource = other.theResource;
  other.theResource = NULL;
}复制代码这个新的赋值函数就叫做move赋值函数。move构造函数也可以用差不多的办法实现，这里先不赘述了。如果不太好理解的话，可以这么来：比如你卖了个旧房子搬新家，搬家的时候不一定要把家具都丢掉再买新的对伐（我们在🌰3里面就丢掉了）。你也可以把家具“搬”到新家去。

一：两个的区别
1  在未定义显示拷贝构造函数的情况下，系统会调用默认的拷贝函数——即浅拷贝，它能够完成成员的一一复制。当数据成员中没有指针时，浅拷贝是可行的；但当数据成员中有指针时，如果采用简单的浅拷贝，则两类中的两个指针将指向同一个地址，当对象快结束时，会调用两次析构函数，而导致指针悬挂现象，所以，此时，必须采用深拷贝。
2 深拷贝与浅拷贝的区别就在于深拷贝会在堆内存中另外申请空间来储存数据，从而也就解决了指针悬挂的问题。简而言之，当数据成员中有指针时，必须要用深拷贝。
二  带实例的解释
c++默认的拷贝构造函数是浅拷贝
浅拷贝就是对象的数据成员之间的简单赋值，如你设计了一个没有类而没有提供它的复制构造函数，当用该类的一个对象去给令一个对象赋值时所执行的过程就是浅拷贝



# 智能指针

### [普通指针和智能指针的区别以及为什么要使用智能指针](https://www.jianshu.com/p/e4919f1c3a28)



1》auto_ptr 是在普通指针的基础上增加了自动释放内存的功能 但其有如下三个缺点： 1）auto_ptr对象之间的赋值会移交所有权 2）不能指向一组对象 3）不能和STL容器共同使用 auto_ptr无能力自定义删除器

------

C++11的三种智能指针都在头文件中 2》**unique_ptr **是auto_ptr的改进 默认的删除函数是delete,自定义的删除器需要作为unique_ptr类型的一部分 提供了特殊方法unique_ptr<T[]>创建数组对象，并自动调用delete[]来释放内存 unique_ptr不可拷贝和赋值，正如其名字一样，这和auto_ptr不同，其可以避免因为复制和赋值导致所有权移交而带来的bug，但其允许在即将销毁时拷贝或者赋值，常见于返回值 unique_ptr使用release()方法可以释放对内存的占有权，可以利用此方法将所有权转移给另一个指针 或者使用p.reset(p2.release)来让p获取p2内存的占有权 unique_ptr sptr1( new Test[5], //这种写法虽然可以通过编译，但是其实相当于new Test,也不能使用sptr1[0] [ ](Test* p) { delete[ ] p; } ); //除非使用unique_ptr<Test []>

reset() 相当于释放当前所控制的对象 reset(T* p) 相当于释放当前所控制的对象，然后接管p所指的对象

------

3》**shared_ptr**顾名思义就是共享所有权，使用强引用来计数，只有最后一个引用结束时才会自动释放资源 析构时默认调用delete释放资源，当指向一组对象时需自定义delete[]来释放资源，自定义的删除器不需要作为shared_ptr类型的一部分 shared_ptr不支持shared_ptr<T[]>的写法，其虽然可以指向一组对象，并使用自定义的delete[]删除这组对象，但全然没有意义 因为shared_ptr未提供operator[]操作符，只能通过复杂的指针计算来访问，且array、vector那么方便，所以全然无意义 shared_ptr sptr1( new Test[5], // 这种写法无意义 [ ](Test* p) { delete[ ] p; } );
如果需要指向一组对象的智能指针，可以使用Boost中的shared_array：shared_array text(new char[1000]); 缺点： 1）不同的指针指向同一个资源,即不要使用裸指针来创建智能指针，这样会导致析构两次，当然这个问题所有的智能指针都有 2）循环引用导致资源不能正确释放

#### 循环引用



```
class B;
class A
{
public:
 A(  ) : m_sptrB(nullptr) { };
 ~A( )
 {
  cout<<" A is destroyed"<<endl;
 }
 shared_ptr<B> m_sptrB;
};
class B
{
public:
 B(  ) : m_sptrA(nullptr) { };
 ~B( )
 {
  cout<<" B is destroyed"<<endl;
 }
 shared_ptr<A> m_sptrA;
};
//***********************************************************
void main( )
{
 shared_ptr<B> sptrB( new B );
 shared_ptr<A> sptrA( new A );
 sptrB->m_sptrA = sptrA;
 sptrA->m_sptrB = sptrB;
}
```



shared_array与shared_ptr的区别如下： 1：构造函数接受的指针p必须是new[]的结果，而不能是new表达式。 2：提供operator[]操作符重载，可以像普通数组一样用下标访问元素，shared_ptr却没有下标访问运算符 3：没有*、->操作符重载，因为shared_array持有的不是一个普通指针。 4：析构函数使用delete[]释放资源，而不是delete。

------

4》**weak_ptr**引入弱引用计数解决资源不能释放 weak_ptr使用shared_ptr来创建，来共享但不拥有资源，其不支持*和->来操作。 当shared_ptr创建weak_ptr对象时，shared_ptr的强引用计数不会改变，会添加一个弱引用计数，weak_ptr会继承其计数 当shared_ptr销毁时，不管weak_ptr如何，直接销毁

1. [智能指针的数组版](https://blog.csdn.net/rain_qingtian/article/details/10615575)



# 数组指针，指针数组

```cpp
#include <iostream>
#include <string>

using namespace std;

int main()
{	
	//区分这个问题核心在，*和[]的优先级：() > [] > *，以及数组指针到底是指针还是数组，指针数组到底是数组还是指针？
    //https://blog.csdn.net/sszgg2006/article/details/7317877
	//https://blog.csdn.net/qsyzb/article/details/9567463
	double(*arr1)[6] = new double[10][6];  // 数组指针，因为()优先级最高，所以arr1其实是个指针，这个指针指向了一个6个double型的一维数组，即arr1指针每加1代表它会跨过6个double的长度
	double *arr2[6];   // 指针数组，[]优先级高，所以arr2首先是个数组，其每个元素是个double*的指针
	double arr3[10][6]; // 数组数组
	double **arr4 = new double*[10];  //指针的指针

	return 0;
}
```



# 数组，new，malloc

```cpp
#include <iostream>
#include <cstdlib>

using namespace std;

/****************************************
    1. 数组、new、malloc的内存分配情况
    2. new和malloc的区别
*****************************************/

int main()
{
    int32_t array1[10] = {0};
    int32_t *array2 = new int32_t [10];
    int32_t *array3 = (int32_t *)malloc(sizeof(int32_t) * 10);
    int32_t array4[5][3] = {0};

    /****************    1    **************/

    cout << "数组、new和malloc都是从堆上分配内存" << endl;

    //array1是array1[0]的地址
    cout << "array1的值 = " << array1 << ", &array1[0] = " << &array1[0] << endl;

    //查看array1里面各元素的地址，可以看到其是连续分配
    cout << endl << "查看array1各元素的地址，其连续分配且相差4个字节，因为int32_t占用4个字节" << endl;
    for(int i = 0; i < 10; i++)
    {
        cout << array1 + i <<endl;
    }

    //查看array2里面各元素的地址，可以看到其是连续分配
    cout << endl << "查看array2各元素的地址，其连续分配且相差4个字节，因为int32_t占用4个字节" << endl;
    cout << "new出来地址是逻辑上连续，物理上可能连续也可能不连续" << endl;
    for(int i = 0; i < 10; i++)
    {
        cout << array2 + i <<endl;
    }

    //查看array3里面各元素的地址，可以看到其是连续分配
    cout << endl << "查看array3各元素的地址，其连续分配且相差4个字节，因为int32_t占用4个字节" << endl;
    cout << "malloc出来地址是逻辑上连续，物理上可能连续也可能不连续" << endl ;
    for(int i = 0; i < 10; i++)
    {
        cout << array3 + i <<endl;
    }
    cout << endl;

    //查看二维数组内存分配
    cout << "array4是array4[0]的地址 : " << "array4 = " << array4 << ", &array4[0] = " << &array4[0] << endl;
    cout << "array4[0]是array4[0][0]的地址 : " << "array4[0] = " << array4[0] << ", &array4[0][0] = " << &array4[0][0] << endl;
    cout << endl << "二维数组以一维数组的形式存放，地址之间差3*4B = 12B:" << endl;
    cout << "array4[0] = " << array4[0] << endl;
    cout << "array4[1] = " << array4[1] << endl;
    cout << "array4[2] = " << array4[2] << endl;
    cout << "array4[3] = " << array4[3] << endl;

    cout << "相邻元素之间以一维数组形式存放，地址相差4B" << endl;
    cout << "&array4[2][0] = " << &array4[2][0] << endl;
    cout << "&array4[2][1] = " << &array4[2][1] << endl;


    /****************    2    **************/
    cout << endl << "      new和malloc的区别       " << endl;
    cout << "1. new不需要指定内存类型，类型安全，而malloc返回的内存为void*，需要强制转换;" << endl;
    cout << "2. new自动确定内存大小，而malloc需要手动指定;" << endl;
    cout << "3. new会自动调用构造函数(new包括operator new申请一块内存，然后在此内存上调用构造函数，第二步类似于replacement new)，并由delete释放内存调用析构函数，malloc由free释放内存," << 
         "需要注意的是，new/delete、new[]/delete[]、malloc/free只能各自匹配使用，不能混用！" << endl;
    cout << "4. new/delete是C++的运算符，而malloc/free是C++的库函数," <<
         "不在编译器控制权限之内，不能够把执行构造函数和析构函数的任务强加。" << endl;
}
```



# 拷贝构造函数和复制函数

```cpp
#include <math.h>
#include <limits>
#include <iostream>
#include <vector>

using namespace std;


class NumberOp{
    public:
        NumberOp(){}
	    // 拷贝构造函数不要声明成 explicit,因为return调用的是隐式拷贝构造函数，因为是return是初始化新对象，所以和复制函数没关系
	    //为什么加explicit返回引用不会提示错误，因为返回引用不会构造新对象。return默认调用隐式拷贝构造函数
		NumberOp(const NumberOp &other)  //不设置引用传参的话每次都会调用拷贝构造函数，所以拷贝构造函数必须是引用
		{
			x = other.x;
			cout << "调用拷贝构造函数" << endl;
		}
        NumberOp(double x_){
            x = x_;
        }
		// + ,-等一般返回的不是引用，因为没有必要
		//如果用this去返回，这会改变原来的值，+的要求一般不要求改变原来的值
		//为何设置引用，是因为不设置引用传参的话每次都会调用拷贝构造函数，所以拷贝构造函数必须是引用
		// 同时，返回不是引用的话，返回也会调用拷贝构造函数。
        NumberOp& operator+(NumberOp a);   //+不能加&，否则n1+n2,n1的值也就直接改变了 
		// 因为= += -= 的this指针直接返回就行且=本来就要求改变原来的值
		NumberOp& operator=(NumberOp a); //必须返回，因为只有返回才能连续赋值，+号也一样，才能连续+和跟赋值运算
		double x;
};

NumberOp& NumberOp::operator+(NumberOp a){   //左值参数即this默认是引用传递，+号没必要带引用
    /*NumberOp newOp;
    newOp.x = x + a.x;
    return newOp;*/
	this->x = this->x + a.x; 
	return (*this);
}
NumberOp& NumberOp::operator=(NumberOp a){   //1. 操作符重载实现为类成员函数
    this->x = a.x;
	cout << "调用赋值运算" << endl;
    return (*this);
}


//NumberOp operator*(NumberOp a, NumberOp b){ //2. 操作符重载实现为非类成员函数(全局函数)，全局函数的return也调用的是此类的拷贝构造函数
//由于都是调用拷贝构造函数，所以即使是局部变量或者形参return，因为调用的是拷贝构造函数，所以不管是返回引用还是非引用都无bug,都是返回一个新的对象
NumberOp operator*(const NumberOp &a, const NumberOp &b){
    NumberOp newOp;
    newOp.x = a.x * b.x;
    return newOp;
}

int main()
{
    NumberOp n1(2.5), n2(3.7);
    NumberOp newN1, newN2, newN3(n1), newN4;  //NumberOp newN3(n1)等价于NumberOp newN3 = n1,都是初始化，而非赋值运算，都调用的是复制构造函数
	newN4 = n1 * n2;
	cout << newN4.x << endl;
	cout << (n1+n2).x << endl;
	cout << n1.x << endl;
	//cout <<  n1.x << endl;
	//n1*n2;
    //newN1 = n1 + n2;  //重载运算符后可以直接使用对象直接相加
    //newN2 = n1 * n2;  //重载运算符后可以直接使用对象直接相加

    return 0;
}
```



# 只在堆或者栈上创建对象

### [C++ 如何让类对象只在堆或栈上创建](https://blog.csdn.net/qq_30835655/article/details/68938861)

1. 只在堆上创建

   在堆上和在栈上创建对象都需要调用构造函数进行对象的构造，但是A a；是在栈上创建对象，A *a = new A;是在堆上创建对象

   两者都要调用构造函数

   但**在栈上创建对象时**，编译器会检查析构函数的可见性，**如果类的析构函数是私有的，则编译器不会在栈空间上为类对象分配内存。**

   所以我们只需要将析构函数私有化就可以组织直接创建对象了。由于堆上创建需要释放内存，所以若是析构函数，自然会内存泄漏。

   当然为了我们能够正确释放动态创建的对象，我们必须提供一个公有函数，该函数的唯一功能就是**删除对象本身**。

   ```
   #include<iostream>
   using namespace std;
   class test
   {
   private:
   	~test(){ cout << "test destroy" << endl; }
   public:
   	void destroy()
   	{
   		delete this;
   	}
   };
   int main()
   {
   	//test p;//编译器报错test::~test()不可访问
   	test *p = new test;
   	p->destroy();
   }
   ```

   

2. 只在栈上创建

   其实理解了这个理念，不难想到我们只需要 **让new操作符无法使用即可，**要做到这件事，我们可以将 **new操作符重载并设置为私有访问**即可。是不是很巧妙的方法~

   重载new的同时最好重载delete

   ```
   #include<iostream>
   using namespace std;
   class test
   {
   private:
   	void* operator new(size_t t){}
   	void operator delete(void* ptr){}
   public:
   	~test()
   	{
   		cout << "test destroy" << endl;
   	}
   };
   int main()
   {
   	//test *A = new test;
   	//编译器报错函数test::operator new 不可访问
   	test A;
   }
   ```



# 多态：编译和运行

```cpp
C++编译时多态和运行时多态
大家默认说的多态是运行时多态

编译时多态
编译期多态，正如其名，就是在编译期确定的一种多态性。这个在C++中主要体现在函数模板，这里需要注意的是函数重载和多态无关，很多地方把函数重载也误认为是编译期多态，这是错误的。

那么函数模板是如何体现编译期多态的呢？下面举一个简单的例子就可以明白。

// 例1：函数模板体现出编译期多态
#include <iostream>
 
template <typename T>
T add(T a, T b)
{
	T c = a + b;
	return c;
}
 
int main()
{
	int i1 = 1;
	int i2 = 2;
	int iResult = 0;
 
	iResult = add(i1, i2);
	std::cout << "The result of integer is " << iResult << std::endl;
 
	double d1 = 1.1;
	double d2 = 2.2;
	double dResult = 0;
 
	dResult = add(d1, d2);
	std::cout << "The result of double  is " << dResult << std::endl;
 
	return 0;
}
从例1中可以看到，我们定义了一个函数模板add，用来求两个数的和，这两个数的数据类型在使用时才知道。main函数中使用了两个int值的求和以及两个double值的求和，这里就体现了多态性，即在编译期，编译器根据一定的最佳匹配算法确定函数模板的参数类型到底是什么，这就体现了编译期的多种状态。

当说到多态性的时候一般都默认指运行期多态，所以编译期多态大家只要知道是如何表现的就可以了，下面重点来讨论运行期多态

运行时多态
运行期多态主要是指在程序运行的时候，动态绑定所调用的函数，动态地找到了调用函数的入口地址，从而确定到底调用哪个函数。在C++中，运行期多态主要通过虚函数来实现，并且一定要有继承关系，下面举一个简单的例子来讲解

// 例2：虚函数和继承关系体现运行期多态
#include <iostream>
 
class parent
{
public:
	parent() {}
 
	// 父类的虚函数
	virtual void eat()
	{
		std::cout << "Parent eat." << std::endl;
	}
 
	// 注意这个并不是虚函数！！！
	void drink()
	{
		std::cout << "Parent drink." << std::endl;
	}
};
 
class child : public parent
{
public:
	child () {}
 
	// 子类重写了父类的虚函数
	void eat()
	{
		std::cout << "Child eat." << std::endl;
	}
 
	// 子类覆盖了父类的函数，注意由于父类的这个函数
	// 并不是虚函数，所以不存在继承后重写的说法
	void drink()
	{
		std::cout << "Child drink." << std::endl;
	}
 
	// 子类特有的函数
	void childLove()
	{
		std::cout << "Child love playing." << std::endl;
	}
};
 
int main()
{
	parent* pa = new child();
	pa->eat();		// 运行期多态的体现！！！
	pa->drink();	// 非多态就是隐藏，取决于等号左边这里调用的还是父类的drink，所以并不是多态！！！
	// pa->childLove(); // 编译出错，父类的指针不能调用父类没有的函数
 
	return 0;
}
```





# 回调函数

回调函数就是使用一个函数指针来调用一个函数 https://www.cnblogs.com/danshui/archive/2012/01/02/2310114.html 回调函数 https://www.runoob.com/w3cnote/cpp-func-pointer.html 函数指针和类成员函数指针



# 四种cast

```
#include <iostream>

/*********************************************

    C++  static_cast  const_cast dynamic_cast reinterpret_cast

*********************************************/
void foo(int *num) //用于测试const_cast
{
    std::cout << "const_cast函数参数测试 " << *num << std::endl;
}

int main()
{
    /********  static_cast  *********/
    // 1. 强制类型转换，从量上直接从大类型转换到小类型和从小类型转换到大类型都可以, 但是指针从大类型到小类型或者小类型到大类型都不可以编译, 但是C的强制转换会编译成功,进而导致莫名其妙的错误结果,也就是说static_cast会进行安全类型检查
    // 2. 找回void *指针中存在的值
    double a = 5.3;
    int b = static_cast<int>(a);
    const char *cp = "hq";

    // 1. 强制类型转换
    std::cout << b << std::endl;
    std::string scp = static_cast<std::string>(cp);
    std::cout << scp << std::endl;

    // 2. 找回void *指针中存在的值
    void *p = &a;
    double *dp = static_cast<double *>(p);
    std::cout << *dp << std::endl;


    /********  const_cast  *********/
    // 1. const_cast绝对不能改变const修饰的常量的值
    // 2. 当一个量定义为const，函数形参要求是non_const时，可使用const_cast去掉const传递
    // 3. 当一个量是non_const，却使用一个const指针指向它，若需要修改其，使用const_cast
    //[当企图对一个const变量使用const_cast赋值给non const指针指向来修改其值时，虽然地址相同但值却不同，这是未定义且不安全的行为，尽量避免](https://www.cnblogs.com/QG-whz/p/4513136.html)
    // 4. const_cast只能对指针使用,对量直接使用编译不通过

    // 1. const_cast绝对不能改变const修饰的常量的值
    const int constant = 25;  //如果带const不能通过去const指针来修改
    //int constant = 25; // 3. 当一个量是non_const，却使用一个const指针指向它，若需要修改其，使用const_cast
    const int *constant_p = &constant;
    int *modify = const_cast<int *>(constant_p);
    *modify = 3;
    std::cout << "constant = " << constant << std::endl;
    std::cout << "*modify = " << *modify << std::endl;
    /*  错误，const_cast一般对指针去const
    int modify_2 = const_cast<int>(constant);
    modify_2 = 6;
    std::cout << "constant = " << constant << std::endl;
    std::cout << "modify_2 = " << modify_2 << std::endl;
    */
    // 2. 当一个量定义为const，函数形参要求是non_const时，可使用const_cast去掉const传递
    //foo(&constant); //error: invalid conversion from 'const int*' to 'int*' [-fpermissive]|
    foo(const_cast<int *>(&constant));
    
    
    /********  reinterpret_cast  *********/
    //什么类型转换都可以实现，但是其不会改变值，但是会从位模式上重新解释原来的值，可能会造成结果不一致，所以不建议使用
    //https://zhuanlan.zhihu.com/p/33040213
    // 有时候明知道一个变量是 int8_t类型的指针,但为了进一步保证从位模式上使用int8_t来解释,会使用此类型变换进一步完成保证
    // 有时候对量进行编解码,例如对提取到的特征结构体,直接从位模式上重新解释,映射成字符串进行存储,使用的时候再重新解码
    int num = 0x00636261;
    int *pnum = &num;
    //char *psnum = pnum;  //出错，不能直接转换
    char *ps2num = reinterpret_cast<char *> (pnum);
    //char *ps3num = static_cast<char *>(pnum);  //出错，不允许int*到char*的强制转换
    cout << hex << *pnum << endl;
    //cout << *psnum << endl;
    cout << *ps2num << endl;
    cout << ps2num << endl;
    //cout << ps3num << endl;
    //double *dum = pnum;  //出错，不能直接转换
    double *dnum = reinterpret_cast<double*>(pnum);
    //double *d2num = static_cast<double*>(pnum);  //出错，不允许int*到double*的强制转换
    cout << *dnum << endl;
    //cout << *d2num << endl;



    /********  dynamic_cast  *********/
    //用于多态类型之间进行转换，支持向上，向下、横向类型间转换，在执行期会动态检查安全性
    //用于将父类的指针或引用转换为子类的指针或引用，此场景下父类必须要有虚函数，因为dynamic_cast是运行时检查，检查需要运行时信息RTTI，而RTTI存储在虚函数表中
    //https://www.cnblogs.com/weidagang2046/archive/2010/04/10/1709226.html
    //https://blog.csdn.net/hongkangwl/article/details/21161713
    
    
    
    /***************  为什么不用C语言的强制转换方式呢？？ ************/
    1) 首先C语言的强制类型转换不够明确，比较暴力，不能进行错误检查，容易出错；
        C++的强制类型转换可能根据需求不同选择不同的cast进行转换
    2) 同时，C++的强制转换提供了类型转换安全检查，且功能强大

    return 0;
}
```



# 友元

                                                            C++ 友元
    在C++中，我们使用类对数据进行了隐藏和封装，类的数据成员一般都定义为私有成员，成员函数一般都定义为公有的，以此提供类与外界的通讯接口；
    但是，有时需要定义一些函数，这些函数不是类的一部分，但又需要频繁地访问类的数据成员，这时可以将这些函数定义为该函数的友元函数；
    除了友元函数外，还有友元类，两者统称为友元。友元类的所有成员函数都是另一个类的友元函数，都可以访问另一个类中的隐藏信息（包括私有成员和保护成员）；
    友元的作用是提高了程序的运行效率（即减少了类型检查和安全性检查等都需要时间开销），但它破坏了类的封装性和隐藏性，使得非成员函数可以访问类的私有成员
    
    1. 友元函数
    2. 友元类
    
    private：只能由1.该类中的函数、2.其友元函数访问。
            不能被任何其他访问，该类的对象也不能访问。
    
    protected：可以被1.该类中的函数、2.子类的函数、以及3.其友元函数访问。
            但不能被该类的对象访问。
    
    public：可以被1.该类中的函数、2.子类的函数、3.其友元函数访问，也可以由4.该类的对象访问。
     
    注：友元函数包括3种：设为友元的普通的非成员函数；设为友元的其他类的成员函数；设为友元类中的所有成员函数。

c++ 友元
    在C++中，我们使用类对数据进行了隐藏和封装，类的数据成员一般都定义为私有成员，成员函数一般都定义为公有的，以此提供类与外界的通讯接口；
    但是，有时需要定义一些函数，这些函数不是类的一部分，但又需要频繁地访问类的数据成员，这时可以将这些函数定义为该函数的友元函数；
    除了友元函数外，还有友元类，两者统称为友元。友元类的所有成员函数都是另一个类的友元函数，都可以访问另一个类中的隐藏信息（包括私有成员和保护成员）；
    友元的作用是提高了程序的运行效率（即减少了类型检查和安全性检查等都需要时间开销），但它破坏了类的封装性和隐藏性，使得非成员函数可以访问类的私有成员

    1. 友元函数
    2. 友元类



# 原码，反码 ，补码

**原码：** 原码的表示方法中，不能统一加减法运算。两个正数相加不会出错，如果正数和负数、负数和负数运算就会出错

------

**反码：** 为了克服原码的缺陷，统一正负数加减法运算。反码运算中，正数和负数的运算结果是正确中，其中特殊情况是两个相反数运算得到的结果是-0，也就是对于0而言在反码中有两个表示一个是0000，还有一个是1000。对于负数和负数的直接反码运算得到的结果是错误的，但是可以强制使用正数运算然后加上负号

------

**补码：** 直接统一了正负数加减法运算，所有正负数直接补码运算得到结果。并且，相反数之间运算得到的是0000，省略出了10000，便用这个1000表示-8。所以通常x位的有符号整数的表示范围是：-2^(x-1)~2^(x-1)-1

------

更多详细内容参考[文章](https://www.cnblogs.com/zhxmdefj/p/10902322.html)





# 函数形参是否引用、实际特种生成机制和move,add const的心得

```cpp
#include <iostream>
#include <fstream>
#include <cstdio>
#include <cstdlib>
#include <string>
#include <type_traits>

using namespace std;

class Test 
{
public:
	Test()
	{
		a = 0;
	}
	Test& operator=(const Test& other)
	{
		this->a = (other.a + 1);
		return (*this);
	}
	/*Test(const Test& other)
	{
		this->a = (other.a + 1);
	}*/
	int a;
};



int& test2(int &name)  //name是拿了a的引用，a不析构，name拿的内存就不会析构，所以int &c = test2(a)中c相当于一直拿这a的内存直到a被析构
{
	name = 77;
	return name;
}
int& test3(int name)  //name是按值传递，且因为是局部变量，在test3结束时name的内存就会被回收，所以name拿的内存就不会析构，所以int &d = test3(b)，d拿的和name的地址一样，但内存会被回收
{
	name = 77;
	return name;
}

template <typename T>
const T& addConst(T& other)
{
	return static_cast<const T&>(other);
}

template <typename T>
add_const_t<T>& addConst2(T& other)
{
	return static_cast<add_const_t<T>&>(other);
}

template <typename T>
typename remove_reference<T>::type&& MOVE(T&& other)
{
	return static_cast<typename remove_reference<T>::type&&>(other);
}

int main()
{
	//int a = 3;
	//int b = 4;
	//int &c = test2(a);
	//int &d = test3(b);
	//cout << a << endl;
	//cout << b << endl;
	//cout << d << endl;
	//const string cs("5555");
	//string s2("ttt");
	//auto& x = addConst(s2);
	//auto& finals = s2;
	//auto& x2 = addConst2(s2);
	//auto& finals2 = s2;

	//int m(5);
	//MOVE(m);
	//int &&mm = MOVE(m);   //上面的addConst，addConst2和MOVE的这种强制转换带返回，只是声明了原数据的另一种访问方式，而没有改变原数据，当然，在函数里面改值，肯定会改变原数据


	//Test t1();  //注意这块相当于声明了一个函数 new Test或者new Test()这两个都是创建新的对象
	Test t1;
	Test t2;
	t2 = t1;   
	Test t3 = t2;  //声明了复制赋值，依旧可以使用默认复制构造
	return 0;
}
```



# 位操作总结

1,

```
if(n & 1== 1) {
    // n 是个奇数。 
} else {
    // n 是个偶数
}
```



2，

```
a=a^b; //a=a^b 
b=a^b; //b=(a^b)^b=a^0=a 
a=a^b; //a=(a^b)^(a^b^b)=0^b=b
```



3,

```
n&(n-1) // 可以去掉最后一个1
// 用于求一个数的二进制中有多少个1
```



4，

```
// 给定一个非空整数数组，除了某个元素只出现一次以外，其余每个元素均出现两次。找出那个只出现了一次的元素。
a ^ a = 0
a ^ 0 = a
```



5，

```
// 一个整型数组里除了两个数字之外，其他的数字都出现了两次。请写程序找出这两个只出现一次的数字。
先全部^一次，找出只出现一次的两个数在位上的差异，差异位为1
然后分两组分别求两个只出现一次的数 
    vector<int> singleNumbers(vector<int>& nums) {
        int diff = 0;
        for (auto num : nums)
            diff ^= num;
        
        int right1 = 1;
        while ((diff & right1) == 0) 
            right1 <<= 1;
        
        int res1 = 0, res2 = 0;
        for (auto num : nums) {
            if ((num & right1) == 0) {
                res1 ^= num;
            } else {
                res2 ^= num;
            }
        }
        return {res1, res2};
    }
```



6，

```
// 在一个数组 nums 中除一个数字只出现一次之外，其他数字都出现了三次。请找出那个只出现一次的数字。
对32位所有位置的所有数计算1出现的次数，如果1出现的次数不是3的倍数，说明结果数的当前位为1
// 统计32位对应的所有数字的0和1的个数，肯定要么0不是3的倍数，要么1不是3的倍数
    int singleNumber(vector<int>& nums) {
        int n = nums.size();
        int res = 0;
        for (int i = 0; i < 32; i++) {
            int num1 = 0;
            for (auto num : nums) {
                if (((num >> i) & 1) == 1)
                    num1++;
            }
            
            if (num1 % 3 == 1) {
                int tmp = 1 << i;
                res += tmp;
            }
        }
        return res;
    }    
```



7， 找最右边的1

```
int right1 = 1;
while ((diff & right1) == 0) 
    right1 <<= 1;
```



[参考](https://www.sohu.com/a/451702212_115128)



# volatile and asm

volatile关键字声明的变量，其最重要的作用是：告诉编译器，某些不被编译器可知的因素也可以修改该变量的值，包括操作系统、硬件、多线程等，

所以使用这个变量的代码部分不能做编译优化，从而让编译器每次需要读取该变量的值时必须从该变量的地址（内存）中读，这样就会保证不会出错。

如果存在编译优化的话，变量值可能被某些不被编译器可知的因素偷偷修改，但是编译器从就近读过的值（优化方式）直接读取，会出错，volatile能保证代码一定不被优化，每次都从地址读取，就保证了不会出错！

[更详细的解释](https://www.huaweicloud.com/articles/e4e3ee53e594c40a9ad3da9687f94bc7.html)

GCC允许C/C++中嵌入汇编语言,以解决C/C++无法处理的问题或者使用更简介的汇编来表达

这就叫GCC内联汇编, GCC Inline ASM

__asm__ 是汇编asm的宏定义, 是一切内联汇编的开头

__volatile__ 是c++关键字volatile的宏定义, __asm__ __volatile__("指令列表")表达不要优化写的汇编指令

__asm__("":::"memory")声明此处内存可能发生了编译器察觉不到的变化,让编译器不再使用优化,老老实实从内存读取并生成不优化的汇编指令

> 它告诉编译器：这条指令（其实是空的）可能会读取任何内存地址，也可能会改写任何内存地址。那么编译器会变得保守起来，它会防止这条fence命令上方的内存访问操作移到下方，同时防止下方的操作移到上面，也就是防止了乱序，是我们想要的结果。 但这还没完，这条命令还有另外一个副作用：它会让编译器把所有缓存在寄存器中的内存变量flush到内存中，然后重新从内存中读取这些值。

[更详细的华为的帖子](https://www.huaweicloud.com/articles/fa8106916385898e3449b3ed7586f94e.html) [知乎文章](https://zhuanlan.zhihu.com/p/41872203)



# typename

http://feihu.me/blog/2014/the-origin-and-usage-of-typename/

1. typename的第一个作用
    在模板中，和class的作用一样，声明模板类型的表示字母
    例如 template <class T>和 template <typename T> 功能一样

2. typename的第二个作用
    用来说明“带依赖型别”而不是一个数据成员，如下例：



# typename ..args

```cpp
#include <functional>
#include <iostream>
using namespace std;

//https://blog.csdn.net/xs18952904/article/details/85221921

template<typename T>
void pair_comparer(T a, T b) {
	cout << "a = " << a << ", b = " << b << "  final" << endl;
}

//这里typename... 的参数args  代表了上面的普通的模版函数
template<typename T, typename... Args>
void pair_comparer(T a, T b, Args... args) {
	//cout << "args" << args... << endl;
	cout << "a = " << a << ", b = " << b << endl;
	pair_comparer(args...);
}

int main()
{
	//args 的参数可以增加   
	pair_comparer(1, 1, 2, 2, 3, 3, 4, 4); //（1）3,3,{4,4}输出3,3 （2）{4,4}解成4,4,{}调用无参数包的同名函数
}
```



# strcpy,memcpy,memmove

https://www.cnblogs.com/stonejin/archive/2011/09/16/2179248.html
https://www.jianshu.com/p/9c3784d8d8ad
https://www.cnblogs.com/stonejin/archive/2011/09/16/2179248.html



# std::tuple

std::tuple 类似于 std::pair的扩展，std::pair只能存放两个元素，而tuple可以存放任意个元素和类型，也可以将其当做结构体使用，不需要定义结构体，而又有结构体的特征

[更多具体使用](https://blog.csdn.net/sevenjoin/article/details/88420885)



# std::function and std::bind

[关于`std::function`的更多的例子](https://www.sczyh30.com/posts/C-C/cpp-stl-functional/)

[关于`std::bind`的更多的例子](https://www.jianshu.com/p/f191e88dcc80)



# static

```
C++ static
    https://www.cnblogs.com/BeyondAnyTime/archive/2012/06/08/2542315.html
                    
        对于一个完整的程序，在内存中的分布情况如下图：　
         【代码区】   【全局数据区】  【堆区】   【栈区】
一般程序的由new产生的动态数据存放在堆区，函数内部的自动变量存放在栈区。
自动变量一般会随着函数的退出而释放空间，静态数据（即使是函数内部的静态局部变量）也存放在全局数据区。全局数据区的数据并不会因为函数的退出而释放空间。

    一、 面向过程的static关键字
        1. 静态全局和局部变量
            • 变量在全局数据区分配内存，始终驻留在全局数据区，直到程序运行结束；
            • 未经初始化的静态变量会被程序首次初始化(或自动初始化为0)（自动变量的值是随机的），之后的调用便不再初始化；
            • 静态全局变量在声明它的整个文件都是可见的，而在文件之外是不可见的；
            • 静态局部变量其作用域为局部作用域，但声明周期随程序结束而不是随函数结束
        2. 静态函数
            • 在函数的返回类型前加上static关键字,函数即被定义为静态函数。静态函数与普通函数不同，它只能在声明它的文件当中可见，不能被其它文件使用
            • 其它文件中可以定义相同名字的函数，不会发生冲突

    二、 面向对象的static关键字
        1. 静态数据成员
            • 对于非静态数据成员，每个类对象都有自己的拷贝。而静态数据成员被当作是类的成员。无论这个类的对象被定义了多少个，静态数据成员在程序中也只有一份拷贝，
              由该类型的所有对象共享访问。也就是说，静态数据成员是该类的所有对象所共有的。对该类的多个对象来说，静态数据成员只分配一次内存，供所有对象共用。
              所以，静态数据成员的值对每个对象都是一样的，它的值可以更新；
            • 静态数据成员存储在全局数据区。静态数据成员定义时要分配空间，所以不能在类声明中定义。在Example 5中，语句int Myclass::Sum=0;是定义静态数据成员；
            • 静态数据成员和普通数据成员一样遵从public,protected,private访问规则；
            • 因为静态数据成员在全局数据区分配内存，属于本类的所有对象共享，所以，它不属于特定的类对象，在没有产生类对象时其作用域就可见，即在没有产生类的实例时，我们就可以操作它；
            • 静态数据成员初始化与一般数据成员初始化不同。静态数据成员初始化的格式为：
            ＜数据类型＞＜类名＞::＜静态数据成员名＞=＜值＞
            • 类的静态数据成员有两种访问形式：
            ＜类对象名＞.＜静态数据成员名＞ 或 ＜类类型名＞::＜静态数据成员名＞
        2. 静态成员函数
            • 出现在类体外的函数定义不能指定关键字static；
            • 静态成员之间可以相互访问，包括静态成员函数访问静态数据成员和访问静态成员函数；
            • 非静态成员函数可以任意地访问静态成员函数和静态数据成员；
            • 静态成员函数不能访问非静态成员函数和非静态数据成员；
            • 由于没有this指针的额外开销，因此静态成员函数与类的全局函数相比速度上会有少许的增长；
            • 调用静态成员函数，可以用成员访问操作符(.)和(->)为一个类的对象或指向类对象的指针调用静态成员函数，也可以直接使用如下格式：
            ＜类名＞::＜静态成员函数名＞（＜参数表＞） 调用类的静态成员函数。
```



# sizeof and strlen

对一个字符串进行sizeof和strlen的区别

对于一个字符串而言，可能有三种形式，一种是字符数组的形式，一种是字符指针形式，一种是string类型；

1. 对于string类型 内存占用和具体的库对string类的实现方式有关，4字节、12、28字节都有可能
2. 对于字符数组 首先如果是数组已经指定了大小，那么sizeof数组出来的结果肯定等于指定的数组的大小； 对于strlen，对于每一个字符串，其后面对自动加0，strlen遇到字符'\0'或者数字0（'\0'的ASCII码就是0）就会停止计数，若数组的长度不足以保存'\0'，那么strlen的结果将是不确定的； 如果数组没有指定大小，由字符串自己来确实； 对于sizeof，其会计算真实的字符串会有多少长度，即他会计算字符串本身长度+'\0'的长度，即使字符串本身内部有'\0'也会在字符串末尾加'\0'计数； 对于strlen，其会遇到'\0'就结束，如果字符串内部有'\0'就会提前结束
3. 对于字符指针 sizeof对于任何类型的指针都是32位机器对应4字节，64位机器对应8字节 strlen的计算方式和字符数组计算方式一样

还有一个特例就是字符数组作为函数参数时，实际是一个指针，所以对函数参数是数组的字符取sizeof时要注意



具体的例子结合上面的原理和下面这篇文章绝对会掌握和明白！

[strlen和sizeof](https://zhuanlan.zhihu.com/p/93054021?utm_source=wechat_session&utm_medium=social&utm_oi=752170274572500992)





# size_t and size_type

https://blog.csdn.net/cs_wangshuo/article/details/60146725
https://www.jianshu.com/p/164b57134bbc
在32bit Windows平台上，unsigned和std::vector<int>::size_type有同样的大小，都是32bit
在64bit Windows平台上，unsigned是32bit，但是std::vector<int>::size_type是64bit
size_type与string和vector搭配使用，包含在头文件string或者vector
size_t是全局使用，包含在头文件cstddef 



# reverse_iterator and base()

```
#include <iostream>
#include <vector>

using namespace std;
//reverse_iterator的base成员函数返回一个"对应的"正向iterator”，此说法其实并不准确。
//对于插入操作而言，直接在ri.base()（返回的是正向迭代器的位置，即ri的位置向右移一位）位置插入元素等于在反向迭代器ri的插入效果（但是insert和erase函数不支持反向迭代器，所以需要base函数）
//但是对于删除操作，并非如此。当需要把reverse_iterator转换成iterator的时候，由于ri.base()在ri的右一位，所以需要使用(ri++).base()来获取原来的需要删除的正向迭代器的位置
//关于reverse_iterator的base()函数的具体意义和使用参考（讲的简单清晰） https://www.cnblogs.com/yanqi0124/p/3808286.html
int main()
{
	vector<int> v;

	v.reserve(5);
	for (int i = 0; i < v.capacity(); i++)
		v.push_back(i);
	//cout << v.size() << endl << v.capacity() << endl;
	//for (int i = 0; i < v.size(); i++)
	//	cout << v[i] << endl;

	vector<int>::reverse_iterator ri = find(v.rbegin(), v.rend(), 3); // 如果ri = v.rbegin.    iterator i = ri.base()的话。就已经向右偏移越界了，记得不也能访问，否在崩溃
	cout << *ri << endl;
	vector<int>::iterator ib = ri.base();
	cout << *ib << endl;
	vector<int>::iterator ri_ = find(v.begin(), v.end(), 3);
	cout << *ri_ << endl;
	/*vector<int>::reverse_iterator ri2 = find(v.rend(), v.rbegin(), 3);  // 对于反向迭代器来说，对rend返回的指针，再++的话就会向左越界
	cout << *ri2 << endl;*/
	vector<int>::iterator ri2_ = find(v.end(), v.begin(), 3);  //对于迭代器来说，对end返回的指针，再++的话就会向右越界
	cout << *ri2_ << endl;


	return 0;
}
```





# new and operator new

https://www.cnblogs.com/slgkaifa/p/6887887.html
https://blog.csdn.net/zyw_ym_zone/article/details/10300793  关于delete和delete []、operator delete对 析构和内存释放的影响



# new point and new point()

```
#include <iostream>
#include <array>
#include <memory>
using namespace std;


class Point {
public:
	//Point() = default;  //在自定义了下面的构造函数后，如果没有此声明，则不会生成默认构造，直接Point p或p{}会出错，此时想要继续使用默认构造，可以使用=default来声明我还要继续使用你编译器默认声明的东西
	Point(double value) :x(value), y(value) {}
	//Point() = default;
	constexpr Point(const double &xValue = 3.3, const double &yValue = 5.0) noexcept
		: x(xValue), y(yValue){}

	Point(const Point& other)  //自声明了复制构造函数之后，类不会在自动生成复制构造函数。在声明了移动操作之后，则默认复制构造函数会被删除，但使用=default会复活，
	{                          //在声明了复制赋值运算符或者析构函数的条件下，仍然生成复制构造函数的行为已被废弃（不是不允许）。复制赋值运算符和此过程一样。
		x = other.x;
		y = other.y;
	}
	Point(Point&& other) {

	}
	//两种移动操作仅在不包括任何自定义复制、移动或析构存在的情况下才会生成
	constexpr double xValue() const noexcept { return x; }
	constexpr double yValue() const noexcept { return y; }

	inline void setX(double newX) noexcept { x = newX; }
	inline void setY(double newY) noexcept { y = newY; }
private:
	double x, y;
};

int main()
{

	constexpr Point p0(9.4, 25.0);  //只有加了constexpr才能使用constexpr构造函数返回的编译常量对象，此处不加，依旧不能在编译期知道p1的值
	auto del = [](Point *point) {delete point; };
	//std::unique_ptr<Point, decltype(del)> pPoint(new Point[5], del);
	//std::unique_ptr<Point> pPoint = new Point(1.0, 2.0);  //error  不能把裸指针直接赋值给智能指针，因为智能指针的构造函数需要为裸指针创建控制块
	//Point newp = pPoint[0];
	//cout << pPoint[0].xValue() << endl;
	std::shared_ptr<Point> p(new Point());  // 当自定了有默认值的二参数构造函数且默认构造函数不工作，new Point和new Point()都会调用这个构造函数并且使用默认值
											// 当自定了有默认值的二参数构造函数且让默认构造函数工作，此时相同于两个重载的构造函数，且new Point会优先调用默认构造函数，new Point()会调用失败，因为匹配的有两个
        									// 当自定义的二参数构造函数没有默认值时，new Point和new Point()都会调用默认构造函数，但是new Point是随机初始化,new Point()是初始化为0
	//Point p1(); //注意这块相当于声明了一个函数，返回值是Point,函数名为p1，参数列表为空， new Point或者new Point()这两个都是创建新的对象
	std::shared_ptr<Point> pf(new Point(5,6));
	cout << p->xValue() << "\t" << p->yValue() << endl;
	p.reset(new Point(1, 2));
	cout << p->xValue() << "\t" << p->yValue() << endl;
	Point *p2 = new Point(1.2, 3.5);
	p.reset(p2);
	cout << p->xValue() << p->yValue() << endl;
	auto p3 = std::make_unique<int>(5);  //true

}
```





# move and forward

```
1. 首先对万能引用的参数推导的理解（其本质是引用重叠）
   template <class T>
   void f(T&& param);  //param现在是个万能引用
   int x = 28;
   f(x);   //对f(x)调用，x是个左值，T的类型被推导为引用类型int &, 所以现在param变成 int& &&，引用重叠为int & (引用重叠只能由编译器完成，人为不行)，所以param类型是个左值引用
   f(28);  //28个右值，T的类型被推导为非引用类型int, 所以现在param变成int&&,无需发生引用重叠，所以param是个右值引用

2. move和forward的理解
   move的真实作用只是把参数类型无条件强制转换为右值（move作用就到此），那为啥有移动功能呢？是因为强转为右值后，无论调用赋值、构造函数此时都会调用移动版本
   move的示例实现,参数是个万能引用，根据1拿到T的参数类型后，拿到去除T的引用后的类型再加上&&组成右值类型，强制转换后返回。
   template<class T>
   typename remove_reference<T>::type&& move(T&& param){
       using ReturnType = typename remove_reference<T>::type&&;
       return static_cast<ReturnType>(param);
   }

   forward的真实作用和万能引用一起搭配，若参数是左值则将左值引用转发出去，若参数是右值，则将右值引用转发出去，即完美转发
   forward的实现其实是利用了引用重叠
   template <class T>
   void f(T&& param){
      someFunc(std::forward<T>(param));
   }

   template<class T>
   T&& forward(typanem remove_reference<T>::type& param){  
       return static_cast<T&&>(param);
   }

3. 推荐move用于右值引用，forward用于万能引用，因为声明为右值肯定是想提高效率用于移动最好，但是为左值时不一定需要移动，因为移动会析构原来的值
```





# b and b+

[为什么MYSQL索引使用B+树，而Mongodb使用B树？（一）](https://zhuanlan.zhihu.com/p/81273236?utm_source=wechat_session&utm_medium=social&utm_oi=752170274572500992&utm_campaign=shareopn)

[为什么MYSQL索引使用B+树，而Mongodb使用B树？（二）](https://zhuanlan.zhihu.com/p/107228878?utm_source=wechat_session&utm_medium=social&utm_oi=752170274572500992&utm_campaign=shareopn)

总结来说就是：

1. B树单一查询效率不稳定，B树每个节点都要存储索引和数据，所以每个节点页能存储的索引就会变少，导致树高度相比B+树变高，所以磁盘IO可能会比B+树多，但是B树每个节点存储数据，其单一查询可能最优是O(1)，。 所以其适合非关系型数据库的单一查询，其叶子节点没有指向下一页子节点的指针，所以不适合关系型数据库的范围查询。
2. B+树是非叶子节点只存储索引，所以树是最矮胖的，同时叶子节点间存在指向下一叶子节点的指针，所以适合范围查询！
3. 红黑树虽然是平衡树，但是其是二叉树，相比于B/B+多叉树而言，树就高很多，从而从磁盘加载索引到内存时，会导致更多的磁盘IO



# HTTPS

采用 对称加密 和 非对称加密 结合的方式来保护浏览器和服务端之间的通信安全。 对称加密算法加密数据+非对称加密算法交换密钥+数字证书验证身份=SSL安全

[好文章一](https://www.jianshu.com/p/ffafb98ffdd7)

[好文章二](https://www.jianshu.com/p/e30a8c4fa329)



# TCP

[三握：SYN_SEND,SYN_RCVD,ESTABLISHED, SYN不能携带数据但要消耗一个序号因为连接未建立，第三次握手ACK可以携带数据因为连接已经建立，第二次握手不能携带，如果不携带则不消耗序号; 四挥：FIN_WAIT_1，CLOSE_WAIT,FIN_WAIT_2,LAST_ACK, TIME_WAIT(等待2MSL),CLOSED，FIN报文可以携带数据因为连接未释放，不管携不携带，都要消耗一个序号](https://cloud.tencent.com/developer/article/1546825)



# 僵尸进程

两种情况造成僵尸进程：1.子进程退出后，父进程没有读取子进程的退出信息前，内核不会立即释放该进程的进程表表项；2.父进程退出后，子进程退出前，子进程成为孤儿进程被init进程接管，此时子进程是僵尸进程

解决1的方法：父进程调用wait或者waitpid配合SIGCHILD信号读取子进程退出信息；wait是阻塞进程，直到该进程的某个子进程运行退出，返回运行结束的子进程的PID,并将退出信息存在stat_loc参数指向的内存中,wait的 阻塞特性是不理想的。waitpid解决了这个阻塞问题，在父进程中捕获监听SIGCHILD信号，信号处理函数中调用waitpid读取该退出的进程的退出信息(《Linux高性能编程》p240)

解决2的方法：在子进程中调用[prctl(PR_SET_PDEATHSIG,SIGKILL)](https://zhuanlan.zhihu.com/p/56833833?utm_source=wechat_session&utm_medium=social&utm_oi=752170274572500992&utm_campaign=shareopn)，在父进程退出时，子进程将会收到SIGKILL信号，而进程收到该信号的默认动作则是退出。因而最后不会看到它成为孤儿进程，被其他进程所收养。需要注意的是，该函数并非所有系统都支持





# 单例模式

单例模式分为懒汉模式和饿汉模式

懒汉模式有三种情况：不加锁非线程安全的懒汉、加锁线程安全的懒汉、局部静态变量不加锁的线程安全的懒汉

饿汉模式本身就线程安全！

------

[深度好文](https://www.cnblogs.com/xiaolincoding/p/11437231.html)



# 多进程和多线程

1. 首先进程是资源分配的基本单位，由于其[进程切换](https://blog.csdn.net/hjt1036/article/details/99169783)开销比较大，需要更换页面空间，而线程则不用，所以提出线程成为cpu调度的基本单位；
2. 其次进程是独立逻辑地址空间的，不同的进程占有不同的内存，而线程是共享进程的地址空间的，具体是共享全局数据区、代码区、堆区，而线程可以看作是函数调用，所以只有线程自己的栈(保存函数返回值、参数、局部变量等)、程序计数器(记录cpu指令运行的位置，用于保存和恢复线程运行)还有相关的寄存器等是私有的（具体可以看[线程间到底共享了哪些进程资源？](https://mp.weixin.qq.com/s?__biz=MzkyODE5NjU2Mw==&mid=2247484696&idx=1&sn=0b78dd50d9c85d47e9c6caf570676df3&source=41#wechat_redirect)）
3. 多进程程序正因为独立地址空间，所以隔离性和安全性会高很多，同步简单，它们之间的通信是通过[进程间通信IPC](https://mp.weixin.qq.com/s?__biz=MzUxODAzNDg4NQ==&mid=2247485318&idx=1&sn=0da0a684639106f548e9d4454fd49904&chksm=f98e432ccef9ca3ab4e10734fd011c898785f18d842ec3b148c7a8ee500790377858e0dbd8d6&mpshare=1&scene=1&srcid=1222PoSE3btDDbVZTpbVFRGY&sharer_sharetime=1608644397743&sharer_shareid=054214e3287ede8cff93de9018c6d7da&key=452df62e0d28fbf6ce00f71c62937730d5ee6936faf39a6e38f3afc1c1eecf29611d24184dbfae89d8572d05b933bf63ae56e9118ace1d52fa40f573302fbb240c57f01722a5ef36097c8c318bca1ac4ac7c0b40ba3d20cced55ad57d2adcaf0d9482b9bb2d4df1e2972d6f6852e6a40180375a034de99aa89e324a7340ae5a4&ascene=1&uin=MTk5NTg4MjAwOA%3D%3D&devicetype=Windows+10+x64&version=6300002f&lang=zh_CN&exportkey=A4h4l3h%2B65ju24LrHmgYAdE%3D&pass_ticket=qyjAnw1gFqQF7mqoJAeFoX33p%2F2bzYSguVo9fxYdp8pzmhFq3q6eYEyAuf%2F1fLAm&wx_header=0)来完成的，而线程间通信是通过信号量、条件变量和互斥锁完成的
4. 多进程程序很少发生内存泄漏，当进程退出后，其内存就会自动释放，而多线程需要自己用完便释放
5. 多进程程序中，子进程完成所有父进程资源的复制,同时会拷贝所有文件描述符，仅fork时子进程会继承父进程fork之前所设置的信号处理方式，并且是写时复制(copy on write)，也就是如果子进程不修改则和父进程共享读同一份资源，只有需要修改时才复制一份，[参考](https://www.zhihu.com/question/265400460/answer/467910016?utm_source=wechat_session&utm_medium=social&utm_oi=752170274572500992&utm_content=group2_Answer&utm_campaign=shareopn)



# 海量数据

[一般都是通过hash算法将文件里重复的数据映射到同一文件或者将文件的字符串映射到许多小的文件里，足以让内存加载，再进行相关处理](https://blog.csdn.net/v_july_v/article/details/6685962)



# OS

1. [进程和线程基础知识全家桶，30 张图一套带走](https://mp.weixin.qq.com/s?__biz=MzUxODAzNDg4NQ==&mid=2247485175&idx=1&sn=eda03758d4e810afd897ade44c19a508&chksm=f98e425dcef9cb4b3da63e6054f34d5012068b16eb3503d7e5a93bc2a857f1e5116ff793f1d9&mpshare=1&scene=1&srcid=0826ca3tehRsSHa7czjLMSna&sharer_sharetime=1598443376312&sharer_shareid=054214e3287ede8cff93de9018c6d7da&key=f8fcfe1a6c069eefba57e1761b92531ce7b1979516e1422c14dd6527c24f30b33ebddaa4fc5c10baa6479c0031796d5cd8f47dbd6a597deed6380bd9552307b87c8adbea40c134f608f9f4e9b0646e40c9abd308c627b6522591e2a1dd86098e7a64e6ed2c27e2df047975b7636675db16f04953787a48ffe24634c8a46131b5&ascene=1&uin=MTk5NTg4MjAwOA%3D%3D&devicetype=Windows+10+x64&version=62090529&lang=zh_CN&exportkey=A8TiXIls7EsmAft8F9zlM7U%3D&pass_ticket=bj74g1ZD8%2ByyctOKfx3dhTXVBCfLthb7gFHAK5T31BKUtkwjvbiNyoe7EHIRsSPo)
2. [多个线程为了同个资源打起架来了，该如何让他们安分？](https://mp.weixin.qq.com/s?__biz=MzUxODAzNDg4NQ==&mid=2247485264&idx=1&sn=78585cbabd1e0c333b43a3abd2b2ff64&chksm=f98e43facef9caecb8681562a1465fc5e1657b4bd332c6370289454b95737484cdbccd7b8076&mpshare=1&scene=1&srcid=0826dOt6c08lCwrvIXdyJTGM&sharer_sharetime=1598443365799&sharer_shareid=054214e3287ede8cff93de9018c6d7da&key=281d398fa0af70c949f6a2459da8ceae83b6936dfa6851f226dc7fc9a7499eeed44e70fae4497d19d94454c274d2a76de01e618a73133d20db68f0b94d1077958cbce59655c23326559a1c9ff7a4354440840f8dbd614454bb0497300c5798bc67149254220f269d7568dae21e3a0285140d9a8e28617a9f4321a1643259b4a4&ascene=1&uin=MTk5NTg4MjAwOA%3D%3D&devicetype=Windows+10+x64&version=62090529&lang=zh_CN&exportkey=A6gu8N%2FEcAhjRQw0aoDXOm8%3D&pass_ticket=bj74g1ZD8%2ByyctOKfx3dhTXVBCfLthb7gFHAK5T31BKUtkwjvbiNyoe7EHIRsSPo)
3. [凉了！张三同学没答好「进程间通信」，被面试官挂了....](https://mp.weixin.qq.com/s?__biz=MzUxODAzNDg4NQ==&mid=2247485318&idx=1&sn=0da0a684639106f548e9d4454fd49904&chksm=f98e432ccef9ca3ab4e10734fd011c898785f18d842ec3b148c7a8ee500790377858e0dbd8d6&scene=158#rd)
4. [真棒！ 20 张图揭开内存管理的迷雾，瞬间豁然开朗](https://mp.weixin.qq.com/s?__biz=MzUxODAzNDg4NQ==&mid=2247485033&idx=1&sn=bf9ba7aca126ad186922c57a96928593&chksm=f98e42c3cef9cbd514df38d04deb5e7a9ea67dbd478da75fc4a7636ee90b1384d65f68eb23f5&scene=158#rd)
5. [2w字 + 40张图带你参透并发编程！](https://mp.weixin.qq.com/s?__biz=MzU2NDg0OTgyMA==&mid=2247492647&idx=1&sn=4d85bc69e46ba7ab67972348577b8e78&chksm=fc4619d4cb3190c2467da4b2fc91ff94522c7581ddc21068df26f88a838452f42117403e7d2f&mpshare=1&scene=1&srcid=0816lrAm4yicb0UWZbGI3qMx&sharer_sharetime=1597539580972&sharer_shareid=054214e3287ede8cff93de9018c6d7da&key=281d398fa0af70c91f25c9d76af220aec96542b8bad86ae134ceaa9fa7c1b2ca45d0f4de036a28f3aa544c1825eacc582a3495845354a58bdadea12b1ca2a104a3dbcbd14d74c42853587d7d8caa552afd7412cd3b42d67e46de9ecaabdb245b52ffc1bff70b2f4b044fd853d2fffb95d9b3d34c1f84132f004134770898c271&ascene=1&uin=MTk5NTg4MjAwOA%3D%3D&devicetype=Windows+10+x64&version=62090529&lang=zh_CN&exportkey=AxHxNCX%2F6gbBXuZ2AeuMSS8%3D&pass_ticket=bj74g1ZD8%2ByyctOKfx3dhTXVBCfLthb7gFHAK5T31BKUtkwjvbiNyoe7EHIRsSPo)
6. [键盘敲入 A 字母时，操作系统期间发生了什么...](https://mp.weixin.qq.com/s?__biz=MzUxODAzNDg4NQ==&mid=2247485498&idx=1&sn=6948f309461ea83c691892949c8272dd&chksm=f98e4c90cef9c5861ae3747780ea74eeab79eb84d8bc16c8a389d2a29ec606acdf1295cae3b9&scene=158#rd)
7. [一口气搞懂「文件系统」，就靠这 25 张图了](https://mp.weixin.qq.com/s?__biz=MzUxODAzNDg4NQ==&mid=2247485446&idx=1&sn=2c525f008622b98bc08a66f2b4dcfee8&chksm=f98e4caccef9c5bafe0a69378623049a1cf37fbb8b61b65922f772e2170f98292b914a4268e5&scene=158#rd)



# ping

https://zhuanlan.zhihu.com/p/45110873



# 软硬链接

软链接

```
ln -s srcfile dstfile
```



硬链接

```
ln srcfile dstfile
```



使用`ls -il`来查看文件索引节点：最前面是索引节点号，-r 后面的是链接数，后面的数字是文件大小

软链接可以对任何文件系统使用，包括文件夹和文件， 仅仅是在其他地方多一个对srcfile的符号文件，并不拷贝其内容，而且软链接建立后，无论是srcfile移动，还是dstfile移动之后， 软链接的dstfile都会失效。

> 只有软链接才会ls -il时显示 dstfile->srcfile这个符号，同时，dstfile和srcfile的连接数都是1，并不会增加，文件索引节点也都不一样，因为软连接其实是多了个符号文件，并不是同一个文件

硬链接只能链接文件，不能链接文件夹，其dstfile和srcfile具有相同的索引节点号和大小，相当于拷贝了内容，但是是同一个文件，类似于C++中的引用，所以无论是哪个文件移动或删除，都不影响其正常使用

> 硬链接在ls -il时不会显示dstfile->srcfile，同时，dstfile和srcfile的链接数都会增加1，且索引节点号相同的所有硬链接的链接数都是相同的，增加一个硬链接，所有文件索引节点号都加1且相同

------

[更多解释1](https://zhuanlan.zhihu.com/p/67366919) [更多解释2](https://zhuanlan.zhihu.com/p/41443416)



# kill

kill和kill -9       
https://blog.csdn.net/weixin_42139375/article/details/83107127

知道进程名怎么杀死进程？
https://blog.csdn.net/andy572633/article/details/7211546                       
https://blog.csdn.net/zhaoyue007101/article/details/7699259



# 检查内存泄漏

1. windows下通过在代码中加入crtdbg_map_alloc宏及相关函数[检测、定位内存泄漏](https://www.cnblogs.com/skynet/archive/2011/02/20/1959162.html)
2. Linux下通过[valgrind](https://blog.csdn.net/gatieme/article/details/51959654)无需重新编译就可以检测定位;[mtrace](https://blog.csdn.net/u010318270/article/details/73603723)使用方法和CRT运行库类似

## Windows 下的内存泄漏检测方法



在 Windows 下，可使用 Visual C++ 的 [C Runtime Library（CRT） 检测内存泄漏](https://msdn.microsoft.com/zh-cn/library/x98tx3cf.aspx)。

首先，我们在两个 .c 文件首行插入这一段代码：

```
#ifdef _WINDOWS
#define _CRTDBG_MAP_ALLOC
#include <crtdbg.h>
#endif
```



并在 `main()` 开始位置插入：

```
int main() {
#ifdef _WINDOWS
    _CrtSetDbgFlag(_CRTDBG_ALLOC_MEM_DF | _CRTDBG_LEAK_CHECK_DF);
#endif
```



在 Debug 配置下按 F5 生成、开始调试程序，没有任何异样。

然后，我们删去 `lept_set_boolean()` 中的 `lept_free(v)`：

```
void lept_set_boolean(lept_value* v, int b) {
    /* lept_free(v); */
    v->type = b ? LEPT_TRUE : LEPT_FALSE;
}
```



再次按 F5 生成、开始调试程序，在输出会看到内存泄漏信息：

```
Detected memory leaks!
Dumping objects ->
C:\GitHub\json-tutorial\tutorial03_answer\leptjson.c(212) : {79} normal block at 0x013D9868, 2 bytes long.
 Data: <a > 61 00 
Object dump complete.
```



这正是我们在单元测试中，先设置字符串，然后设布尔值时没释放字符串所分配的内存。比较麻烦的是，它没有显示调用堆栈。从输出信息中 `... {79} ...` 我们知道是第 79 次分配的内存做成问题，我们可以加上 `_CrtSetBreakAlloc(79);` 来调试，那么它便会在第 79 次时中断于分配调用的位置，那时候就能从调用堆栈去找出来龙去脉。

## 1B. Linux/OSX 下的内存泄漏检测方法



在 Linux、OS X 下，我们可以使用 [valgrind](https://valgrind.org/) 工具（用 `apt-get install valgrind`、 `brew install valgrind`）。我们完全不用修改代码，只要在命令行执行：

```
$ valgrind --leak-check=full  ./leptjson_test
==22078== Memcheck, a memory error detector
==22078== Copyright (C) 2002-2015, and GNU GPL'd, by Julian Seward et al.
==22078== Using Valgrind-3.11.0 and LibVEX; rerun with -h for copyright info
==22078== Command: ./leptjson_test
==22078== 
--22078-- run: /usr/bin/dsymutil "./leptjson_test"
160/160 (100.00%) passed
==22078== 
==22078== HEAP SUMMARY:
==22078==     in use at exit: 27,728 bytes in 209 blocks
==22078==   total heap usage: 301 allocs, 92 frees, 34,966 bytes allocated
==22078== 
==22078== 2 bytes in 1 blocks are definitely lost in loss record 1 of 79
==22078==    at 0x100012EBB: malloc (in /usr/local/Cellar/valgrind/3.11.0/lib/valgrind/vgpreload_memcheck-amd64-darwin.so)
==22078==    by 0x100008F36: lept_set_string (leptjson.c:208)
==22078==    by 0x100008415: test_access_boolean (test.c:187)
==22078==    by 0x100001849: test_parse (test.c:229)
==22078==    by 0x1000017A3: main (test.c:235)
==22078== 
...
```



它发现了在 `test_access_boolean()` 中，由 `lept_set_string()` 分配的 2 个字节（`"a"`）泄漏了。

Valgrind 还有很多功能，例如可以发现未初始化变量。我们若在应用程序或测试程序中，忘了调用 `lept_init(&v)`，那么 `v.type` 的值没被初始化，其值是不确定的（indeterministic），一些函数如果读取那个值就会出现问题：

```
static void test_access_boolean() {
    lept_value v;
    /* lept_init(&v); */
    lept_set_string(&v, "a", 1);
    ...
}
```



这种错误有时候测试时能正确运行（刚好 `v.type` 被设为 `0`），使我们误以为程序正确，而在发布后一些机器上却可能崩溃。这种误以为正确的假像是很危险的，我们可利用 valgrind 能自动测出来：

```
$ valgrind --leak-check=full  ./leptjson_test
...
==22174== Conditional jump or move depends on uninitialised value(s)
==22174==    at 0x100008B5D: lept_free (leptjson.c:164)
==22174==    by 0x100008F26: lept_set_string (leptjson.c:207)
==22174==    by 0x1000083FE: test_access_boolean (test.c:187)
==22174==    by 0x100001839: test_parse (test.c:229)
==22174==    by 0x100001793: main (test.c:235)
==22174== 
```



它发现 `lept_free()` 中依靠了一个未初始化的值来跳转，就是 `v.type`，而错误是沿自 `test_access_boolean()`。

编写单元测试时，应考虑哪些执行次序会有机会出错，例如内存相关的错误。然后我们可以利用 TDD 的步骤，先令测试失败（以内存工具检测），修正代码，再确认测试是否成功。



# 链接

[静态链接](https://zhuanlan.zhihu.com/p/145263213)

[动态链接](https://www.zhihu.com/question/439231062/answer/1678652070?utm_source=wechat_session&utm_medium=social&utm_oi=752170274572500992&utm_content=group2_Answer&utm_campaign=shareopn)

[.o .obj .a .lib .so .dll .bin .exe](https://blog.csdn.net/lanxueCC/article/details/52799955)



# 算法

## DFS

## HashMap

## KMP

## topk

## 买卖股票

## 区间重叠

