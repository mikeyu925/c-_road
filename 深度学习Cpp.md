## C++知识点总结



#### namespace

---

命名空间`namespace`作用及注意点

C语言通过`static`关键字来控制`标识符`只得在当前源文件可见，C++还可以通过`namespace`命名空间实现。

> 命名空间通常解决在多人合作过程种出现的 标识符重复问题。
>
> 注意：
>
> 1. 命名空间只能写在全局中
>
> 2. 命名空间可以进行嵌套
>
> 3. 命名空间是开放的，可随时加入，但加入后的成员作用域在其加入后
>
>    ```cpp
>    namespace A{
>    	int a;
>    }
>    //在命名空间A中加入int c；
>    namespace A{
>        int c;//在此之后，命令空间A中的c才能被使用
>    }
>    ```
>
>    
>
> 4. 通过 作用域 运算符`::`来访问不同命名空间`A::cd`的成员
>
>    > `::`作用域运算符也可以解决、局部变量与全局变量的重名问题
>
>    ```cpp
>    //全局变量
>    int a = 10;
>    void test(){
>    	int a = 20;//局部变量
>    	cout << "局部变量a:" << a << endl;//打印局部变量a
>    	cout << "全局变量a:" << ::a << endl;//打印全局变量a
>    }
>    ```
>
>    
>
> 5. 匿名命名空间作用
>
>    ```cpp
>    namespace {
>        int a; //相当于static int a;
>    }
>    //限制其只能在本文件被使用
>    ```
>
> 6. 可以取别名
>
>    ```cpp
>    namespace newname = oldname;
>    cout<<newname::a<<endl;
>    ```
>
> 7. 一个.h文件下有两个命名空间，且具有相同的成员时，在.c文件使用或定义时，需要加上作用域符`::`
>
>    > 不然会出现==链接问题==如：无法解析的外部命令
>



#### 自定义类型

---

using 声明 与 编译指令

using声明：让命名空间中某个标识符直接使用，但同时也可能造成二义性

```cpp
namespace A{
    int a;
    int b;
}
void test(){
    //using是可以让命名空间中某个标识符直接使用
    using A::a;
    cout<<a<<endl;//using声明后可以直接使用
    int a = 10; //错误语句！！！同名冲突
}
```

```
注意：如果命名空间包含一组用相同名字重载的函数，using声明就声明了这个重载函数的所有集合。
```

using 编译指令：让命名空间中的所有标识符都可以直接使用

```cpp
namespace A{
    int a = 1;
    int b;
}
namespace B{
    int a = 2;
    int b;
}
void test(){
    //using 编译指令
    using namespace A;
    cout<<a<<endl;//可以直接使用,打印1
    int a = 10; //此时不会冲突！
    //相当于命名空间中的a为全局变量，此处的为局部变量，覆盖了全局变量a,不会冲突
    cout<<a<<endl;//打印10
}
void test2(){
	using namespace A;
    using namespace B;
	cout<<a<<endl;//错误！！此时又会冲突了，不知道调用A还是B的成员a
}
```

using也可以用来声明新的类型：

```cpp
using int64_ = long;
```



类型转换：

```cpp
// c++
int a = static_cast<int> (b);

// c
int a = int(b);
```





#### 结构体

---

`struct` 类型的加强

> 1. C中定义结构体变量需要加上`struct`关键字，C++不需要。
> 2. C中的结构体只能定义成员变量，不能定义成员函数。C++即可以定义成员变量，也可以定义成员函数。 

```cpp
struct a{
    char c;
    int dd;
};//此时没有分配空间
a my_a{'a',10};//此时分配空间了
```

---

C语言中`const`修饰的全局变量不能直接修改，也不能间接修改，但修饰的局部变量不能直接修改，可以间接修改。修饰的全局变量具有外部链接属性，即可以`extern const int a`使用。

C++中的编译器可能会在编译阶段优化`const`的变量，从而提高效率。修饰的全局变量具有内部链接属性，只能在本文件内使用。

```cpp
const int a = 10;//在c++中没有内存

const int b = 20;
int * p = (int*)&bb;//进行了取址，所以有空间
*p = 200;
cout<<"b:"<<b<<endl;//打印20 因为编译器进行了优化，等价于 cout<<"b:"<<20<<endl，即发生了常量折叠 
//如果不想被优化，需要加上volatile修饰，或者用变量给const修饰的局部变量赋值
cout<<"*p:"<<*p<<endl;//打印200
//两者地址是一样的
cout<<"b地址:"<<&b<<endl;
cout<<"p指向的地址:"<<p<<endl;
```

==尽量用`const`代替`#define`==，不是一定要，是尽量

> 1. #define没有数据类型，`const`修饰的变量有数据类型，会进行类型检查
> 2. `const`有作用域，而#define不重视作用域，默认定义处到文件结尾，不能限定常量的作用范围。宏常量可以有命名空间吗？



#### 字符串

---

字符串创建：

```cpp
string s{"11412431"};
cout<<s<<endl;
string ss{"124112",2}; // 从0开始截取2位
cout<<ss<<endl;
string sss{"1241512",2,3}; // 从下标2开始截取3位
cout<<sss<<endl;


ss = ss + "asd";
// ss = "ss" + "124"; // error!!!不可以
```

字符串截取：

```cpp
// 字符串截取
std::string str{"123456789"};
cout<<str.substr(3)<<endl; //456789
cout<<str.substr(2,3)<<endl; //345
```

字符串比较：

```cpp
// 字符串比较
string A{"abcdefg"};
string B{"abcdffg"};
if(B > A){
  cout<<B<<" > "<<A<<endl;
}
if(B.compare(("dd" + A)) < 0){
  cout<<B<<" < "<<("dd" + A)<<endl;
}

// compare 扩展用法
cout<<A.compare(2,3,B)<<endl; // 将cde和B比较
cout<<A.compare(4,2,B,2,3)<<endl; // 将ef和cdf比较
```

字符串插入：

```cpp
str.insert(要插入的位置，要插入的字符串，要插入字符串的起始位置，要插入的大小);
```

替换字符串：

```cpp
str.replace(要替换内容的起始位置，要替换的长度，替换后的内容)
str.replace(要替换内容的起始位置，要替换的长度，替换后的字符长度，替换的字符)
```







#### 指针

---

指针的类型 限制了指针能够操作的范围

```cpp
unsigned ua{100000};
int a{1000};

long long * lptr = (long long *) &ua;
cout<<*lptr<<endl;
/*
    因为 lptr 是long long 类型，可以操作8个字节的内存
    ua: xx xx xx xx
        xx xx xx xx
    相当于long long 把ua后面的8个字节都作为了其访问的内存值
*/
unsigned * uptr = &ua;
cout<<*uptr<<endl;

int * ptr = (int*)&ua;
*ptr = -1;
cout<<*ptr<<endl;   // -1

cout<<*uptr<<endl;  // 4294967295 uptr要按照其类型进行取值
```

```cpp
int a [] {1,20,30,40,50};
int * p = &a[0]; // 等价于 p = &a  数组的地址就是数组第一个元素的地址
cout<<(*p)++<<endl; // 打印1，然后修改a[0]为2

int a [] {1,20,30,40,50};
int * p = &a[0];
cout<<*p++<<endl; // 打印1，然后指针后移
cout<<(*p)<<endl; // 打印20
```



