> {*.c, *.h} -预处理-> *.i -编译-> *.s -汇编-> *.o -链接-> exe 
>
> ![image-20250305124209571](D:\data\Typora_imgs\cpp note\image-20250305124209571.png)
>
> 预处理
> gcc -E main.c -o main.i
>
> 编译
> gcc -S test.c -o test.s
>
> 汇编
> as
>
> 一步编译
> gcc  -c test.c -o test.o
>
> 链接
> gcc test.o -o test

```
gcc
	-g gdb
	-O 编译优化
	-D 添加宏
	-Wall 开启所有警告
	-I 指定头文件搜索路径
	-fno-elide-constructors	关闭编译器优化
```



### gdb 调试

```shell
gcc -c test.c -o test.o -g -O0	# -O编译优化(默认时O0，最高是O3) -g调试(调试时不要开优化)
gcc test.o -o test

gcc test.c -o test -g	# 一步到位

# 使用core dump文件调试，处理段错误
gdb test core
```

```shell
help		# 查看命令帮助
l|list			# [文件名:]行号or函数名 列出10行代码
r|run			# 运行
b|break			# [文件名:]行号 打断点
enable|disable	# 不删除断点，使断点生效或失效
d|delete		# 删除断点（使用断点编号）
info b|dispaly|threads	# 展示信息
p|print			# 打印
display			# 自动显示
undisplay		# dispaly编号 关闭显示
n|next			# 逐过程
s|step			# 逐语句（进入程序调用）
c|continue		# 执行到下一个断点
x/FMT address	# FMT:数量格式(o,x,d,u...)大小(b,h,w,g)	查看内存
bt|backtrace	# 查看栈 (段错误时，使用bt查看栈帧)
set args		# 补充命令行参数


set var i=8		# 改变变量的值

# 多进程/线程

# 调试已经运行的进程
gdb program -p=[pid]
attach [pid]	# 附着在进程上
detach 			# 取消
# 跟踪子进程
set follow-fork-mode child
# 同时跟踪父子进程，另一个阻塞
set detach-on-fork off
i inferiors	# 查看正在调试的进程
inferior [infno]	# 切换进程
# 父子进程同时运行
set schedule-multiple on

# 查看线程
i threads
# 切换线程
thread [thread_id]
# 设置一个监视点，当指定变量的值发生变化时停下来
watch <variable>
# 查看所有线程的调用栈信息
thread apply all bt
# 一个线程暂停时，其他线程暂停执行
set scheduler-locking on
# 显示当前作用域内的变量信息
info variables
```

### 内存管理

![image-20250629181549788](D:\data\Typora_imgs\cpp note\image-20250629181549788.png)

* 编译后运行前
  * 代码段：存放代码
  * 数据段：存放非零初始化的全局变量和静态局部变量。
  * BSS：存放0初始化或未初始化的全局变量和静态局部变量
* 运行中增加
  * 栈
  * 堆

==虚表==：

> 无虚继承时：
>
> ![image-20250705132034211](D:\data\Typora_imgs\cpp note\image-20250705132034211.png)
>
> 有虚继承时：
>
> ![image-20250705133908340](D:\data\Typora_imgs\cpp note\image-20250705133908340.png)



### 库文件

#### 静态库（.a）

链接阶段链接进产品，更大体积，更新时需要重新链接

```shell
# 生成静态库文件，并拷贝到系统或用户库中
ar crsv lib*.a *.o
```

#### 动态库(.so)

运行时链接，不用重新链接。有依赖问题

```shell
# 编译过程，加上-fpic生成位置无关代码
gcc -c add.c -o add.o -fpic

# 生成动态库文件,并拷贝到系统或用户库中
gcc -shared add.o -o add.so
```

#### 链接

```shell
gcc main.o -o main -l*
```

### 宏

```c++
// 只包含一次
#pragma once

// 按4个字节对齐
#pragma pack(4)
```



### 命名空间

* 用于解决命名冲突的问题

* ```cpp
  namespace name{}
  ```

* ```cpp
  using namespace name;
  using name::func;
  using name::var;
  ```

* 匿名命名空间只能在本文件使用

### new 语句

