## 容器 和 迭代器

### What is container(容器) ?

**容器**，简单理解起来就是一些**模板类的集合**，它封装的是数据结构（如我们所熟悉的栈、队列、哈希表等等）。

STL提供了三种标准容器：

1. **序列容器**：元素在容器中的位置同元素的值无关（**无序的**），如 vector 向量容器、list 列表容器以及 deque 双端队列容器。
2. **排序容器**：排序容器中的元素默认是**由小到大**排序好的（**有序的**），即便是插入元素，元素也会插入到适当位置。如 set 集合容器和 map 映射容器。
3. **哈希容器**：哈希容器中的元素是未排序的，元素的位置由哈希函数确定。

### What is iterator(迭代器) ?

**迭代器**，迭代器是泛型技术发展的必然结果，我们常常需要遍历容器中的元素，虽然不同容器的内部结构都各有特点，但本质上都是用来存储大量数据的，都是由一串的存储单元组成，因此遍历的操作方法也是类似的，因此就出现了迭代器，从而能够讲容器和算法分离开，实现对数据的访问。

迭代器可以是需要的任意类型，通过迭代器可以指向容器中的某个元素，也可以对该元素进行读/写操作。

迭代器按功能分为**输入迭代器、输出迭代器、前向迭代器、双向迭代器、随机访问迭代器**。

1. **输入、输出迭代器**：操作的对象为输入/输出流。
2. **前向迭代器**：p 是一个前向迭代器，则 p 支持 ++p，p++，*p 操作，还可以被复制或赋值，可以用 == 和 != 运算符进行比较。此外，两个正向迭代器可以互相赋值。
3. **双向迭代器**：在前向迭代器的基础上，双向迭代器还支持--p或p--操作。
4. **随机访问迭代器**：具有双向迭代器的全部功能（功能最强！）并且具有独特的操作：如下
   - p+=i：使得 p 往后移动 i 个元素。
   - p-=i：使得 p 往前移动 i 个元素。
   - p+i：返回 p 后面第 i 个元素的迭代器。
   - p-i：返回 p 前面第 i 个元素的迭代器。
   - p[i]：返回 p 后面第 i 个元素的引用。
   - 可以使用 <、>、<=、>= 运算符进行比较
   - 支持p2-p1（返回的是p2所指向元素和 p1 所指向元素的序号之差）

不同容器中指定使用的迭代器类型也不同。

| 容器                                 | 对应的迭代器类型 |
| ------------------------------------ | :--------------: |
| `array`                              |       随机       |
| `vector`                             |       随机       |
| `deque`                              |       随机       |
| `list`                               |       双向       |
| `set / multiset`                     |       双向       |
| `map / multimap`                     |       双向       |
| `forward_list`                       |       前向       |
| `unordered_map / unordered_multimap` |       前向       |
| `unordered_set / unordered_multiset` |       前向       |
| `stack`                              |      不支持      |
| `queue`                              |      不支持      |

#### 简单使用迭代器

我们使用迭代器对vector容器进行遍历：

```c++
#include<iostream>
#include<vector>

using namespace std;

int main()
{
    vector<char> hello{'h','e','l','l','o','!'};//初始化一个vector容器，并初始化容器元素内容，vector对应的迭代器类型是随机访问迭代器
    cout<<"method one:"<<endl;
    for (int i = 0; i < hello.size(); i++)//调用容器的size()方法获得容器大小
    {
        cout<<hello[i];
    }
    cout<<endl;
    
    cout<<"method two:"<<endl;
    vector<char>::iterator iter;//定义一个指向vector<char> 的正向迭代器 iter
    for (iter = hello.begin(); iter != hello.end(); iter++)//使用 != 运算符比较两个迭代器是否相等，也显示迭代器可以互相赋值
    {
        cout<<*iter; // *迭代器名 表示访问迭代器所指向的元素
    }
    cout<<endl;

    cout<<"method three:"<<endl;
    iter = hello.begin();
    while (iter < hello.end())//迭代器可以使用 < 运算符比较大小
    {
        cout<<*iter;
        iter=iter+2; //迭代器向后移动2个元素每次
    }
    cout<<endl;

    getchar();
    return 0;
}
```

输出结果：

```
method one:
hello!
method two:
hello!
method three:
hlo
```

#### 迭代器的基本操作

##### 定义

尽管不同容器对应着不同类别的迭代器，但这些迭代器有着较为统一的定义方式，具体分为 4 种：

1. 正向迭代器:`容器类名::iterator 迭代器名;`
2. 常量正向迭代器:`容器类名::const_iterator 迭代器名;`
3. 反向迭代器:`容器类名::reverse_iterator 迭代器名;`
4. 常量反向迭代器:`容器类名::const_reverse_iterator 迭代器名;`

*注意：以上 4 种定义迭代器的方式，并不是每个容器都适用。有一部分容器同时支持以上 4 种方式，比如 `array、deque、vector`；而有些容器只支持其中部分的定义方式，例如` forward_list` 容器只支持定义正向迭代器，不支持定义反向迭代器。*

如上程序中我们定义一个vector类型的正向迭代器：`vector<char>::iterator iter;`

##### 访问元素

通过`*迭代器名`表示访问迭代器所指向的元素

##### 修改迭代器指向

可以通过++、--、+=、-=等修改迭代器指向元素。但是单向迭代器和反向迭代器具有区别：

- 正向迭代器进行++操作时，迭代器是指向后一个元素
- 反向迭代器进行++操作时，迭代器是指向前一个元素

##### 其他注意事项

- **双向迭代器不可以比较大小，随机迭代器可以比较大小**
- **输入迭代器用于读入数据，输出迭代器用于写入数据**
- **stack、queue是容器适配器，不可使用迭代器**