数组的底层实现是利用了指针，从原理上讲，指针和数组是同一个方法的不同表达。数组名本身就是一个指针，数组元素只是这个指针按照一定的偏移量后对应的内存区域中的内容。

```cpp
int a [] {1,20,30,40,50};
int * p = &a[0];
cout<<a[2]<<" "<< p[2]<<endl; // 指针p也能够直接访问

cout<<sizeof(a)<<endl; // 20
cout<<sizeof(p)<<endl; // 8
```

```cpp
int a [] {1,20,30,40,50};
int * p = {a+2}; // 此时p指向的是30

cout<<*p<<endl; // 30
cout<<p[2]<<endl; // 50
```



指针数组：

```cpp
int* p[5]; // 指针数组 是一个数组，数组中每个元素都是一个指针
int (*p)[5]; //数组指针 是一个指针，指向的是一个长度为5的数组类型
```

数组指针：

```cpp
int a [2][3] {{1,2,3},{2,3,1}};
int (*p)[3]{a}; // 指向的类型是数组
cout<<&a<<endl;
cout<<p<<endl;
cout<<p[1][1]<<endl;
cout<<p+1<<endl; // 增加了 1*数据类型大小=1*3(元素)*4(字节)
```



常量指针：`const 变量类型 *`

常量指针表示这个指针指向的是一个常量的内存地址，常量指针中，不能对其指向的内存进行修改，但指针指向的地址可以改变。

```cpp
const int a {1000};
const int * p = &a;
const int b {2000};
p = &b;
*p = 200; // 不可以 会报错！
```



指针常量：`变量类型 * const`

指针常量表示该指针是个常量，不能修改指针的指向，但是可以修改指针指向的内存的值。

```cpp
int a {1000};
int * const p {&a};
*p = 200;
cout<<*p<<endl;
int b {10};
p = &b;  // 不可以改变指针的指向
```



```cpp
int a[5];
int c = sizeof(a); // 数组可以使用sizeof求内存大小，这个过程是由编译器来实现的
```



**智能指针**：

`std::unique_ptr`：唯一智能指针，只能有唯一一个指针指向对应的内存

```cpp
int * a = new int;
unique_ptr<int> uptr {new int{5}};  // 指向单个对象
unique_ptr<int> intptr;
// intptr = uptr;  不可以指向同一个内存空间
cout<<uptr<<" "<<*uptr<<endl;

unique_ptr<int[]> arrayptr {new int[3]{1,2,3}};
cout<<arrayptr[2]<<endl; // 只能用数组方式访问元素

//c++14之后
std::unique_ptr<int[]> uptr{std::make_unique<int[]>(5)}; // 此处5代表元素个数
std::unique_ptr<int> uptr{std::make_unique<int>(5)}; // 此处5代表初始化值
```

```cpp
uptr.reset(); // 释放内存空间，并将指针设置为nullptr

// release() 返回std::unique_ptr的指针，并且将std::unique_ptr设置为nullptr，但是不会释放其占用的内存空间
int * p = uptr.release(); // 不会释放其占用的内存空间,将指针设置为nullptr
```

```cpp
int * p = uptr.get(); // 返回对应的原始指针
```

`std::unique_prt`指针具备唯一性，因此不能被复制，但是可以转移。

```cpp
int * a = new int;
unique_ptr<int> ptrA {std::make_unique<int>(9)};  // 指向单个对象
unique_ptr<int> ptrB;
// ptrB = ptrA; //不可以！
ptrB = std::move(ptrA); // 转移后ptrA被设置为nullptr
cout<<"addr of aptA: "<<ptrA<<" val of ptrB: "<<*ptrB<<endl;
```



`std::shared_ptr`：共享指针

```cpp
std::shared_ptr<int> sptrA{std::make_shared<int>(9)};
cout<<*sptrA<<endl;

// std::shared_ptr<int[]> sptrB{std::make_shared<int[]>(2)};  // make_shared不支持[]
std::shared_ptr<int[]> sptrB{new int[4]}; 

```

多个共享指针可以指向同一个内存

```cpp
std::shared_ptr<int> sptrA{std::make_shared<int>(9)};
std::shared_ptr<int> sptrB{sptrA};
cout<<sptrA<<" "<< *sptrA<<" " <<sptrB<<" "<<*sptrB<<endl;
```

> 可以有多个share_ptr指向同一地址，同一地址只有当足后一个share_ptr释放的时候才会释放其所占的内存空间。
>
>  share_ptr会记录当前的地址有多少个智能指针调用。

`ptr.use_count()`：返回当前指针有多少个对象调用

`std::shape_ptr.unique()`：如果当前指针是唯一拥有该指针的，会返回true  `c++ 17 已经用不了了`

`std::shape_ptr.reset()`： 会将当前共享指针设置为nullptr，如果当前指针是最后一个拥有该指针的对象，会释放内存。







#### 动态内存分配

---

C语言内存分配：

```cpp
// void * malloc(size_t size);
int * p = (int *)malloc(sizeof(int) * 10);
if(p == nullptr){
  //分配内存失败
}else{
  // 进一步操作
}
```

```CPP
// void * calloc(size_t count,size_t size);  为用户分配count * size 字节个内存，并返回内存分配的地址
int * p = (int *)calloc(20,sizeof(int));
```

> calloc 和 malloc 的区别，calloc会默认把分配的内存清0.
>
> 但是calloc的效率没有malloc高。

```cpp
// void * realloc(void * block,size_t size);
//realloc将为用户重新分配内存，block是已经分配好的内存，size是要求重新分配的大小，函数返回重新分配的内存
int *p = (int*)malloc(10);
 p = (int*)realloc(p,100);
```

>  realloc 还会将之前的数据拷贝

```cpp
void free(void * block); // 用于释放上述 申请的内存
```

C++内存分配：

```cpp
int * p = new int;  // 申请内存
delete p;

int * p = new int[10];  
delete []p;         // 释放内存
```

动态内存分配的风险：

- 悬挂指针问题：释放完内存后没有将指针指向为nullptr
- 重复释放内存：释放完之后，另一个指向当前内存的指针再次释放
- 内存碎片问题：频繁的申请和释放小块内存会造成内存碎片问题，对于嵌入式开发以及要求较高的开发要注意该问题。

复制内存：

```cpp
// void	*memcpy(void *__dst, const void *__src, size_t __n);
int a[3]{1,2,3};
int * b = new int[3];
memcpy(b,a,3*sizeof(int));
```

设置内存：

```cpp
// void * memset(void * dst,int val,size_t size);
int *p = new int[100];
memset(p,12,100 * sizeof(int));
```



**堆和栈**：

堆本质是一块空闲内存（c++把堆称为自由存储区）。malloc和new 都是操作系统从堆中申请内存。

栈，是程序在编译时就确定了大小的一段内存区域，主要是用于临时变量的存储，栈的效率高于堆，但是容量有限。







#### 引用

---

引用是创建一个变量的引用名称