* 不需要类型转换
* 包含初始化
* 传入初始化值，而非字节数。
* 数组：`int* arr = new int[3]();`
* 初始化：`int *i = new int(2);`或`int* arr = new int[3]{1,2,3};`

### const 常量

* 与C的差异
  * 必须初始化
  * 默认static，无法跨文件调用
  * 可以和宏定义常量一样初始化数组
  * 普通指针指向常量指针所指内容，必须const_cast
  * 可能编译器直接替换，不分配内存
* const成员函数中，不允许对数据成员进行修改。实际上把this指针变成常指针常量。【可以和非const成员函数重载，this指针类型不同】
* const对象：无法调用非const成员函数

####常量指针（常指针）

const int* a = &var; ==>常量的指针，内容不可变
  |    |
 常量  指针

#### 指针常量

int* const a = &var; ==> 指针是常量，指向不可变
 |     |
指针  常量

### 引用

* 相当于指针常量
* 引用必须初始化
* 引用不能修改绑定
* 对引用取地址得到的是原变量的地址
* 作为函数返回值时可以实现持久化
* const引用可以绑定到右值（即常量）

### 类型转换

```cpp
// 最常用的，指针的类型转换
// 编译时转换。不能转换运行时信息
static_cast<int*>(p);

// 去除常量属性，无法改变原常量的值【引用可以】
// 用于移除const和volatile（和const对应，变量，防止编译优化）属性
const_cast<int*>(p);

// 用于基类和派生类之间的转换【基类转向派生类。合理则成功，不合理则空指针】
// 主要处理多态性，需要有多态的内容。
// 会检查转换安全性，不安全返回nullptr
dynamic_cast<child*>(p);

// 用于任意类型之间的转换
reinterpret_cast<int*>(p)
```

### 函数重载

* 通过在编译期间，更改函数名实现
* func(int x)改成funci

### struct和class

* 基本相同， struct默认是public，class默认是private
* 可以初始化
* 可以定义函数

### 构造函数

* 默认自带：无参构造、析构、拷贝构造
* 拷贝构造：`cls(const cls & cpy_cls){}`。应该避免(1. 做参数时调用 2. 做返回值时调用)【用引用来解决】
* 初始化列表，可以初始化特殊变量【常量、引用数据成员、类对象成员（使用构造函数）】
* 静态成员（类外初始化，没有this指针）
* `explicit`加在构造函数前可以禁止隐式转换
* 委托构造函数：在初始化列表调用别的构造函数

### 左值和右值

* 能够取地址的是左值，不能则是右值
* 临时变量、匿名变量、临时对象、匿名对象为右值

### 重写拷贝和赋值

1. 解决自拷贝
2. 释放原始对空间
3. 深拷贝
4. 返回*this

* 拷贝构造函数、赋值函数和

```cpp
cls & operator=(const cls & cls){
    if(this != &cls){
        delete [] _data;
        _data = new int[3]();
        ...;
    }
    return *this;
}
```

### string字符串

* 对于长字符串，使用写时复制(CWO)机制优化
* 对于短字符串，使用短字符串优化(SSO)

构造函数

* `string()`：默认。空字符串
* `string(const char * rhs)`：通过c风格字符串创建string对象
* `string(const char *rhs, size_type count)`：通过前count个字符构造对象
* `string(const string & rhs)`：拷贝构造
* `string(size_type count, char ch)`：包含count个ch字符的string对象
* `string(InputIt first, InputIt last)`：使用迭代器区间内的字符创建

成员函数

* `size()` and `length()`：返回字符数
* `empty()`：是否为空
* `data()` and `c_str()`：返回c风格字符串
* `push_back()`：把字符追加到末尾
* `append()`：在字符串末尾添加到末尾
* `find()`：查找子串
* `begin()` and `end()`：首迭代器和尾迭代器（尾元素后一位）【s`string::iterator`】

### 流

IO状态

* badbit：系统级错误。`bad()`
* failbit：可恢复错误。`fail()`
* eofbit：流结尾。`eof()`
* goodbit：有效状态。`good()`
* `clear()`：恢复状态
* `ignore(cnt, '\n')`：清理缓冲区

缓冲区