如下程序：

```c++
#include<iostream>
#include<list>
using namespace std;
int main()
{
    list<int> l{1,2,3,4,5,6,7};
    list<int>::iterator iter;
    for(iter = l.begin();  iter !=  l.end(); iter++){
        cout<<*iter;
    }
    for (iter = l.begin(); iter < l.end(); iter++)//!注意会报错！因为list<int>为双向迭代器，不能用 < 比较
    {
        cout<<*iter;
    }
    getchar();
    return 0;
}
```

在上文我们定义了一个list容器，注意list容器是双向迭代器。而我们创建的另外一个正向迭代器。我们可以使用 != 对这两个迭代器进行比较，但是不能用 < 因为双向迭代器不支持 < 比较。

因此，尽量使用`for(itr = l.begin(); itr != l.end(); itr++)`，而不是`for(itr = l.begin(); itr < l.end(); itr++)`因为，**前者对于各种迭代器均通用，后者只对随机迭代器通用**，而且，一旦考虑从后向前遍历，就需要改变"<"为">"。

## 序列式容器

序列式容器时一种无序的以线性排列存储方式存储指定类型的容器，可以存储`int`、`double`等类型的数据。

以下 T 代表类型（Type），N 代表数量（Numbers）

序列容器包括以下几类：

- `array<T,N>`：数组容器，定义即可以存储N个T类型的元素，长度固定不变。
- `vector<T>`：向量容器，长度可变（即空间不足时可以申请更多的内存空间）
- `list<T>`：链表容器（双向链表），长度可变，增加\删除元素快，访问元素较慢。
- `deque<T>`：双端队列，长度可变，在头、尾增加或删除元素高效，但在其他位置较慢。
- `forward_list<T>`：正向链表容器（单链表），节省内存但效率比`list`低一些。

### array<T,N>数组容器

头文件：`#include<array>`

**特点**：大小固定，无法动态扩展。

#### 创建一个array

假如我们想创建一个包括了10个double类型的array容器my_array，如下：

```c++
std::array<double,10> my_array;
```

需要加 std:: 是因为是在 std 命名空间下的，并且需要注意array容器的初始化 N 大小必须为**常量**，不能是变量。

我们也可以在创建array容器时并对其初始化，想初始化常规数组一样：

```c++
std::array<double,10> my_array {2.5,4,5.9,0.8};
```

如果不指定初始化值，则默认为0或该元素类型等效 '0' 的值：

```c++
std::array<double,10> my_array {};		
```

#### array成员函数

array有很多成员函数，begin(),end(),size(),等等等等，有需要可以去`http://www.cplusplus.com/reference/stl/`了解下。

下面我们通过实例来简单介绍下成员函数的作用：

```c++
#include<iostream>
#include<array>
using namespace std;

int main()
{
    array<double,10> my_array {4,0.4,8.2,5.3,8.9,0.4,9.9,100.3};//初始化默认全为0

    if (my_array.empty())//empty()函数：判断容器是否为空，和通过 size()==0 的判断条件功能相同，但其效率可能更快。
    {
        cout<<"array is empty!"<<endl;
    }else
    {
        cout<<"array is not empty!"<<endl;
    }

    array<double,10>::iterator iter;
    for  (iter = my_array.begin(); iter !=  my_array.end(); iter++)//通过迭代器遍历
    {//begin():返回指向容器中第一个元素的随机访问迭代器
     //end():返回指向容器最后一个元素之后一个位置的随机访问迭代器
        cout<<*iter<<" ";
    }
    cout<<endl;
    
    cout<<"first val is: "<<my_array.front()<<endl; //front():返回容器中第一个元素的直接引用
    cout<<"last val is: "<<my_array.back()<<endl;   //back():返回容器中最后一个元素的直接应用
    
    cout<<"now filling the array with 0.01"<<endl;
    my_array.fill(0.01);
    for (int i = 0; i < my_array.size(); i++)//调用size()成员函数利用for遍历
    {
        cout<<my_array[i]<<" ";//array容器可以像普通数组一样随机访问元素
    }
    cout<<endl;

    array<double,10> temp{};//新创建一个Array类容器
    my_array.swap(temp);//交换两个array类容器

    for (int i = 0; i < my_array.size(); i += 2)
    {
        my_array.at(i) = i*2; //at():返回容器中 n 位置处元素的引用,n溢出则会抛出 out_of_range 异常。 
    }

    array<double,10>::reverse_iterator r_iter;//定义以一个反向迭代器
    for (r_iter = my_array.rbegin(); r_iter != my_array.rend(); r_iter++)
    {//rbegin():返回指向最后一个元素的随机访问迭代器。
     //rend():返回指向第一个元素之前一个位置的随机访问迭代器。
        cout<<*r_iter<<" ";
    }
    cout<<endl;

    getchar();
    return 0;
}
```

输出：

```
array is not empty!
4 0.4 8.2 5.3 8.9 0.4 9.9 100.3 0 0
first val is: 4
last val is: 0
now filling the array with 0.01
0.01 0.01 0.01 0.01 0.01 0.01 0.01 0.01 0.01 0.01
0 16 0 12 0 8 0 4 0 0
```

`array<T,N>`的常用成员函数及其作用已经在上述程序注释。

##### begin()/end()、rbegin()/rend()、cbegin()/cend()和crbegin()/crend()