语法：`type &ref = val`，此处的`&`不是取地址操作符，起的是标识作用  [引用是什么？--给空间取别名]

> 本质：内部实现是一个常量指针`type * const ref = &val`[即不能改变指针指向]，因此 **引用所占用的空间大小与指针相同**

> **使用场景**：
>
> 1. 作为函数参数传递数据，节约空间
> 2. 作为函数返回值,但是注意   **不要返回局部变量的引用**   [因为局部变量在函数结束后内存会被回收]

> **注意**：
>
> 1. 必须在**声明引用变量时进行初始化**
> 2. **初始化后不能改变**
> 3. **不能有NULL引用**，必须确保引用是和一块合法的存储单元关联
> 4. 如果函数当左值，那么该函数必须返回引用**[**针对使用场景的第2条**]**



建立数组的引用：

```cpp
int arr[] = {1,2,3,4,5};
//方法1： 
//定义数组类型 
typedef int(MY_ARR)[5];
//建立引用
MY_ARR &  arr2 = arr;

//方法2：
//直接定义引用
int (&arr3)[5] = arr;

//方法3：
typedef int (&MY_ARR1)[5];
MY_ARR1 arr4 = arr
```

```cpp
// 数组的引用
int a[100]{};
int (&b) [100] = a; // 数组引用 b是一个引用 引用类型是int[100] 其他类型不行！比如int[120]
cout<<a<<endl;
cout<<b<<endl;


//作为函数参数传递后:
//  --- 优势：可以在函数内部更方便的操作
//  --- 劣势：数组元素不固定就不行了
void show(int (&a) [100]){
    int n  = sizeof(a);
    for(auto x : a){
        cout<<x<<endl;
    }
}
int main(){
    int a[100]{};
    show(a);
}

```



指针的引用（难点）：

> c语言中如果想改变一个指针的指向而不是它所指向的内容，函数声明可能这样:`void fun(int**);`，这样很麻烦，传参还要传指针的地址

```cpp
char * p  = "hello!";
char* &p1 = p;
cout<<p1<<endl;

struct Teacher{
	int mAge;
};
//指针间接修改teacher的年龄
void AllocateAndInitByPointer(Teacher** teacher){
	*teacher = (Teacher*)malloc(sizeof(Teacher));
	(*teacher)->mAge = 200;  
}
//引用修改teacher年龄
void AllocateAndInitByReference(Teacher*& teacher){
	teacher->mAge = 300;
}
void test(){
	//创建Teacher
	Teacher* teacher = NULL;
	//指针间接赋值
	AllocateAndInitByPointer(&teacher);
	cout << "AllocateAndInitByPointer:" << teacher->mAge << endl;
	//引用赋值,将teacher本身传到ChangeAgeByReference函数中
	AllocateAndInitByReference(teacher);
	cout << "AllocateAndInitByReference:" << teacher->mAge << endl;
	free(teacher);
}
```

常量引用：

格式:`const type &ref = val;`

> 注意：
>
> 1. 字面量不能赋值给引用，但是能够赋值给常量引用
>
>    ```cpp
>    const int &ref3 = 10;
>    //编译器会把上面代码变为：int tmp = 10;const int &ref3 = tmp;
>    ```
>
> 2. `const`修饰的引用，不能修改





#### 函数

---

**内联函数**：出现的原因是想解决#define的一些缺陷

格式：在函数前加上`inline`，即向编译器申请成为内联函数，因此**内联仅仅只是给编译器一个建议**。[最终决定权不在程序员，由编译器决定，因此不可控]

> 没有加`inline`的函数也有可能成为内联函数

> 什么时候不会成为内联函数：
>
> 1. **有过多的条件判断语句**
> 2. **存在循环语句**
> 3. **函数体过大**
> 4. 对函数进行取址操作

> 内联函数的好处：
>
> 1. 有宏函数的效率，但规避了其缺点
> 2. 类内部定义的成员函数默认加上`inline`





函数的引用参数：

- 指针可以传入nullptr，而引用不可以。

- 引用作为参数不能出现隐式转换

  ```cpp
  void add(int& a){
      a += 10;
  }
  
  int main(){
      float a = 11234.02f;
      add(a);  // 会报错！
      cout<<a<<endl;
  }
  ```

  



不定量函数参数：

```cpp
#include<iostream>
#include<cstdarg>
using namespace std;

int add(unsigned int count,...){
    int rt{};
    char * p;
    va_start(p,count);  // 将参数数据赋值给p
    for(int i = 0;i < count;i++){
        rt += va_arg(p,int); // 以int类型读取参数
    }
    va_end(p); // 释放指针
    return rt;
}

int main(){
    cout<<add(8,2,3,3,4,4,5,5,10)<<endl;
}
```







函数的默认参数：[增加函数的灵活性]

> 注意：
>
> 1. 函数的 默认参数从左向右，**如果一个参数设置了默认参数，那么这个参数之后的参数都必须设置默认参数。**
>
> 2. 如果函数声明和函数定义分开写，函数声明和函数定义不能同时设置默认参数。
>
>    ```cpp
>    void func(int a,int b = 10);
>    void func(int a,int b){
>              }
>    ```

函数的占位参数：[应用于运算符重载时区分前++和后++]

```cpp
void func(int a,int){
    
}
func(10,20);
//占位参数也可以有默认值
void func2(int a,int = 1){
    
}
func2(10);
```



函数重载：允许函数名相同

> 实现条件：
>
> 1. 同一个作用域
> 2. `参数个数`不同
> 3. `参数类型`不同
> 4. `参数顺序`不同

> 函数重载**没有 返回值 的相关要求**。为什么呢？
>
> 当编译器能从上下文中确定唯一的函数的时，如`int ret = func()`,这个当然是没有问题的。然而，我们在==编写程序过程中可以忽略他的返回值==。那么这个时候,假如一个函数为`void func(int x);`另一个为`int func(int x); `当我们直接调用`func(10)`,这个时候编译器就不确定调用那个函数。所以在c++中禁止使用返回值作为重载的条件。

> 调用重载函数时的注意事项：
>
> 1. 严格的类型匹配,若类型不匹配则尝试转换，转换成功就调用，失败就报错。
>
> 2. 函数重载和函数默认参数一起使用时需要注意二义性问题出现
>
>    ```cpp
>    void func(int a,int b= 10){
>              
>    }
>    void func(int a){
>              
>    }
>    a = 10;
>    func(a);//出现二义性
>    ```

> 函数重载的原理：**汇编过程取别名**
>
> 编译器为了实现函数重载，也是默认为我们做了一些幕后的工作，编译器用不同的参数类型来修饰不同的函数名，比如`void func(); `编译器可能会将函数名修饰成`_func`，当编译器碰到`void func(int x)`,编译器可能将函数名修饰为`_func_int`,当编译器碰到`void func(int x,char c)`,编译器可能会将函数名修饰为`_func_int_char`





#### 类

---

类`class`

> `struct`和`class`的区别 ?
>
> `class`默认访问权限为`private`,`struct`默认访问权限为`public`.

访问权限：`private、protected、public`

> 1. 类内没有访问权限限制，均都可访问
> 2. 公有`public`可以在对象外部访问，私有`private`和保护`protected`可以在对象外部访问
> 3. 子类的类内可以访问父类的保护权限`protected`成员