* `endl`：换行并刷新
* `ends`：加上空字符并刷新
* `flush`：直接刷新缓冲区
* cerr不需要刷新

文件流

```cpp
#include <fstream>
ifstream ifs;
ifs.open("", std::fstream::in);
while(ifs.getline(line, line_size)){}
while(ifs>>word){}
while(std::getline(ifs, line)){}
```

字符串流

```cpp
#include <sstream>
istringstream iss;
string ip;
uint16_t port;
iss>> ip>> port;

ostringstream oss;
oss<< "ip="<< ip<< endl<< "port="<< port<< endl;
oss.str();
```

### 友元

* 让类外的函数或类访问本类的私有成员
* 在类内使用`friend`声明
* 单向、不能继承、无传递

### 占位参数 ==>

func(int a, int) func(int a, int=0)//亚元

### 函数指针 ==>

1. `typedef int(*MY_FUNC_P)(int, int)` -> `MY_FUNC_P fp2 = NULL;`
2. `typedef void (FFF::*memberFunc)()`->`memberFunc p = &FFF::func`

* `.*`：成员指针访问运算符
* `(cls_obj.*func)();` `(p_cls_obj->*func)();`

空指针调用成员函数时，如果成员函数不访问成员变量，则正常执行。

### 深拷贝和浅拷贝 ==>

### static变量和函数 ==>

不占类大小，属于静态区

### 返回对象本身 ==>

使用 return *this;

### 操作符重载 

* 不修改操作数的值，采用友元形式重载
* 修改操作数的值，采用成员函数形式重载

operator
不改变参数个数、不改变优先级、不改变结合性、不能有默认参数

#### ++重载:

* 前++ ： `cls & operator++()`
* 后++ ： `cls operator++(int)`

#### []重载

* `operator[](size_t idx)`

#### *重载

* `cls & operator*()`

#### ->重载

* `cls* operatpr->()`

#### int()重载

* `operator int()`
* 类型隐式转换重载
* 没有返回类型

#### <<,>>重载:

只能写在全局，不能写在成员方法中。否则调用的顺序会变 

### 仿（伪）函数 ==>

将一个类当作函数来调用。重载小括号【函数对象】

### 释放堆数组 ==>

delete[] array

### 继承

public -> 类内部 和 外部都能访问
protected , private -> 类内部可以访问，类外部不可以访问

私有也会继承，只是访问不到。
使用基类域访问基类和派生类同名的变量【c.p::var】

* 构造和析构不能继承
* 复制控制（拷贝构造、赋值运算符）不能继承
* 空间分配方式（new、delete）不能继承
* 友元不能继承【破坏了封装性】

内存空间

* 会创造一个基类子对象（隐式调用基类无参构造函数）【显示调用使用基类名】
* public继承时，派生类中的基类子对象也是public。【其他继承类似】
* 如果派生类定义了变量和函数的名字和基类相同，则会隐藏基类的变量和函数

### 继承方式 ==> 

公有继承： >public -> public
保护继承：>peotected -> peotected 
私有继承：>private -> private

### 多继承和虚继承

c++可以多继承。
当出现菱形继承时，派生类会包含多分基类子对象，中间层需要使用虚继承(virtual)
class a：virtual public b{}

* 虚拟继承后，派生类的内存结构发生变化

* ```cpp
  +--------+
  | +---- base1
  | vfptr
  | vbptr
  | var
  | +---- base2
  | vbptr
  | var
  | +--- 
  | var	
  | +---- virtual base
  | var
  +--------+
  
  +----- vbtable
  | 0		// 到本类的首地址偏移
  | 16	// 到虚基类的首地址偏移
  +-----
  ```

* 虚拟继承后，最底层构造函数要显示调用虚基类的构造

* 虚函数在类外定义时，不需要virtual关键字

### 虚函数

父类成员函数变成虚函数，子类重写以实现多态。（只有加virtual才会实现多态）
有virtual关键字时，会给类增加一个vptr指针（会根据是子类还是父类指向不同的虚函数表，以实现多态）

* 构造不能设为虚函数：要先有对象，才能有多态
* 静态成员函数不能设为虚函数：静态成员函数没有this指针。虚表存在于对象中
* inline函数不能设为虚函数：inline在编译期间替换，虚函数实在运行中实现多态
* 普通函数不能设为虚函数：普通函数不在类中，和继承多态无关