C++ 11 标准库还新增加了begin()和end()这2个函数，这两个函数的操作的对象不仅可以是容器，还可以是普通数组，若操作对象是普通数组，则begin()返回的是指向数组第一个元素的指针，end()返回最后一个元素后一个位置的指针(不是最后一个）。

```c++
#include<iostream>
#include<array>
using namespace std;

int main()
{
    int arr[5] = {1,2,3,4,5};
    int * ptr;
    cout<<"first: "<<*begin(arr)<<endl;
    ptr = end(arr);
    cout<<"last : "<<*(--ptr)<<endl;//注意要先 -- 哦！
    getchar();
    return 0;
}
```

输出：

```
first: 1
last : 5
```

同样，还可以使用全局的begin()和end()函数来从容器中获取迭代器。

```c++
auto first = std::begin(value);
auto last = std::end(value);
```

rbegin()/rend()、cbegin()/cend()、crbegin()/crend()用法和begin()/end()用法相似，只不过rbegin()/rend()用于迭代器逆序，cbegin()/cend()、crbegin()/crend()成员函数返回的迭代器，可以用来遍历容器内的元素，也可以访问元素，但是**不能对所存储的元素进行修改**。

**注意事项**：

- 在使用反向迭代器rbegin()/rend()和crbegin()/crend()进行 ++ 和 -- 运算时，++ 指的是迭代器向左移动一位， -- 是迭代器向右移动一位。

##### 访问array容器中的元素

方法1：像普通数组一样，使用`容器名[]`的方式直接访问和使用容器中的元素

```c++
array<int,3> val{1,2,3};
val[2] = val[1] * 5 + val[0] * 2;
```

*注意：使用此方法如果越界不会报错*

方法2：使用成员函数at()

```c++
array<int,3> val{1,2,3};
val.at(2) = val(1)*5 + val(0)*2;
```

*注意：此方法如果越界会抛出错误 std::out_of_range*

方法3：模板函数 `get<n>`

```c++
#include<iostream>
#include<array>
using namespace std;
int main()
{
    array<int,5> val{1,2,4,5,3};
    cout<<"last : "<<get<2>(val)<<endl;//返回 元素 4
    for (int i = 0; i < val.size(); i++)
    {
        cout<<get<i>(val)<<" ";//错误！会报错！
    }
    getchar();
    return 0;
}
```

注意：此方法越界会报错，并且参数的实参必须是一个能够在编译时确定的常量表达式，不能是循环变量！

方法4：成员函数data()，该函数返回指向容器首个元素的指针。

```c++
#include<iostream>
#include<array>
using namespace std;
int main()
{
    array<int,5> val{1,2,4,5,3};
    for (int i = 0; i < val.size(); i++)
    {
        cout<<*(val.data()+i)<<" ";
    }
    getchar();
    return 0;
}
```

### vector< T>向量容器

头文件：`#include<vector>`

**特点**：可以动态调整所占用的内存空间，擅长在尾部插入/删除元素。

#### 创建一个vector

```c++
std::vector<int> int_vec;//创建一个int类型的vector容器
std::vector<double> double_vec{0.2,4.2,3.99};//初始化时指定初始值及元素个数
std::vector<char> char_vec(5);//创建具有5个char类型元素的vector容器，默认值为0
std::vector<int> temp(10,2);//创建具有10个int类型元素且默认值为2的vector容器
std::vector<int> temp2(temp);//通过其他的容器来创建自己的vector容器
```

*注意：采用`std::vector<int> temp(10,2);`创建vecotr容器时，两个参数既可以是常量，也可以是变量。*

#### vector成员函数

下面我们通过实例来简单介绍下vector成员函数的作用：

```c++
#include<iostream>
#include<vector>
using namespace std;

int main()
{
    std::vector<int> int_vec{1,9,8,8,0,3,2,1};
    for (int i = 0; i < int_vec.size(); i++)//利用size()成员函数遍历vector容器
    {//size():返回容器元素个数
        cout<<int_vec[i]<<" ";//像普通数组一样访问容器元素
    }
    cout<<endl;
    
    cout<<"push a 25 at last!"<<endl;
    int_vec.push_back(25);//push_back():在序列的尾部添加一个元素
    //也可以使用emplace_back()成员函数，与push_back作用一样，但前者更快。
    vector<int>::iterator iter;
    for (auto iter = int_vec.begin(); iter != int_vec.end(); iter++)//通过begin()和end()成员函数遍历vector容器
    {//begin():返回指向容器中第一个元素的迭代器
     //end():返回指向容器最后一个元素所在位置后一个位置的迭代器
        cout<<*iter<<" ";
    }
    cout<<endl;
    cout<<"pop 3 elem at last"<<endl;
    for (int i = 0; i < 3; i++)
    {
        cout<<"the elem which is poped is :"<<int_vec.back()<<endl;
        //back():返回最后一个元素的引用
        int_vec.pop_back();
        //pop_back():移出序列尾部的元素
    }
    cout<<"now the vector's size is : "<<int_vec.size()<<endl;
    cout<<"we change the size to nowsize * 2"<<endl;
    int_vec.resize(int_vec.size()*2);//resize():改变实际元素的个数。
    int * ptr = int_vec.data();//data():返回指向容器中第一个元素的指针。
    for (int i = 0; i < int_vec.size(); i=i+2)
    {
        cout<<*(ptr+i)<<" ";
    }
    cout<<endl;
    vector<int> temp{1,1,0};
    int_vec.swap(temp);//swap():交换两个容器的所有元素
    for (int i = 0; i < int_vec.size(); i++)
    {
        cout<<int_vec.at(i)<<" ";//at():使用经过边界检查的索引访问元素。越界会抛出异常哦~
    }
    cout<<endl;
    getchar();
    return 0; 
}
```

输出：

```
1 9 8 8 0 3 2 1
push a 25 at last!
1 9 8 8 0 3 2 1 25
pop 3 elem at last
the elem which is poped is :25
the elem which is poped is :1
the elem which is poped is :2
now the vector's size is : 6
we change the size to nowsize * 2
1 8 0 0 0 0
1 1 0
```

`vector<T>`的常用成员函数及其作用已经在上述程序注释。我们可以发现，其中大部分内容包括了`array<T,N>`中的成员函数，且作用相似，如 begin() 指向首元素，end() 指向尾元素后面的一个位置。

**注意事项**：

- vector容器包括了成员函数 swap() 之外，还有一个 std::swap(x,y) 非成员函数，但注意两者必须是存储相同类型的元素。

- vector容器的 swap() 成员函数交换了两个容器内的数据，也改变了其 size 大小。

- vector容器可以初始化为空的容器，因为其长度可变，但是在初始化空的vector容器时，不能使用迭代器，因为对于空的 vector 容器来说，其 begin() 和 end() 都指向的是同一个位置。此时如果想初始化，可以通过调用push_back() 或 resize() 函数来初始化。

  ```c++
  #include<iostream>
  #include<vector>
  using namespace std;
  int main(){
      vector<int> val;
  
      for(auto iter = val.begin(); iter != val.end(); iter++)
      {//没有输出
          *iter = 1;
          cout<<*iter<<" ";
      }
      cout<<"**************"<<endl;
      val.resize(10);//改变vector容器大小
      for(auto iter = val.begin(); iter != val.end(); iter++)
      {
          *iter = 1;
          cout<<*iter<<" ";
      }
      getchar();
      return 0;
  }
  ```

  输出：

  ```
  **************
  1 1 1 1 1 1 1 1 1 1
  ```

- 如果想增加容器的容量，还可以使用成员函数 reserve() 达成目的，但是申请了更多内存的同时，容器中元素的存放地址会发生改变（可以说是找了一块儿新的内存存储区来存储）

  ```c++
  #include<iostream>
  #include<vector>
  using namespace std;
  int main(){
      vector<int> val{1,2,4,5};
      cout<<"old address : "<<val.data()<<endl;
      val.reserve(10);
      cout<<"new address : "<<val.data()<<endl;
      for (auto i = val.begin(); i != val.end(); i++)
      {
          cout<<*i<<" ";
      }
      cout<<endl;
      cout<<"the val's size is : "<<val.size()<<endl;
  
      vector<int> temp{1,2,3,4};
      temp.resize(10);
      for (auto iter = temp.begin(); iter != temp.end(); iter++)
      {
          cout<<*iter<<" ";
      }
      cout<<endl;
      cout<<"the temp's size is : "<<temp.size()<<endl;
      getchar();
      return 0;
  }
  ```

  输出：

  ```
  old address : 0xef4320
  new address : 0xef4360
  1 2 4 5
  the val's size is : 4
  1 2 3 4 0 0 0 0 0 0
  the temp's size is : 10
  ```

  可见调用 reserve() 后改变了地址，并且遍历后只显示了4个元素，这是因为 reserve() 开辟出来的空间并不代表就都是有效空间，只有 size() 的大小才是有效空间。

  *注意：如果你调用 reserve(n) ，其中 n 小于原本的容量，则这个函数什么作用都没有。*

  具体的 reserve() 和 resize() 之间的区别可以看看这几篇文章` https://blog.csdn.net/jacky_789/article/details/90578809`

  `http://c.biancheng.net/view/6770.html`

##### 插入操作 insert() 和 emplace() 详解

**insert() 函数**的功能是在 vector 容器的指定位置插入一个或多个元素。

具体用法 (4中插入方法) 如下：

```c++
#include<iostream>
#include<vector>

using namespace std;

int main()
{
    vector<int> vec{1,2,3,4,5,6};
    vector<int>::iterator iter;
    vec.insert(vec.begin()+2,10);//在第begin()+2 (3)个位置前插入int型元素 10
    for (auto&& val : vec)
    {
        cout<<val<<" ";
    }
    cout<<endl;
    vec.insert(vec.end()-1,3,9);//在最后一个元素之前插入 3 个int型元素 9
    for (auto&& val : vec)
    {
        cout<<val<<" ";
    }
    cout<<endl;
    vec.insert(vec.begin()+3,{10,9,8,7});//在第4个元素之前插入列表{10,9,8,7}
    for (auto&& val : vec)
    {
        cout<<val<<" ";
    }
    cout<<endl;   
    vector<int> add{9,9,6};
    vec.insert(vec.end(),add.begin(),add.end());//在最后插入整个vector容器add
    for (auto&& val : vec)
    {
        cout<<val<<" ";
    }
    cout<<endl;
    getchar();
    return 0;
}
```

输出：

```
1 2 10 3 4 5 6
1 2 10 3 4 5 9 9 9 6
1 2 10 10 9 8 7 3 4 5 9 9 9 6
1 2 10 10 9 8 7 3 4 5 9 9 9 6 9 9 6
```

emplace() 函数也是在指定位置前插入元素，不过每次只能插入一个元素，而不是多个。

当然其也比 insert() 函数效率高一些。

```c++
#include<iostream>
#include<vector>
using namespace std;
int main()
{
    vector<int> v{1,2,3};
    v.emplace(v.begin(),0);
    for(auto && val : v)
    {
        cout<<val<<" ";
    }
    cout<<endl;
    getchar();
    return 0;
}
```

输出：

```
0 1 2 3
```

##### 删除操作pop_back()、erase()、remove()和clear() 详解

还是先上代码看看：

```c++
#include<iostream>
#include<vector>
#include<algorithm>
using namespace std;

int main()
{
    vector<int> v{1,9,9,9,0,6,0,2};
    cout<<"test pop_back()"<<endl;
    cout<<"size is : "<<v.size()<<endl;
    cout<<"capacity is : "<<v.capacity()<<endl;
    cout<<"pop last one : "<<v.back()<<endl;
    v.pop_back();
    cout<<"size is : "<<v.size()<<endl;
    cout<<"capacity is : "<<v.capacity()<<endl<<endl;
    //此时v{1,9,9,9,0,6,0}
    cout<<"test erase()"<<endl;
    auto iter = v.erase(v.begin()+2);//删除指定位置元素，返回一个指向删除元素所在位置下一个位置的迭代器。
    for(auto i = iter; i != v.end(); i++ )//从迭代器后开始遍历
    {
        cout<<*i<<" ";
    }
    cout<<endl;
    cout<<"size is : "<<v.size()<<endl;
    cout<<"capacity is : "<<v.capacity()<<endl<<endl;
    //此时v{1,9,9,0,6,0}  
    cout<<"test another erase()"<<endl;
    iter = v.erase(v.begin(),v.begin()+3);//删除[first，last)区域的元素，并返回指向此区域之后一个位置的迭代器
    for(auto i = iter; i != v.end(); i++ )
    {
        cout<<*i<<" ";
    }
    cout<<endl;
    cout<<"size is : "<<v.size()<<endl;
    cout<<"capacity is : "<<v.capacity()<<endl<<endl;
    //此时v{0,6,0}    
    cout<<"test remove()"<<endl;
    iter = remove(v.begin(),v.end(),0);//删除指定区域内元素为 0 的所有元素
    for(auto i = v.begin();i != iter;i++)
    {
        cout<<*i<<" ";
    }
    cout<<endl;
    for(auto i = v.begin(); i != v.end() ; i++)
    {
        cout<<*i<<" ";
    }
    cout<<endl;
    cout<<"size is : "<<v.size()<<endl;
    cout<<"capacity is : "<<v.capacity()<<endl<<endl;
    //此时v{6,6,0}
    cout<<"test clear()"<<endl;
    v.clear();//删除容器中所有的元素
    cout<<endl;
    cout<<"size is : "<<v.size()<<endl;
    cout<<"capacity is : "<<v.capacity()<<endl<<endl;

    getchar();
    return 0;
}
```

输出：

```
test pop_back()
size is : 8
capacity is : 8
pop last one : 2
size is : 7
capacity is : 8

test erase()
9 0 6 0
size is : 6
capacity is : 8

test another erase()
0 6 0
size is : 3
capacity is : 8

test remove()
6
6 6 0
size is : 3
capacity is : 8

test clear()

size is : 0
capacity is : 8
```

pop_back()，函数用法很简单，即删除 vector 容器中最后一个元素，该容器的大小（size）会减 1，但容量不会改变。

erase(pos)，删除 vector 容器中 pos 迭代器指定位置处的元素，并返回指向被删除元素下一个位置元素的迭代器。该容器的大小（size）会减 1，但容量（capacity）不会发生改变。

erase(pos1,pos2)，删除 vector 容器中 [pos1,pos2) 的元素，并返回指向被删除区域下一个位置元素的迭代器。该容器的大小（size）会减 pos2-pos1，但容量（capacity）不会发生改变。

remove()，删除容器中所有和指定元素值相等的元素，并返回指向最后一个元素下一个位置的迭代器。值得一提的是，调用该函数不会改变容器的大小和容量。

- **注意**：我们看输出可以发现通过remove() 删除元素后，大小没变，且对容器进行遍历后输出为 `6 6 0` ，这是为什么呢？这是因为remove() 的实现原理是，在遍历容器中的元素时，一旦遇到目标元素，就做上标记，然后继续遍历，直到找到一个非目标元素，即用此元素将最先做标记的位置覆盖掉(即用 第二个6 覆盖了最初的 0，但原来的 6 没有背覆盖，最后一个 0因为后面没有元素了也不会背覆盖），同时将此非目标元素所在的位置也做上标记，等待找到新的非目标元素将其覆盖。

clear()，删除 vector 容器中所有的元素，使其变成空的 vector 容器。该函数会改变 vector 的大小（变为 0），但不是改变其容量。

### deque< T> 双端队列容器

头文件：`#include<deque>`

**特点**：可以动态调整所占用的内存空间，擅长在 头部/尾部 插入/删除 元素。

#### 创建一个deque

```c++
std::deque<int> int_deque;//创建一个int类型的deque容器
std::deque<double> double_deque{0.2,4.2,3.99};//初始化时指定初始值及元素个数
std::deque<char> char_deque(5);//创建具有5个char类型元素的deque容器，默认值为0
std::deque<int> temp(10,2);//创建具有10个int类型元素且默认值为2的deque容器
std::deque<int> temp2(temp);//通过其他的容器来创建自己的deque容器
```

#### deque成员函数

deque< T> 成员函数大部分与array<T,N> 和 vector< T> 成员函数相同且作用类似。但是 deque 支持在容器头部添加和删除元素，同时其删除了 capacity()、reserve() 和 data() 成员函数。

```c++
#include<iostream>
#include<deque>
using namespace std;

int main()
{
    std::deque<int> int_deque;
    for (int i = 0; i < 3; i++)
    {
        int_deque.push_front(i);
    }
    for (int i = 0; i < 3; i++)
    {
        int_deque.push_back(i);
    }   
    auto iter = int_deque.begin();
    for (auto i = iter; i != int_deque.end(); i++)
    {
        cout<<*i<<" ";
    }
    cout<<endl;
    for (int i = 0; i < 4; i++)
    {
        int_deque.pop_front();
    }
    for (auto i = int_deque.begin(); i != int_deque.end(); i++)
    {
        cout<<*i<<" ";
    }
    cout<<endl;
    getchar();
    return 0; 
}
```

输出：

```
2 1 0 0 1 2
1 2
```

deque 相比 vector 比较独特的两个成员函数 push_front() 和 pop_front() 。

push_front() ：在容器现有元素的头部添加一个元素 。 emplace_front() 和其相似，但效率更好些。

pop_front() ： 移除容器尾部的一个元素 。

其余成员函数和之前类似，就不细说了~，可以看下表。

| 成员函数        | 功能                                                         |
| --------------- | ------------------------------------------------------------ |
| push_back()     | 在容器现有元素的尾部添加一个元素，和 emplace_back() 不同，该函数添加新元素的过程是，先构造元素，然后再将该元素移动或复制到容器的尾部。 |
| pop_back()      | 移除容器尾部的一个元素。                                     |
| emplace_back()  | 在容器尾部生成一个元素。和 push_back() 不同，该函数直接在容器头部构造元素，省去了复制或移动元素的过程。 |
| emplace_front() | 在容器头部生成一个元素。和 push_front() 不同，该函数直接在容器头部构造元素，省去了复制或移动元素的过程。 |
| insert()        | 在指定的位置直接生成一个元素。和 emplace() 不同的是，该函数添加新元素的过程是，先构造元素，然后再将该元素移动或复制到容器的指定位置。***注意：插入方式和 vector 类似，有4种*** |
| emplace()       | 在指定的位置直接生成一个元素。和 insert() 不同的是，emplace() 直接在容器指定位置构造元素，省去了复制或移动元素的过程。 |
| erase()         | 移除一个元素或某一区域内的多个元素。                         |

### list< T> 双向链表容器

头文件：`#include<list>`

**特点**：list 容器中的元素可以分散存储在内存空间里，可以动态调整所占用的内存空间，以在序列已知的任何位置快速插入或删除元素。

#### 创建一个list

简单介绍下 list 创建的五种方法：

首先是和 vector 很类似的几种创建方法：

```c++
std::list<int> int_list;//创建一个int类型的list容器
std::list<double> double_list{0.2,4.2,3.99};//初始化时指定初始值及元素个数
std::list<char> char_liste(5);//创建具有5个char类型元素的list容器，默认值为0
std::list<int> temp(10,2);//创建具有10个int类型元素且默认值为2的list容器
std::list<int> temp2(temp);//通过其他的容器来创建自己的list容器
```

也可以通过下面两种方法创建：

```c++
#include<iostream>
#include<list>
#include<array>
using namespace std;

int main()
{
    //拷贝其它类型的容器，创建 list 容器
    array<int,5> a{1,2,3,4,5};
    list<int> ls1(a.begin()+1,a.end());
    for (auto && val : ls1)
    {
        cout<<val<<" ";
    }
    cout<<endl;
    //拷贝普通数组，创建list容器
    int arr[] = {9,8,7,6,5};
    list<int> ls2(arr,arr+4);
    for (auto && val : ls2)
    {
        cout<<val<<" ";
    }
    cout<<endl;
    getchar();
    return 0;
}
```

输出：

```
2 3 4 5
9 8 7 6
```

#### list成员函数

首先list 的 begin()/end()、rbegin()/rend()、cbegin()/cend()、crbegin()/crend() 成员函数是和 array , vector , deque 相同的用法。

注意：list 容器迭代器与vector、array、deque最大的不同在于，其配备的迭代器类型为双向迭代器，而不再是随机访问迭代器。

因此，假设 p1 和 p2 都是双向迭代器，则它们支持使用 ++p1、 p1++、 p1--、 p1++、 *p1、 p1==p2 以及 p1!=p2 运算符，但不支持以下操作（其中 i 为整数）：

- p1[i]：不能通过下标访问 list 容器中指定位置处的元素。
- p1-=i、 p1+=i、 p1+i 、p1-i：双向迭代器 p1 不支持使用 -=、+=、+、- 运算符。
- p1<p2、 p1>p2、 p1<=p2、 p1>=p2：双向迭代器 p1、p2 不支持使用 <、 >、 <=、 >= 比较运算符。

##### 插入操作 emplace()、emplace_front()、emplace_back()、insert()和splice()

```c++
#include<iostream>
#include<list>
#include<array>
using namespace std;

int main()
{
    list<int> ls{1,2,3,4,5,6,7,8,9,10};
    cout<<"first is : "<<ls.front()<<endl;
    cout<<"last is : "<<ls.back()<<endl;
    cout<<"add in last :"<<6<<endl;
    ls.emplace(ls.end(),6);
    cout<<"add in front :"<<100<<endl;
    ls.emplace(ls.begin(),100);
    for (auto && val : ls)
    {
        cout<<val<<" ";
    }
    cout<<endl;    
    list<int> temp{1,11,111,1111};
    auto start = temp.begin();
    auto end = temp.end();
    ls.splice(++ls.begin(),temp);
    for (auto && val : ls)
    {
        cout<<val<<" ";
    }
    cout<<endl;
    cout<<"temp size is: "<<temp.size()<<endl;
    cout<<"*********"<<endl;
    list<int> ano{9,99,999};
    ls.splice(ls.begin(),ano,ano.begin(),ano.end());
    for (auto i = ls.begin(); i != ls.end(); i++)
    {
        cout<<*i<<" ";
    }
    cout<<endl;
    getchar();
    return 0;
}
```

输出：

```
first is : 1
last is : 10
add in last :6
add in front :100
100 1 2 3 4 5 6 7 8 9 10 6
100 1 11 111 1111 1 2 3 4 5 6 7 8 9 10 6
temp size is: 0
*********
9 99 999 100 1 11 111 1111 1 2 3 4 5 6 7 8 9 10 6
```

***注意：一旦调用成员函数 splice() 将 list1 中的元素移动至 list2 ，则 list1 中将不再存在这些元素。***

##### 删除操作 pop_front()、pop_back()、erase()、remove()、unique()和 remove_if()

pop_front()、pop_back() 操作:

```c++
#include<iostream>
#include<list>
using namespace std;
int main()
{
    list<int> ls{1,9,9,9,0,1,0,1};
    for (auto i = ls.begin(); i != ls.end(); i++)
    {
        cout<<*i<<" ";
    }
    cout<<endl;
    cout<<"size is : "<<ls.size()<<endl;
    cout<<"pop one from front.\n";
    ls.pop_front();
    ls.pop_back();
    cout<<"pop one from last.\n";
    for (auto i = ls.begin(); i != ls.end(); i++)
    {
        cout<<*i<<" ";
    }
    cout<<endl;
    cout<<"now size is : "<<ls.size()<<endl;
    getchar();
    return 0;
}
```

输出：

```
1 9 9 9 0 1 0 1
size is : 8
pop one from front.
pop one from last.
9 9 9 0 1 0
now size is : 6
```

可见 pop_front() 是删除位于 list 容器头部的一个元素。pop_back() 是删除位于 list 容器尾部的一个元素。两者均会将容器大小（size）减一。

erase() 操作：

```c++
#include<iostream>
#include<list>

using namespace std;

int main()
{
    list<int> ls{1,9,9,9,0,1,0,1};
    for (auto i = ls.begin(); i != ls.end(); i++)
    {
        cout<<*i<<" ";
    }
    cout<<endl;
    cout<<"size is : "<<ls.size()<<endl;
    cout<<"erase one from pos 5.\n";
    auto iter = ls.begin();
    for (int i = 0; i < 4; i++)
    {
        iter++;
    }
    ls.erase(iter);
    for (auto i = ls.begin(); i != ls.end(); i++)
    {
        cout<<*i<<" ";
    }
    cout<<endl;
    cout<<"now size is : "<<ls.size()<<endl;

    cout<<"erase datas from pos 1 to pos 3.\n";
    auto begin = ls.begin();
    auto end = begin;
    for (int i = 0; i < 3; i++)
    {
        end++;
    }
    ls.erase(begin,end);
    for (auto i = ls.begin(); i != ls.end(); i++)
    {
        cout<<*i<<" ";
    }
    cout<<endl;
    cout<<"now size is : "<<ls.size()<<endl;    
    getchar();
    return 0;
}
```

输出：

```
1 9 9 9 0 1 0 1
size is : 8
erase one from pos 5.
1 9 9 9 1 0 1
now size is : 7
erase datas from pos 1 to pos 3.
9 1 0 1
now size is : 4
```

由上可知，erase() 成员函数有两种使用语法。分别是：

| 函数原型                                        | 功能                                                         |
| ----------------------------------------------- | ------------------------------------------------------------ |
| iterator erase (iterator position);             | 删除 list 容器中 position 迭代器所指位置处的元素             |
| iterator erase (iterator first, iterator last); | 删除 list 容器中 first 迭代器和 last 迭代器限定区域内的所有元素（包括 first 指向的元素，但不包括 last 指向的元素） |

remove() 操作：

```c++
#include<iostream>
#include<list>
using namespace std;
int main()
{
    list<int> ls{1,9,9,9,0,1,0,1};
    for (auto i = ls.begin(); i != ls.end(); i++)
    {
        cout<<*i<<" ";
    }
    cout<<endl;
    cout<<"size is : "<<ls.size()<<endl;
    cout<<"remove num 1\n";
    ls.remove(1);
    for (auto i = ls.begin(); i != ls.end(); i++)
    {
        cout<<*i<<" ";
    }
    cout<<endl;
    cout<<"now size is : "<<ls.size()<<endl;    
    getchar();
    return 0;
}
```

输出：

```
 9 9 9 0 1 0 1
size is : 8
remove num 1
9 9 9 0 0
now size is : 5
```

所以如果想根据元素的值来执行删除操作，可以使用 remove() 成员函数。

unique() 操作：

```c++
#include<iostream>
#include<list>
using namespace std;
bool unique_demo(int a,int b)
{
    return (a + b) % 2 == 0;
}
int main()
{
    list<int> ls{1,9,9,9,0,1,0,1};
    for (auto i = ls.begin(); i != ls.end(); i++)
    {
        cout<<*i<<" ";
    }
    cout<<endl;
    cout<<"size is : "<<ls.size()<<endl;
    cout<<"after unique()\n";
    ls.unique();
    for (auto i = ls.begin(); i != ls.end(); i++)
    {
        cout<<*i<<" ";
    }
    cout<<endl;
    cout<<"now size is : "<<ls.size()<<endl;    

    ls.unique(unique_demo);
    for (auto i = ls.begin(); i != ls.end(); i++)
    {
        cout<<*i<<" ";
    }
    cout<<endl;
    cout<<"now size is : "<<ls.size()<<endl;      
    getchar();
    return 0;
}
```

输出：

```
1 9 9 9 0 1 0 1
size is : 8
after unique()
1 9 0 1 0 1
now size is : 6
1 0 1 0 1
now size is : 5
```

unique() 函数也有以下 2 种语法格式：

| 函数原型                                             | 功能                                     |
| ---------------------------------------------------- | ---------------------------------------- |
| void unique()                                        | 去除 list 容器中相邻重复的元素           |
| void unique（BinaryPredicate）//传入一个二元谓词函数 | 根据二元谓词函数的规则去除符合要求的元素 |

可以看到我们在上面定义了一个二元谓词函数 bool unique_demo(int a,int b) ，通过将自定义的谓词函数传给 unique() 成员函数，list 容器中能使谓词函数成立的元素都会被删除（即前后两个元素 a,b 相加为偶数的话，去除第二个元素 b ）。

remove_if() 操作：

```c++
#include<iostream>
#include<list>
using namespace std;
int main()
{
    list<int> ls{2,4,6,8,72,12,4,65};
    ls.remove_if([](int value){return value >50;});
    for (auto i = ls.begin(); i != ls.end(); i++)
    {
        cout<<*i<<" ";
    }
    cout<<endl;
    cout<<"now size is : "<<ls.size()<<endl;  
    getchar();
    return 0;
}
```

输出：

```
2 4 6 8 12 4
now size is : 6
```

通过将自定义的谓词函数（不限定参数个数）传给 remove_if() 成员函数，list 容器中能使谓词函数成立的元素都会被删除。

### forward_list< T>单向链表容器

头文件：`#include<forward_list>`

**特点**：forward_list 容器中的元素可以分散存储在内存空间里，可以动态调整所占用的内存空间且耗用的内存空间相比 list 更少，空间利用率更高，以在序列已知的任何位置快速插入或删除元素。 forward_list 容器只提供前向迭代器，而不是双向迭代器。

*注意：效率高是选用 forward_list 而弃用 list 容器最主要的原因，换句话说，只要是 list 容器和 forward_list 容器都能实现的操作，应优先选择 forward_list 容器。*

#### 创建一个forward_list

```
std::forward_list<int> int_forward_list;//创建一个int类型的forward_list容器
std::forward_list<double> double_forward_list{0.2,4.2,3.99};//初始化时指定初始值及元素个数
std::lisforward_listt<char> char_forward_list(5);//创建具有5个char类型元素的forward_list容器，默认值为0
std::forward_list<int> temp(10,2);//创建具有10个int类型元素且默认值为2的forward_list容器
std::forward_list<int> temp2(temp);//通过其他的容器来创建自己的forward_list容器
```

#### forward_list成员函数

```c++
#include<iostream>
#include<forward_list>
using namespace std;

int main()
{
    forward_list<int> fls{1,2,4,5,7,9,3,0,15};
    cout<<"the forward_list length is : "<<distance(fls.begin(),fls.end())<<endl;
    int len = distance(fls.begin(),fls.end());
    for (auto i = fls.begin(); i != fls.end() ; i++)
    {
        cout<<*i<<" ";
    }
    cout<<endl;
    fls.sort();
    for (auto i = fls.begin(); i != fls.end() ; i++)
    {
        cout<<*i<<" ";
    }
    cout<<endl;
    fls.reverse();
    for (auto i = fls.begin(); i != fls.end() ; i++)
    {
        cout<<*i<<" ";
    }
    cout<<endl;
    getchar();
    return 0;
}
```

输出：

```
the forward_list length is : 9
1 2 4 5 7 9 3 0 15
0 1 2 3 4 5 7 9 15
15 9 7 5 4 3 2 1 0
```

上面只是举出了几个成员函数的使用例子，其余成语函数用法和前面的容器的成员函数用法类似。

*注意！forward_list 容器中是不提供 size() 函数的，但如果想要获取 forward_list 容器中存储元素的个数，可以使用头文件 `<iterator> `中的 distance() 函数。*

| 成员函数        | 功能                                                         |
| --------------- | ------------------------------------------------------------ |
| before_begin()  | 返回一个前向迭代器，其指向容器中第一个元素之前的位置。       |
| begin()         | 返回一个前向迭代器，其指向容器中第一个元素的位置。           |
| end()           | 返回一个前向迭代器，其指向容器中最后一个元素之后的位置。     |
| cbefore_begin() | 和 before_begin() 功能相同，只不过在其基础上，增加了 const 属性，不能用于修改元素。 |
| cbegin()        | 和 begin() 功能相同，只不过在其基础上，增加了 const 属性，不能用于修改元素。 |
| cend()          | 和 end() 功能相同，只不过在其基础上，增加了 const 属性，不能用于修改元素。 |
| empty()         | 判断容器中是否有元素，若无元素，则返回 true；反之，返回 false。 |
| max_size()      | 返回容器所能包含元素个数的最大值。这通常是一个很大的值，一般是 232-1，所以我们很少会用到这个函数。 |
| front()         | 返回第一个元素的引用。                                       |
| assign()        | 用新元素替换容器中原有内容。                                 |
| push_front()    | 在容器头部插入一个元素。                                     |
| emplace_front() | 在容器头部生成一个元素。该函数和 push_front() 的功能相同，但效率更高。 |
| pop_front()     | 删除容器头部的一个元素。                                     |
| emplace_after() | 在指定位置之后插入一个新元素，并返回一个指向新元素的迭代器。和 insert_after() 的功能相同，但效率更高。 |
| insert_after()  | 在指定位置之后插入一个新元素，并返回一个指向新元素的迭代器。 |
| erase_after()   | 删除容器中某个指定位置或区域内的所有元素。                   |
| swap()          | 交换两个容器中的元素，必须保证这两个容器中存储的元素类型是相同的。 |
| resize()        | 调整容器的大小。                                             |
| clear()         | 删除容器存储的所有元素。                                     |
| splice_after()  | 将某个 forward_list 容器中指定位置或区域内的元素插入到另一个容器的指定位置之后。 |
| remove(val)     | 删除容器中所有等于 val 的元素。                              |
| remove_if()     | 删除容器中满足条件的元素。                                   |
| unique()        | 删除容器中相邻的重复元素，只保留一个。                       |
| merge()         | 合并两个事先已排好序的 forward_list 容器，并且合并之后的 forward_list 容器依然是有序的。 |
| sort()          | 通过更改容器中元素的位置，将它们进行排序。                   |
| reverse()       | 反转容器中元素的顺序。                                       |