类的**静态成员变量**`static`

> 注意点：
>
> 1. 静态成员变量必须在**类中声明**，在**类外定义**
>
>    ```cpp
>    AAAA::static_a = 100;//类外定义
>    ```
>
> 2. **静态数据成员不属于某个对象**，在为对象分配空间中不包括静态成员所占空间[即静态成员变量属于类，**不属于对象，所有对象共享**]
>
> 3. 静态数据成员可以通过类名或者对象名来访问
>
> 4. **生命周期为整个程序，作用域在类内**

类的**静态成员函数**：

**注意点**：

> 1. 静态成员函数**只能访问静态成员变量**
>2. 静态成员函数也有访问权限
> 3. 普通成员函数可访问静态成员变量、也可以访问非静态成员变量

> 定义`静态const`数据成员时，最好在类内部初始化。

为什么要出现静态成员变量？

> 节省空间，属于类，所有对象共享。

静态成员应用的一个实例： 「单例模式」---- 一个类只能实例化一个对象 

> 单例模式常常用于只需要一个对象的场景中

```
思路：
-把「无参构造函数」和「拷贝构造函数」私有化 [为了防止用户进行普通构造和拷贝构造]
-定义一个类内的静态成员指针，在类外初始化时，new一个对象
-把指针的权限设置为私有，然后提供一个静态成员函数让外面获取这个指针
```

```
例子：
用单例模式，模拟公司员工使用打印机场景，打印机可以打印员工要输出的内容，并且可以累积打印机使用次数。
```

```cpp
#include <iostream>
#include <string>
using namespace std;
//打印机类
class Printer {
public:
	void printInfo(string info) {
		cout << "当前是第" << printTimes << "次打印...." << endl;
		cout << "打印内容如为:  " << info << endl;
		printTimes++;
	}
	//静态成员函数只能访问静态变量
	static Printer* getObject() {
		return pter;
	}
private:
	//私有化，防止用户进行普通构造和拷贝构造
	Printer() {
		printTimes = 0;
	}
	Printer(const Printer& other) {
	}
private:
	//指向当前类的指针，静态成员变量
	static Printer* pter;
    //普通成员变量 打印机打印次数
	int printTimes;
};
//在类外进行一次初始化
//为什么这里能进行一个构造[初始化]呢？
//因为有Printer::作用域，编译器把它当作了在类内，而在类内没有访问权限限制
Printer* Printer::pter = new Printer;

int main()
{
	Printer* pA = Printer::getObject();
	pA->printInfo("C++真tm有意思!");
	pA->printInfo("C++真tm难!");
	Printer* pB = Printer::getObject();
	pB->printInfo("我觉得C++很简单!");
	cout << "pA:" << pA << "   pB:" << pB << endl;
	system("pause");
	return 0;
}
```

输出内容如下：

```
当前是第0次打印....
打印内容如为:  C++真tm有意思!
当前是第1次打印....
打印内容如为:  C++真tm难!
当前是第2次打印....
打印内容如为:  我觉得C++很简单!
pA:010D13B0   pB:010D13B0
```

`可见pA和pB是同一个对象~`

---

「构造函数」和「析构函数」：

实例化对象时，内部做了两件事：**分配空间**和**调用构造函数**进行初始化；在对象销毁前，编译器调用析构函数[释放该对象申请的堆空间等操作]。

> 注意：
>
> 1. 构造函数和析构函数必须是公有的。构造函数私有时，实例不了对象 ==> 单例。
> 2. **构造函数可以重载，析构函数不行**。
> 3. 构造函数和析构函数**没有返回值**。
> 4. 编译器会默认提供默认构造函数和默认析构函数

**拷贝构造函数**：用一个已有的对象去初始化另外一个对象

> 拷贝构造函数的情况：
>
> 1. 对象以值传递的方式传给函数参数
>
> 2. 用一个对象初始化另一个对象
>
> 3. 函数局部对象以值传递的方式从函数返回
>
>    -VS Debug模式返回的对象和局部对象地址不同
>
>    -VS Release模式返回的对象和局部对象地址相同

```cpp
class AAAA {
private:
	int a;
public:
	AAAA() {
		a = 999;
		cout << "无参构造函数" << endl;
	}
	AAAA(int data) {
		a = data;
		cout << "有参构造函数" << endl;
	}
	AAAA(const AAAA& s) {
		a = s.a;
		cout << "拷贝构造函数" << endl;
	}
};
//测试函数
void test() {
	AAAA a1;		  //调用无参构造函数
	AAAA a2(a1);	  // 调用拷贝构造函数
	AAAA a3 = a1;     //调用拷贝构造函数
	AAAA(10);		  //调用有参构造函数,这是一个匿名对象
	AAAA a4 = 10;     //调用有参构造函数，进行了隐式转换，等价于A s = A(10); 如果不想要优化，则使用 explicit 关键字
   //explicit 只能放在构造函数前面，构造函数只有一个参数或其他参数为默认参数时
	AAAA a5(AAAA(200));   //调用有参构造函数
    AAAA a6 = AAAA(200);  //调用有参构造函数
    AAAA a7 = AAAA(); 	  //调用无参构造函数
}
```

> 注意：
>
> 1. 编译器也提供了`默认拷贝构造函数`，进行成员变量的简单拷贝。
>
> 2. **拷贝构造函数的形参要用引用**。因为编译器会把形参的值传递看成一个拷贝构造函数，造成死循环构造函数，而引用只是创建了一个别名。
>
> 3. 不能调用拷贝构造函数去初始化匿名对象
>
>    ```CPP
>    AAAA t1;
>    //会报错error C2086:“Teacher t1”: 重定义
>    AAAA(t1);  //此时等价于 AAAA t1;
>    ```
>
> 4. 匿名对象的生命周期在当前行
>
> 5. 如果匿名对象有东西接，那么就不是匿名对象`AAAA a6 = AAAA(200);`

编译器提供构造函数规则：

> 1. 默认情况下，C++编译器至少为我们写的类增加3个函数
>    - **默认构造函数**(无参，函数体为空)
>    - **默认析构函数**(无参，函数体为空)
>    - **默认拷贝构造函数**，对类中**非静态成员属性进行简单值拷贝**
> 2. 如果用户定义拷贝构造函数，C++不会再提供任何默认构造函数
> 3. 如果用户定义了普通构造(非拷贝)，C++不在提供默认无参构造，但是会提供默认拷贝构造

多个对象的构造和析构：

> 1. 如果类有成员对象，那么**先调用成员对象的构造函数，再调用本身的构造函数，析构函数调用顺序相反；**
> 2. 成员对象的构造函数调用和定义的顺序一样
> 3. 必须保证成员对象的构造和析构能被调用

初始化列表：

> 1. 如果使用了初始化列表，那么所有的构造函数都要使用初始化列表
> 2. 初始化列表只能使用于构造函数

---

对象的深、浅拷贝：

> 默认的 拷贝构造函数 进行了浅拷贝，简单的赋值操作

**浅拷贝问题**：调用默认拷贝构造函数后，新对象和旧对象指向同一块空间，进行析构函数时被释放了两次，引发错误。