_派生类新的虚函数会放到第一个虚表中（如果）。有虚表的基类在内存布局中靠前_

### 纯虚函数

只有声明，没有实现。
`vitrual void func() = 0;`

### 动态联编、静态联编 ==>

联编是指一个计算机程序的不同部分彼此关联的过程

- 静态联编是指联编工作在编译阶段完成的，这种联编过程是在程序运行之前完成的，又称为早期联编。要实现静态联编，在编译阶段就必须确定程序中的操作调用（如函数调用）与执行该操作代码间的关系，确定这种关系称为束定，在编译时的束定称为静态束定。

- 动态联编是指联编在程序运行时动态地进行，根据当时的情况来确定调用哪个同名函数，实际上是在运行时虚函数的实现。这种联编又称为晚期联编，或动态束定。动态联编对成员函数的选择是基于对象的类型，针对不同的对象类型将做出不同的编译结果。动态联编规定，只能通过指向基类的指针或基类对象的引用来调用虚函数。
  编译时绑定函数地址为早绑定，运行时绑定函数地址为迟绑定

### 多态

1. 静态多态【静态联编、编译时多态】：函数重载、运算符重载、模板
2. 动态多态【动态联编、运行时多态】：通过虚函数实现

**动态动态激活条件**

1. 基类定义虚函数
2. 派生类中要重写虚函数
3. 创建派生类对象
4. 基类的指针指向派生类对象（或基类引用绑定派生类对象），即向上转型
5. 通过基类指针（引用）调用虚函数

虚函数表在编译完成时就已经存在且不可修改。

基类指针指向派生类基类子对象的首地址，而不是本身的首地址

### 虚析构函数 ==>

使用了多态时，应当使用虚析构函数，以避免析构不完全

### 纯虚析构函数 ==>

可类内声明，类外实现。该类为抽象类，无法实例化。

### 重载、重写、重定义 ==> 

重载（overload）：同一作用域下
重写（override）：父子关系中，虚函数重重定义
重定义【隐藏】（oversee）：父子关系中，普通函数重定义

### vfptr指针 ==> 

分布初始化，在调用父类构造的时候会临时指向父类虚函数表。

### 抽象类
只要有纯虚函数就是抽象类，抽象类不能直接实例化。子类不重写纯虚函数，依然是抽象类。

只定义了protected访问权限构造函数的类，也算一种抽象类

### 函数模板 ==>

`template<class T>` 等价于 `template<typename T>`
1.自动类型推导 2.显示指定类型 `function<int>()`
		

		1.如果出现重载，优先调用普通函数（普通函数可以进行类型的隐式转换，模板不可以）
		2.如果想强制调用模板函数，使用空参数列表
		3.函数模板可以发生重载
		4.如果函数模板可以产生更好的匹配，那么优先调用函数模板
* 分文件编写模板时
  1. 在`.h`文件中包含`.cc`文件
  2. 为了区分模板和普通的区别，头文件不再有后缀名，`.cc`变成`.tcc`
* 模板的非类型参数没有默认值时，必须显示实例化（`float`和`double`不能作为非类型参数）

### 模板机制 ==>

1. 不能通用所有类型
2. 需要生成模板函数才可以调用
3. 编译器会对模板函数进行两次编译，第一次对模板进行编译，第二次对替换类型后的函数进行编译。

### 具体化模板函数（特化模板）

* 对原模板函数具体化，优先匹配集体化模板函数
* 必须先有通用模板

```cpp
template<>
void func<char*>(char* p1){}
```

### 类模板

* 类模板不支持自动类型推导。

* 类模板可以有默认类型 --> `template<class T = int>`
* 类成员函数在运行时创建 
* 模板类继承必须显示指定类型 `class child: public base<int>{}`
* **继承模板类时，调用基类成员时需要先using。因为模板类的两次编译，第一次编译会忽略模板参数信息（父类被忽略），导致找不到基类成员**

### 可变模板参数

* `template<class ...Args>
  void func(Args ...args){}`

* `sizeof(args)`得到参数个数

