## STL泛型编程笔记



STL六大部件：

- 容器 Containers : 数据存储在容器中
- 分配器 Allocators：对容器进行内存管理
- 算法 Algorithms：操作容器的方法，通过迭代器去操作数据
- 迭代器 Ilterator： 一种泛化的指针
- 适配器 Adapters：进行一些转换
- 仿函数 Functors

<img src="/Users/ywh/Documents/FindGoodJob/cpp_road-master/STL泛型编程笔记.assets/image-20221112193541058-8252946.png" alt="image-20221112193541058" style="zoom:50%;" />



在程序中要用容器，不要直接用分配器，因为麻烦。

GP：Geneic Programming，将数据和方法分开，使得算法和容器可以各自开发。算法通过迭代器确定操作范围，通过迭代器取用容器中的元素。

在STL中，操作符重载占据了非常重要的角色。



为什么List不能使用sort排序？

因为不是随机访问迭代器



所有algorithm,其内部最终涉及元素本身的操作，无非就是比大小。