使用深拷贝解决：

```cpp
class  person{
public:
    person(char * Name,int Age){
        name = (char *)malloc(sizeof(strlen(Name)+1));
        strcpy(name,Name);
        age = Age;
    }
    person(const person & p){
        //即申请新的一块内存，并初始化
        name = (char *)malloc(sizeof(strlen(P.name)+1));
        strcpy(name,P.name);
        age = P.age;
     }
    ~person(){
    	if(name != NULL){
            free(name);
        }
    }
 private:
    char * name;
    int age;
}
```





C++对象模型

> 1. 空类大小为 1
> 2. 类的成员函数不占类大小[在公共函数代码区]，静态成员不占类大小。普通成员变量占用类的大小
> 3. ==类的成员函数和成员变量是分开存储的==。

<img src="F:\Typora\Picture\Cpp\2021-07-23_232843.jpg" alt="2021-07-23_232843" style="zoom:50%;" />

`this`指针：一种隐含指针，**指向被调的成员函数所属对象**

> 注意：
>
> 1. 每个对象都有一个隐藏this指针，但不属于对象(不影响`sizeof(对象)`的结果)，是编译器添加的。
> 2. 编译器会把this指针传入成员函数内。
> 3. **静态成员函数中不能使用this指针**[因为静态成员不属于对象，属于类]
> 4. this指针不能改变[说明**this类型是常指针** `class const * p`]

C++编译器对普通成员函数的内部处理：

![2021-07-23_233143](F:\Typora\Picture\Cpp\2021-07-23_233143.jpg)

> this指针的使用：
>
> 1. 当形参和成员变量同名时，可用this指针来区分形参和成员变量
> 2. 在类的非静态成员函数中**返回对象本身**，可使用`return *this`

---

**常函数**与**常对象**

`const`修饰成员函数：在成员函数的后面加上`const`，使函数成为**常函数**

```cpp
//const修饰成员函数
class Person{
public:
	Person(){
		this->mAge = 0;
		this->mID = 0;
	}
	//在函数括号后面加上const,修饰成员变量不可修改,除了mutable变量
	void Operate() const{
		//this->mAge = 200; //mAge不可修改
		this->mID = 10; //const Person* const tihs;
	}
	void ShowPerson(){
		cout << "ID:" << mID << " mAge:" << mAge << endl;
	}
private:
	int mAge;
	mutable int mID;//被mutable修饰时可以在常函数中被修改
};
```

> 注意：
>
> 1. 用`const`修饰的成员函数时，`const`修饰`this`指针指向的内存区域，成员函数体内不可以修改本类中的任何普通成员变量。
>
>    ```cpp
>    this指针类型： Person * const this;
>    在常成员函数中的this指针类型被改为： const Person * const this;
>    ```
>
> 2. 当成员变量类型符前用`mutable`修饰时例外。



`const`修饰对象，使其成为了**常对象**。

```cpp
const Person  MY;
```

> 注意：
>
> 1. 常对象,**不能调用普通成员函数**，只能调用`const`的成员函数，即常函数
> 2. 常对象可访问`const`或`非const`数据成员，但是不能修改，除非成员用mutable修饰。

常函数和常对象有什么用呢？

```
如果当前成员函数不打算修改成员变量，就最好定义为常函数
```

**类模板**

定义类模板: 

模板类以下面这样的代码开头

```c++
template <class Type>   //方式1
template <typename Type>//方式2
```

假设定义一个Stack类模板：

```c++
template <class Type>
class Stack{
private:
    enum = {MAX = 10};
    Type items[MAX];
    int top;
public:
    Stack();
    bool Push(const Type & item);
    //...
};
```

当模板被调用时,Type将被具体的类型值取代。

同样可以使用模板成员函数替换原有类的类方法，使用泛型名Type替换typedef标识符，还需要将类限定符从`Classname::改为Classname<Type>::`

```c++
typedef int Item;

bool Stack::Push(const Item & item)
{
    //...
}
//改为如下 模板成员函数
bool Stack<Type>::Push(const Type &item)
{
    //...
}
```

**实例化一个模板类对象：**

```c++
Stack<int> kernels;
Stack<double> colons;
```

看到上述声明后，编译器将按Stack<Type>模板生成两个独立的类声明和两组独立的类方法。

**递归使用模板：**

```c++
ArrayTP< ArrayTP<int,5>,10> twodee; //创建一个包含10个元素的数组，每个元素都是一个包含5个int元素的数组，等价于 int twodee[5][10]
```

**使用多个类型参数：**

```c++
template <class T1,class T2>
class Pair
{
private:
    T1 A;
    T2 B;
public:
    //...
}
```

**默认类型模板参数：**

```c++
template <class T1,class T2 = int>
class Topo
{
private:
    //....
}

Topo<double, double> m1; //T1 double, T2 double
Topo<double> m2; //T1 double, T2 int
```

**具体类模板定义：**

```c++
template <> class Classname<spsecialized-type-name>{/*..*/};

//提供一个专供const char*类型使用的SortedArray模板,此时如果参数为const char * 时，则将指定使用该类模板
template <> class SortedArray<const  char*>{/*...*/};
```

**部分具体化：**

template后面的<>声明的是没有具体化的类型参数。如果指定所有类型，则<>内将为空，这将导致显示具体化。

```c++
template <class T1> class Pair<T1,int>{/*...*/};//将第二个参数具体化为int类型
```

**成员模板：**

模板类将另一个模板类和模板函数作为其成员。

**将模板用作参数：**

```c++
template <template <typename T> class Thing>
class Crab;
```





#### 友元

---

**友元**：使得全局函数、成员函数和类可以访问私有成员，即给一个函数、或者某个类拥有特殊权限

> 特点：
>
> 1. **有元函数不是类的成员函数**，没有this指针
> 2. **friend关键字只出现在声明处**
> 3. 其他类、类成员函数、全局函数都可声明为友元
> 4. **友元函数可访问对象任意成员属性**，包括私有属性
> 5. 一定程度上破坏了类的封装性

```cpp
class Building;

class MyFriend{
public:
	void LookAtBedRoom(Building& building);
	void PlayInBedRoom(Building& building);
};
//类外定义成员函数
void MyFriend::LookAtBedRoom(Building& building){
	cout << "我的朋友参观" << building.mBedroom << endl;
}
void MyFriend::PlayInBedRoom(Building& building){
	cout << "我的朋友玩耍在" << building.mBedroom << endl;
}

class Building{
	//全局函数做为友元函数
	friend void CleanBedRoom(Building& building);
#if 0
	//将类的成员函数做为友元函数
	friend void MyFriend::LookAtBedRoom(Building& building);
	friend void MyFriend::PlayInBedRoom(Building& building);
#else	
	//将某个类作为友元类 此类可以访问该类的私有成员变量
	friend class MyFriend;
#endif

public:
	Building();
public://公有成员变量
	string mSittingRoom;
private://私有成员变量
	string mBedroom;
};
//构造函数
Building::Building(){
	this->mSittingRoom = "客厅";
	this->mBedroom = "卧室";
}
//友元全局函数
void CleanBedRoom(Building& building){
	cout << "友元全局函数访问" << building.mBedroom << endl;
}

int main(){
	Building building;
	MyFriend myfriend;
    
	CleanBedRoom(building);
	myfriend.LookAtBedRoom(building);
	myfriend.PlayInBedRoom(building);
    
	system("pause");
	return EXIT_SUCCESS;
}
```