* `args...`解包

* 必须要有出口

* ```cpp
  // 普通函数作为出口
  int sum(int x){
      return x;
  }
  
  template<class T, class... Args>
  T sum(T t1, Args ...args){
      return t1 + sum(args...);
  }
  ```

* 

### 模板类作参数 ==>

1. 指定传入类型 
2. 参数模板化 
   `template<class T>
   int func(className<T> c){}`
3. 整体模板化
   `template<class T>
   int func(T& c){}`
4. 查看类型 --> `typeid(T).name()`;

### 类外实现成员函数 ==> 

`template<class T>
int 类名<T>::func(){}`

### 模板函数中友元函数类外实现 ==>

1. 使用空参数列表，强制调用模板函数
2. 类外提前声明友元函数和类

### 静态类型转换 ==> 

static_cast<目标类型>(原始数据) -->基础数据类型、父子类型

### 动态类型转换 ==>

dynamic_cast<目标类型>(原始数据) -->失去精度、不安全都不可用、发生多态时可基类转化派生类

### 移动语义

**右值**：只有一行的生命周期

左值引用：`&`。
右值引用：`&&`。

可以绑定右值：`&&`，`const int &`

`std::move`可以把左值强转成右值

`std::ref`转成左值引用

* 函数返回值，如果返回值的生命周期返回后结束，会调用移动构造，否则调用拷贝构造

（深）拷贝构造每次都要创建新的空间
移动（浅）拷贝构造：

```cpp
class cls{
public:
    ...
    cls(cls && other)
    :_str(other._str)
    {
        other._str = nullptr;
    }
    
    cls & operator=(cls && other){
        if(this != &other){	// 放置通过std::move实现自复制
            delete [] _str;
            _str = other._str;
        }
    }
private:
    char* _str;
};
```

### 资源管理

#### RAII技术

* 在构造中初始化资源或托管资源

* 析构中释放资源

* 一般不允许复制或赋值

* 提供若干访问资源的方法

* ```c++
  template<class T>
  class RAII{
  public:
      RAII(T* t):_ptr(t){
          cout<< "RAII()"<< endl;
      }
      ~RAII(){
          if(nullptr != nullptr){
              delete _ptr;
          }
          cout<< "~RAII()"<< endl;
      }
      RAII(const RAII & other) = delete;
      RAII & operator=(RAII & other) = delete;
      T* operator->(){
          return _ptr;
      }
      T & operator*(){
          return *_ptr;
      }
      T* get() const{
          return _ptr;
      }
      void set(T* new_ptr){
          if(_ptr){
              delete _ptr;
              _ptr = nullptr;
          }
          _ptr = new_ptr;
      }
  private:
      T* _ptr = nullptr;
  }
  ```

#### 智能指针

* auto_ptr：拥有拷贝构造和赋值，c++17被移除
* unique_ptr：独享控制权
* shared_ptr：共享所有权，引入引用计数【问题：循环引用】
* weak_ptr：弱引用。不增加引用计数。只能依赖于shared_ptr

```c++
vector<unique_ptr<int>> vec;
// 由于unique_ptr没有拷贝构造，所以使用push_back时要用右值
vec.push_back(unique_ptr<int>(new int()));	
vec.push_back(std::move(uni_ptr));

///////
shared_ptr<int> sp(new int(10));

//////
weak_ptr<int> wp;
shared_ptr<int> sp = wp.lock()	// 把weak_ptr提升成shared_ptr
```

使用：`make_shared`和`make_unique`

### 异常处理 ==>

try{}catch(int){}catch(...){}

### 自定义异常类 ==> 

throw myexception();

```cpp
class myexception : public exception{
	~myexception(){}	//重写虚析构
	virtual const char* what(){}	//重写what虚函数	
};
```

### 栈解旋 ==> 

从try开始到throw抛出异常前， 所有栈上的对象都会被释放

### 异常接口声明 ==>

void func() thow(int){} -->只抛出int异常
void func() thow(){} --> 不抛出任何异常

### 异常的多态 ==>

class childException : public baseException{}

### 标准异常类 ==> 

```cpp
#include<stdexcept>
throw out_of_range("越界");
e.what();//check the exception
```

### I/O ==> 

标准io、文件io、串io
标准io: 

> > cin.get() 不读取换行符
> > cin.getline()把读取换行符并丢弃
> > cin.ignore(n)忽略n个字符
> > cin.peek()不取只看
> > cin.putback()取后放回
> > cin.fail()查看标志位
> > cin.clear()重置标志位
> > cin.sync()清空缓冲区  
>
> > cout.flush()刷新缓冲区【linux】
> > cout.put('a').put('b')
> > cout.write(buf, strlen(buf))
> > cout.width(20)输出宽度
> > cout.fill('*')用*填充
> > cout.setf(ios::left)设置格式
> > cout.unsetf()取消格式  

> C:
>
> FILE* fp = fopen("path", "mode");
>
> int c  = fgetc(fp);
>
> char *fgets(char *str, int max_char_num, FILE *stream)
>
> int fputs(const char *str, FILE *stream)
>
> fputc(c, fp);
>
> size_t fread(void *buf, size_t ele_size, size_t ele_num, FILE *stream)
>
> size_t fwrite(const void *buf, size_t ele_size, size_t ele_num, FILE *stream)
>
> fclose(fp);

文件IO ==> #include<fstream> || <ifstream> || <ofstream>

```cpp
	ofstream ofs("./test.txt", ios::out | ios::trunc);
	或
	ofstream ofs;
	ofs.open("./test.txt", ios::out | ios::trunc);
	//判断是否打开成功
	if(!ofs.is_open()){cout<<"打开失败"<<endl;}
	ofs<<"hahha"<<endl;
	ofs.close();

	ifstream ifs("./test.txt", ios::in);
	//第一种读入方式
	while(ifs>>buf){cout<<buf<<endl;}
	//第二种读入方式
	while(!ifs.eof()){ifs.getline(buf,sizeof(buf)); cout<<buf<<endl;}
	//第三种读入方式
	while((ch = ifs.get() !)= EOF){cout<<ch<<endl;}
```

(2)文件io ==>

 ````cpp
 ifstream ifs(filePath, ios::in); //只读
 ofstream ofs(filePath, ios::out | ios::ate); // ios::ate追加
 if(!ism){ 
     cout<<"打开文件失败！"<<endl; 
 }
 //读文件	
 while(ifs.get(ch)){ 
     cout<<ch; ofs.put(ch);
 }
 ofstream ofs(targetName, ios::out | ios::binary); //ios::binary以二进制模式打开
 //写文件
 ofs.write((char*)&class_p1, sizeof(class_1));
 ````

##  STL标准库

####六大组件

1. 容器：用来存放数据
2. 迭代器：访问容器的工具。看作一种广义的指针
3. 算法：操作容器中的元素
4. 适配器：
   1. 容器适配器：stack、queue
   2. 迭代器适配器
   3. 函数适配器
5. 函数对象（仿函数）：定制化操作。delete
6. 空间配置器：进行空间的申请和释放操作

#### 容器

1. 顺序容器：能按顺序访问的容器
   1. `array`：静态数组
   2. `vector`：动态数组
   3. `deque`：双端队列。双端开口的连续线性空间，没有容量概念，动态地以分段连续空间组合而成，可随时增加。迭代器不是普通指针，在排序时最好先复制到vector再排序。可以随机访问。
   4. `forward_list`：单链表
   5. `list`：双链表
2. 关联容器：实现快速查找。有有序和无序两种
   1. `set`：唯一键集合
   2. `map`：映射字典【键值对集合】
   3. `multiset`：可重复键集合
   4. `multimap`：可重复键映射

#### vector

```cpp
class vector{
    ptr* start;
    ptr* finish;
    ptr* end_of_storage;
}

// push_back超出容量时，容量*2
// insert超出容量时，如果插入数量小于size，容量为size*2；如果大于，容量为插入数量加size
// 扩容时，以前生成的迭代器失效。
// 插入和删除后也可能使旧迭代器失效
```

#### set

```markdown
使用.sort()时排序自定义类型时，需要比较自定义类型大小。有三种方式：
1. 类重载<符号
2. 使用[友元类]函数对象
3. 使用特化模板重载std::less<>

元素是有序的。

底层是红黑树
```