> 注意：
>
> 1. **友元关系不能被继承**。
>
>    ```
>    你爸爸的朋友不一定是你的朋友
>    ```
>
> 2. **友元关系是单向的**。类A是类B的朋友，但类B不一定是类A的朋友。
>
>    ```
>    你把他当朋友，但他不一定把你当朋友
>    ```
>
> 3. **友元关系不具有传递性**。类B是类A的朋友，类C是类B的朋友，但类C不一定是类A的朋友。
>
>    ```
>    你朋友的朋友不一定是你的朋友
>    ```





#### 继承

---

**私有继承**

使用私有继承，基类的公有成员和保护成员都将成为派生类的私有成员。因此，基类的方法将不会成为派生对象共有接口的一部分，但是可以在派生类的成员函数中使用它们。

```c++
class Student :private std::string, private std::valarray<double> //多重继承
{
    private:
    	//...
    public:
    	//...
};
```

**访问基类的方法：**

使用私有继承时，只能在派生类的方法中使用基类的方法。

```c++
double Student::Average() const
{
    if(scores.size() > 0)
    {
        return scores.sum / scores.size();//scores为基类valarray
    }else
    {
        return 0;
    }
}
```

私有继承使得能够使用类名和作用域解析运算符来调用基类方法：

```c++
double Student::Average() const
{
    if(std::valarray<double>::size() > 0)
    {
        return std::valarray<double>::sum/std::valarray<double>::size();
    }else
    {
        return 0;
    }
}
```

**访问基类对象：**

使用作用域解析运算符可以访问基类的方法，但若要使用基类对象的本身，则需要使用强制类型转换。由于Student类是从string类派生来的，因此可以通过强制类型转换，将Student对象转换为string对象。（通过*this可以调用方法的对象）

```c++
const string & Student::Name() const
{
    return (const string &) *this;
}
```

**使用包含还是私有继承：**

通常，应使用包含来建立has-a关系；但是如果新类需要访问原有类的保护成员，或需要重新定义虚函数，则应使用私有继承。

**使用using重定义访问权限：**

假设要让基类的方法在派生类外面可用，可以将函数调用包装在另一个函数调用中，即使用using声明来指定派生类可以使用特定的基类成员，即使采用的是私有继承。

可以在student.h的**公有部分**加入如下using声明：

```c++
class Student :private std::string, private std::valarray<double>
{
    //...
    public:
    	using std::valarray<double>::min;
    	using std::valarray<double>::max;
    //...
};
```









#### 运算符重载

---

**运算符重载**：目的是为了简化语法，只是另一种函数调用的方式

> 注意：
>
> 1. 运算符重载不能改变本来寓意，不能改变基础类型寓意
> 2. 不能改变运算符优先级，不能改变运算符的参数个数

> 语法：
>
> 定义重载的运算符就像定义函数，只是该函数的名字是`operator@`,这里的`@`代表了被重载的运算符。函数的参数中参数个数取决于两个因素。
>
> - 运算符是一元(一个参数)的还是二元(两个参数)；
>
> - 运算符被定义为全局函数(对于一元是一个参数，对于二元是两个参数)还是成员函数(对于一元没有参数，对于二元是一个参数-此时该类的对象用作左耳参数)



![img](F:\Typora\Picture\Cpp\clip_image002.jpg)

> 除了赋值号`=`外，基类中被重载的操作符都将被派生类继承。

> `=, [], () 和 -> `操作符只能通过成员函数进行重载 
>
> `<< `和 `>>`操作符最好通过友元函数进行重载
>
> 不要重载 `&&` 和 `||` 操作符，因为无法实现短路规则



> 运算符重载过程：
>
> - 编译器看到两个对象的运算操作，那么编译器就回去寻找有没有对于的`operator`重载函数
>
> - 编译器检查参数是否对应
>
> - 没有问题的话编译器就会执行该函数



**+、-、*、/号运算符重载**

> 以一个复数对象的+、-、*、/运算作为例子

```cpp
//ComplexNUm.h
#pragma once
#include<iostream>
using namespace std;
//复数类 
class ComplexNum
{
public:
    //构造函数
	ComplexNum();
	ComplexNum(double r, double i);
	ComplexNum(const ComplexNum& other);
	void Printcomplex();
    //三个成员函数重载运算符
	ComplexNum operator+(ComplexNum& other) {
		ComplexNum tmp(this->real + other.real, this->image + other.image);
		return tmp;
	}
	ComplexNum operator*(ComplexNum& other) {
		ComplexNum tmp(this->real * other.real - this->image * other.image, this->real * other.real + this->image * other.image);
		return tmp;
	}
	ComplexNum operator/(ComplexNum& other) {
		double mul = other.real * other.real + other.image * other.image;
		ComplexNum tmp((this->real * other.real + this->image * other.image) / mul, (this->image * other.real - this->real * other.image) / mul);
		return tmp;
	}
public:
	double real;
	double image;
};
```

```cpp
//ComplexNUm.cpp
#include "ComplexNUm.h"
ComplexNum::ComplexNum() {
	real = 0.0;
	image = 0.0;
}
ComplexNum::ComplexNum(double r, double i) {
	real = r;
	image = i;
}
ComplexNum::ComplexNum(const ComplexNum& other) {
	real = other.real;
	image = other.image;
}
void ComplexNum::Printcomplex() {
	if (image >= 0)
		cout << real << "+" << image << "i" << endl;
	else
		cout << real << image << "i" << endl;
}
//全局重载运算符 因为是二元运算符 所以有两个参数
ComplexNum operator-(ComplexNum& my, ComplexNum& other) {
	ComplexNum tmp(my.real - other.real, my.image - other.image);
	return tmp;
}
```

**左移/右移运算符重载**

> `<<`注意点：
>
> 1. 形参和实参是一个对象
> 2. `ostream`形参和返回值要引用，因为`ostream`中把拷贝构造函数私有了
> 3. 如果要和`endl`一起使用，那么必须返回`ostream`的对象

> 对与`cout<<endl;` `endl`是一个函数，作为参数传入了`operator<<(endl)`

```cpp
//重载<<是为了方便打印对象
ostream &operator<<(ostream &out,Person & m){
    out<<"name"<<m.name<<" age:"<<m.age<<endl;
    return out;
}
//如果Person类的name、age属性为private，可以将其重载运算符函数定义为友元函数
```

```cpp
istream & operator>>(istream &in,Person & m){
    in>>m.name>>m.age;
    return in;
}
```

**赋值=运算符的重载**

> 默认的赋值运算符重载函数进行了简单的赋值操作[**可能会出现内存泄露和同一块内存被释放两次**]，如下

<img src="F:\Typora\Picture\Cpp\01赋值运算符重载.png" alt="01赋值运算符重载" style="zoom: 67%;" />

因此需要重写赋值运算符重载函数