#### 迭代器

类型：

1. 随机访问迭代器
2. 双向迭代器
3. 前向迭代器（输入和输出）
4. 输入迭代器：读。（不能多趟保证，输入流迭代器）
5. 输出迭代器：写
6. 流迭代器：把输入/输出流作为**容器**看待（缓冲区的概念）

![image-20250616195045498](D:\data\Typora_imgs\cpp note\image-20250616195045498.png)



迭代器适配器：

* back_inserter()返回一个back_insert_iterator，`operator=`会调用容器的push_back
  同理front_inserter()和inserter()。`operator=`调用push_front()和insert()

* 反向迭代器：

  * ++变--、--变++

  * 统一。。。。，通过不同的迭代器，不同的访问方式

#### 算法

分类：

1. 非修改时算法：for_each、count、find、search
2. 修改式算法：copy、remove_if、replace、swap
3. 排序算法：sort
4. 二分搜索：lower_bound、upper_bound
5. 集合操作：set_intersection、set_union
6. 堆操作：make_heap、push_heap、pop_heap
7. 取最值：max、min
8. 数值操作：accumulate、atoi
9. 未初始化的内存操作：uninitialized_copy

一元函数：函数参数是一个

一元谓词（unary predicate）：函数参数是一个，返回类型是bool。

二元谓词：函数参数是两个，返回类型是bool。



lambda表达式：

*  []局部变量捕获列表，默认const。值传递想修改在()后加mutable

* ()函数的参数列表

* {}函数的函数体

* ->返回类型

* 默认看作operator()const的函数对象，捕获变量相当于成员变量

* ```cpp
  int a = 100;
  
  // 值捕获
  [a]()mutable->void{}();
  
  // 引用捕获
  [&a]()->void{}();
  
  // a使用引用传递，其他变量使用值传递
  [=, &a](){};
  
  // a使用值传递，其他变量使用引用传递
  [&, a](){};
  
  // 要访问类
  [this](){}
  
  // 函数指针接收
  pFunc p = [](){};
  ```

**【引用折叠：T&& 如果T类型是左值引用则折叠为左值引用，为右值引用为右值应用。T&如果T类型是左值引用为左值引用，为右值引用还是左值引用】**

* remove_if()：要结合erase，把要删除的元素移到末尾。

* bind1st()：把函数的第一个参数绑定到一个值上 bind2nd()：把函数的第二个参数绑定到一个值上**【已经移除】**

* bind()：为多元函数绑定参数值。默认采用值传递

  * ```cpp
    // 绑定二元函数。func(int, int)
    auto f = bind(func, 1, 2);
    
    // 绑定成员函数。func(int, int)，隐藏this指针
    cls c;
    auto f = bind(cls::func, &c, 1, 2);
    
    // 占位符
    int a = 2;
    using namespace std::placeholders;
    bind(func, 10, _1, _3, a);
    
    // 用引用包装器实现引用传递。ref()、cref()
    auto f = bind(func, 1, std::ref(a), std::cref(b));
    
    // 用function显示指定函数类型，func(int)绑定后得到func()。
    function<int()> f = bind(func, 1);
    ```

  基于对象的方法

  ```cpp
  struct base{
      using DispalyCallback = function<void()>;
      
      base() = default;
      ~base() = default;
      void set_DispalyCallback(DispalyCallback && cb){
          _dispalyCallback = move(cb)
      }
      void dispalyhandler(){
          if(_dispalyCallback){
              _dispalyCallback();
          }
      }
      
      dispalyCallback _dispalyCallback;
  };
  
  struct child1{
      child1(int x):val(x){}
      void display(string && prefix){
          cout << prefix << val << 1 << endl;
      }
      int val;
  };
  
  struct child2{
      child2(int x):val(x){}
      void display(string && prefix){
          cout << prefix << val << 2 << endl;
      }
      int val;
  };
  
  int main(){
      base b;
      child1 c1(1);
      child2 c2(2)
      b.set_DispalyCallback(bind(&child::display, c1, "child1"));
      b.displayHandler();
      
  }
  ```

  

**成员函数适配器**

* `mem_fn`



**空间配置器**

> 头文件： <memory>

```cpp
//成员函数
    
// 申请原始的未初始化的空间
T* allocate(std::size_t n);

// 释放空间
void delallocate(T* p, std::size_t n);

// 在指定空间构建对象
void construct(pointer p, const_reference val);

// 销毁对象
void destroy(pointer p);
```

**特点**

* 空间的申请和对象的构建严格分开
  * 在STL中，元素个数一般是批量创建以及可扩容，为避免频繁创建销毁空间

```cpp
void reAllocate(){
    // 1. 申请一块新空间
    int oldCapacity = capacity();
    int newCapacity = oldCapacity<0? oldCapacity*2 : 1 ;
    T* pTmp = alloc.allocate(newCapacity);
    if(_start){
        // 2. 把原空间内容拷贝到新空间
        uninitialized_copy(_start, _finish, pTmp);
        // 3. 释放原空间内容
        while(_start != _finish){
            alloc.destroy(--_finish);
        }
        // 4. 回收原空间内存
        alloc.deallocate(_start, oldCapacity);
    }
    // 5. 改变指向，指向信空间内存
    _start = pTmp;
    _finish = pTmp + oldCapacity;
    _end_of_storage = pTmp + newCapacity;
}
```

## CPP线程

#### 库

* thread
* atomic
* mutex
* condition_variable
* future

### thread

```cpp
// 无参构造，可以用来实现线程池，创建但不执行
thread();

// 有参构造，函数、函数对象等 + 参数
thread(func, args...);

// 命名空间this_thread，哪个执行流执行this_thread中的某个函数，都是当前执行流的线程在执行的。

// 返回线程id
this_thread::get_id();
// 让出时间片
this_thread::yield();
// 睡到绝对时间点
this_thread::sleep_until(const chrono::time_point<Clock,Duration>& abs_time);
// 睡相对时间
this_thread::sleep_for(const chrono::duration<Rep,Period>& rel_time);

this_thread::sleep_for(chrono::seconds(1));
```

### mutex

```cpp
// 创建mutex对象，只有无参构造，拷贝也没有
mutex m;

// 成员函数，上锁
m.lock();

// 成员函数，解锁
m.unlock();

// 成员函数，尝试上锁，失败不会阻塞
m.try_lock();

```

其他锁：

```cpp
// 时间锁
timed_mutex m;

// 增加了两个成员函数
m.try_lock_for();
m.try_lock_until();


// 递归锁，递归函数调用递归锁时，自己上的锁不会阻塞
recursive_mutex m;

// 智能锁。利用RAII，实现构造中上锁，析构中解锁。
lock_guard m(mutex);
// 智能锁。有成员函数，可提前解锁
unique_lock<mutex> m;

// lock_guard的使用，使用代码块
{
    lock_guard m(mutex);
    x++;
}
```

### atomic

```cpp
// 给变量加上原子性
atomic<int> x(0);

// 原子变量的内存序
// 放松的内存顺序。不对执行顺序做任何保证，除了原子操作本身的原子性
// memory_order_relaxed 

// 较轻量级的保序需求，用于指定操作依赖于先前的某些操作结果。
// memory_order_consume

// 获取操作，禁止后续的读或写被重排到当前操作之前。用于读取操作
// memory_order_acquire

// 释放操作，防止之前的读或写操作被重排到当前操作之后。用于写入操作。
// memory_order_release

// 同时包含获取和释放语义。适用于同时具有读取和写入特性的操作。
// memory_order_acq_rel

// 顺序一致性内存顺序。所有线程看到的操作顺序一致。这是默认的内存顺序，并且提供了最强的顺序保证。
// memory_order_seq_cst
```

### condition_variable

```cpp
// 条件变量
condition_variable
```

![image-20250623213349776](D:\data\Typora_imgs\cpp note\image-20250623213349776.png)

## 11

### 完美转发

```cpp
// 使用std::forward<T>()实现完美转发
// 当使用模板时，引用作为参数时，不论传进来的是右值还是左值，引用本身是左值。
// 比如在使用拷贝构造时，就会调用左值的拷贝。
// 使用完美转发后，就会根据所引用的值是左值还是右值，会调用移动构造。
```