```cpp
//返回Student是为了防止 s1 = s2 = s3的错误；
Student & operator=(const Studen &stu){
    //因为不能确定this指向的空间能否装下stu内数据，需要先释放，再重新申请
     if(this->pName != NULL){
         delete[] this->pName;
         this->pName = NULL;
     }
    //重新申请空间
    this->pName = new char [strlen(stu.pName) + 1];
    strcpy(this->pNmae,stu.pName);
    
    return *this;
}
```

> **上述为什么要返回引用呢？**
>
> 没有引用则会调用拷贝构造，例如对于s1 = s2 = s3,如果返回的是值，则会产生新的对象，则赋值运算符本来的意义就错了

**关系运算符重载**

```cpp
bool operator==(Student & stu){
    return this->id == stu.id;
}
// > < 等关系运算符同理
```

前++和后++运算符重载 [运用到了占位参数]

```cpp
//前置++
Person & operator++(){
    ++this->id;
    return *this;
}

//后置++
Person operator++(int ){//占位参数，必须是int
    Person tmp (*this);//拷贝构造 ，产生了新对象
    this->id++;//修改this
    return tmp;//返回的是tmp
}
//后置++不能返回引用，因为tmp为局部对象
//优先前置++，因为效率高，不需要产生新的对象
```

**数组下标重载**

```cpp
int &operator[](int index){
    if(this->mSize <= index)
    	this->mSize++;
    return this->arr[index];
}
```

**指针运算符重载**

```cpp
class Smartptr{
 public:
    Smartptr(){
            
        }
    Smartptr(Person * p){
        person = p;
    }
    Person & operator*(){
        return *(this->person);
    }
    Person * operator->(){
        return this->person;
    }
private:
    Person * person;
}
Person * p = new Person();
SmartPtr smp(p);
//假设Person类有printfinfo;方法，则可以直接使用->间接调用
smp->printfinfo();
(*smp).printfinfo();
```

**重载函数调用符号**  

```cpp
void operator()(){
    cout<<"我其实不是个函数"<<endl;
}
```

为什么要重载函数调用符号？

> 方便代码维护：写成全局重载函数，大家都可以调用
>
> 作为算法的策略
>
> 方便有权限的调用函数

#### 智能指针

---

智能指针类：管理另一个类的释放  [利用了内存栈的特性——会调用析构函数]

```cpp
class Smartptr{
    public:
    	Smartptr(){
            
        }
    	Smartptr(Person * p){
            person = p;
        } 
    	~Smartptr(){
            if(this->person != NULL){
                delete this->person;
                this->person = NULL;
            }
        }
    private:
    	Person * person;
}

void test(){
    Person * temp = new Person;
    Smartptr  smp(temp);
}
//当test()结束时,smp会调用析构函数从而释放申请的内存空间，防止内存泄露
```



#### 面向对象（继承、多态）

---

面向对象的三大特性：`封装`、`继承`和`多态`

> `封装`：可以实现类对部分成员和方法进行隐藏，只暴露想要暴露的细节。
>
> ​	-把变量（属性）和方法（操作）合成一个整体，封装在一个类中
>
> ​	-对变量和方法进行访问控制[设置权限]
>
> `继承`：类与类之间的关系，作用是==可以避免公共部分代码的重复开发，减少代码和数据冗余==
>
> `多态`：即“一个接口有多种方法[形态]"，程序运行时才决定调用的函数



**继承**： 提高代码的复用性、扩展类的性能

> 构造函数和析构函数不会被继承，因为其是每个类特有的。
>
> `operator=`也不能被继承

> B类继承于A类，或称从类A派生类B。这样的话，类A成为基类（父类）， 类B成为派生类（子类）。
>
> ```cpp
> class 派生类名 :继承方式 基类名{
> }
> ```

> 继承方式：公有继承、私有继承、保护继承
>
> ==继承方式影响着派生类访问基类成员的权限==

![img](F:\Typora\Picture\Cpp\clip_image002.png)

**继承中的构造和析构**：

> 1. 子类对象在创建时会首先调用父类的构造函数，父类构造函数执行完毕后，然后成员变量如果有类变量则会调用成员变量的构造函数，最后才会调用子类的构造函数。
> 2. 当父类构造函数有参数时，需要在子类初始化列表(参数列表)中显示调用父类构造函数
>
> ==析构函数调用顺序和构造函数相反==

继承中出现**同名成员的处理方法**：

> 同名时，子类成员会`隐藏`父类的同名成员。注意：隐藏不是覆盖
>

> **访问方式**：通过作用域访问 `子类.父类名::成员名`

> 对于同名成员函数，若子类和父类同名，则**父类的所有函数重载都会被隐藏**

**继承中的静态成员特性**：

> 1. 静态成员可以被继承 且 静态成员变量在继承时候,无论父类还是子类都**共享同一段内存空间**
> 2. 父类中被继承的静态成员变量一样会被同名的子类成员变量隐藏
> 3. 父类中被继承的静态成员函数中，其所有重载函数也都会被隐藏
> 4. 如果我们改变基类中一个函数的特征，所有使用该函数名的基类版本都会被隐藏

多继承 [**不建议使用**]

> 从多个类继承可能会**导致**函数、变量等同名导致较多的**歧义**。解决方法就是显示指定调用那个基类的成员。

虚继承的原理[虚继承常用于解决 菱形继承的二义性问题]

> 编译器给类添加了一个指针，指针指向类似于表的组织，该表记录了该指针距离变量的偏移量

---

**动态联编和静态联编**

> **静态联编**（早绑定）：编译器会根据函数调用的对象类型，在编译阶段就确定函数的调用地址  [Ex:运算符重载和函数重载]

> **动态编译**（晚绑定）：在运行阶段才确定调用哪个函数  ===> **多态**的实现就运用到了**动态联编**
>
> - 在普通成员函数前面加`virtual`，该函数变为虚函数，是告诉编译器这个函数要 晚绑定
>
> - 作用：动态编译可以晚绑定函数调用的地址，这样可以扩展功能，不修改前面代码的基础上进行项目的扩充



动态联编过程会涉及类型转换问题，因为需要子类指针转换成父类指针

> 1. 子类转换成父类（向上转换）：编译器认为指针的寻址范围缩小了，所以是安全的
> 2. 父类转换成子类（向下转换）：译器认为指针的寻址范围扩大了，所以是不安全的



---

**多态**：同一个操作作用于不同的对象，可以有不同的解释，会产生不同的效果

> 作用：**改善了代码的可读性和组织性**，同时也使创建的程序具有**可扩展性**，项目**不仅**在最初**创建时期可以扩展**，**而且当项目在需要有新的功能时也能扩展**。

> 发生条件：
>
> 1. 有继承。
> 2. 重写父类的虚函数。
> 3. 父类指针指向子类对象。

多态实现原理：

![多态实现原理图1](F:\Typora\Picture\Cpp\多态实现原理图1.png)

![多态实现原理图2](F:\Typora\Picture\Cpp\多态实现原理图2.png)

![多态实现原理图3](F:\Typora\Picture\Cpp\多态实现原理图3.png)

![多态实现原理图4](F:\Typora\Picture\Cpp\多态实现原理图4.png)



**纯虚函数**和**抽象类**

> 依赖倒转：业务层依赖抽象层，实现层依赖抽象层
>
> ​	-[抽象层：含有虚函数的基类 实现层：派生类]
>
> 开闭原则：对修改源代码关闭，对扩展功能开放

```cpp
class Person{//抽象类
public:
    //纯虚函数
    virtual int getnum(int a,int b) = 0;
}
```

> 继承抽象类的派生类需要实现基类的所有虚函数，不然派生类也会变成抽象类

引入纯虚函数的原因：

1. 为了方便使用多态特性，我们常常需要在基类中定义虚拟函数。

2. 在很多情况下，基类本身生成对象是不合情理的。

   > 例如，动物作为一个基类可以派生出老虎、孔雀等子类，但动物本身生成对象明显不合常理。

抽象类：有**纯虚函数**的类叫抽象类，不能实例化对像。

> 抽象类作为一种特殊的类，其目的是为了**抽象**和**设计**

主要作用：

> 将有关的操作作为结果接口组织在一个继承层次结构中，由它来为派生类提供一个公共的根，派生类将具体实现在其基类中作为接口的操作。所以派生类实际上刻画了一组子类的操作接口的通用语义，这些语义也传给子类，子类可以具体实现这些语义，也可以再将这些语义传给自己的子类。

注意事项：

- 抽象类只能作为基类来使用，其纯虚函数的实现由派生类给出。
- 如果派生类中没有重新定义纯虚函数，而只是继承基类的纯虚函数，则这个派生类仍然还是一个抽象类。





模板方法模式

![2016-07-07_204741](F:\Typora\Picture\Cpp\1111.jpg)

```cpp
//抽象 制作饮品类
class AbstractDrinking{
public:
	//烧水
	virtual void Boil() = 0;
	//冲泡
	virtual void Brew() = 0;
	//倒入杯中
	virtual void PourInCup() = 0;
	//加入辅料
	virtual void PutSomething() = 0;
	//规定流程
	void MakeDrink(){
		Boil();
		Brew();
		PourInCup();
		PutSomething();
	}
};

//制作咖啡 要将所有纯虚函数都实现，不然还是一个抽象类
class Coffee : public AbstractDrinking{
public:
	//烧水
	virtual void Boil(){
		cout << "煮农夫山泉!" << endl;
	}
	//冲泡
	virtual void Brew(){
		cout << "冲泡咖啡!" << endl;
	}
	//倒入杯中
	virtual void PourInCup(){
		cout << "将咖啡倒入杯中!" << endl;
	}
	//加入辅料
	virtual void PutSomething(){
		cout << "加入牛奶!" << endl;
	}
};

//制作茶水 要将所有纯虚函数都实现，不然还是一个抽象类
class Tea : public AbstractDrinking{
public:
	//烧水
	virtual void Boil(){
		cout << "煮自来水!" << endl;
	}
	//冲泡
	virtual void Brew(){
		cout << "冲泡茶叶!" << endl;
	}
	//倒入杯中
	virtual void PourInCup(){
		cout << "将茶水倒入杯中!" << endl;
	}
	//加入辅料
	virtual void PutSomething(){
		cout << "加入食盐!" << endl;
	}
};

//业务函数 多态的应用，采用父类指针调用子类对象的方法
void DoBussiness(AbstractDrinking* drink){
	drink->MakeDrink();
	delete drink;
}

void test(){
	DoBussiness(new Coffee);
	cout << "--------------" << endl;
	DoBussiness(new Tea);
}
```

虚析构函数：为了解决基类指针指向派生类对象，并于基类指针释放派生类对象的问题。

纯虚析构函数：有纯虚构函数的类是抽象类，不能实例化对象。

> 纯虚析构函数需要在类外实现







### 常考问题

为什么c++实现多态时候要将父类[基类]的析构函数设置为虚析构函数？

> 将析构函数设为虚函数，是为了防止内存泄漏。

```
解释：如果存在用基类指针指向派生类对象，这样子可以使得基类和派生类的析构函数都被调用，不会造成内存泄漏。如果不把析构函数定义为虚函数，那么只会调用基类的析构函数。
```



---

对比C和C++，讲讲两者的差别。

> 1. C++具有更严格的类型检查
>
>    ```cpp
>    char * p = malloc(10);//C语言中不报错，C++中会报错，不能进行隐式转换
>    ```
>
> 2. C++三目运算符返回的是左值,c语言返回的是右值
>
>    ```cpp
>    int a = 10,b = 20;
>    (a > b ? a : b) = 100;
>    cout<<b<<endl;//打印100
>    ```
>
> 3. C语言`const`为外部链接属性，C++默认为内部链接属性
>
> 4. C语言结构体不能写函数，C++可以,并且还可以不加`struct`关键字



---

C++编译器不能优化的情况：

> 自定义结构类型，编译器不能优化。
>
> 用变量给`const`修饰的局部变量赋值，编译器不能优化。



---

`new/delete` 和 `malloc/free` 有什么差别？

> 对于 `malloc/free` :
>
> 1. 使用`malloc`程序员必须确定对象的长度,`new`不用。
> 2. `malloc`返回一个`void*`指针，C++类型检查很严格，不允许将`void*`赋值给其他任何指针，必须进行强转。
> 3. `malloc`可能申请内存失败，所以必须判断返回值来确保内存分配成功。`new`操作符能确定在调用构造函数初始化之前内存分配是成功的，所有不用显式确定调用是否成功。
> 4. 使用`malloc/free`不会调用对象的构造函数和析构函数，`new/delete`会调用构造函数和析构函数。

> ==建议==:
>
> 1. 在删除指针后，立即把指针赋值为NULL，以免对它删除两次，对一些对象删除两次可能会产生某些问题。
> 2. `new/delete` 和 `malloc/free` 不能混搭使用。

`new`和`delete`在堆上创建、删除数组:

```cpp
int* pArr2 = new int[10]{ 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };
delete[] pArr2; //[]在前！
```

> 当创建一个对象数组的时候，必须对数组中的每一个对象调用构造函数

> 如果对一个`void*`指针执行`delete`操作，这将可能成为一个程序错误，除非指针指向的内容是非常简单的，因为它将不执行析构函数
>
> ```cpp
> void * m = new classA;
> delete m;//不会调用析构函数，从而造成内存泄露
> ```
>
> 这是因为`静态联编`的编译方式，在编译阶段，编译器已经确定了函数的调用地址，C++不认识`void*`，不知道`void *`指向哪个函数，所以不会调用析构函数。



---

重写、重载、重定义

> 重载：同一作用域的同名函数
>
> 1. 同一个作用域
> 2. 参数个数，参数顺序，参数类型不同
> 3. 和函数返回值，没有关系
> 4. `const`也可以作为重载条件`do(const Teacher& t){} do(Teacher& t){}`
>
> 重写(覆盖)：
>
> 1. 有继承
> 2. 子类（派生类）重写父类（基类）的virtual函数
> 3. 函数返回值，函数名字，函数参数，必须和基类中的虚函数一致
>
> 重定义（隐藏）
>
> 1. 有继承
> 2. 子类（派生类）重新定义父类（基类）的同名成员（非virtual函数）