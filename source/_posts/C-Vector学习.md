---
title: C++Vector学习
tags: C++
categories: C plus plus
abbrlink: 26dc6703
date: 2019-07-07 23:26:42
---
Vector详解
<!--more-->
网上借鉴的：
c++中vector的用法详解
　　vector(向量): C++中的一种数据结构,确切的说是一个类.它相当于一个动态的数组,当程序员无法知道自己需要的数组的规模多大时,用其来解决问题可以达到最大节约空间的目的.
     用法:

        1.文件包含:     
             首先在程序开头处加上#include<vector>以包含所需要的类文件vector
             还有一定要加上using namespace std;

        2.变量声明:
            2.1 例:声明一个int向量以替代一维的数组:vector <int> a;(等于声明了一个int数组a[],大小没有指定,可以动态的向里面添加删除)。
            2.2 例:用vector代替二维数组.其实只要声明一个一维数组向量即可,而一个数组的名字其实代表的是它的首地址,所以只要声明一个地址的向量即可,即:vector <int *> a.同理想用向量代替三维数组也是一样,vector <int**>a;再往上面依此类推.

        3.具体的用法以及函数调用:
          3.1 如何得到向量中的元素?其用法和数组一样:

          例如:
           vector <int *> a
           int b = 5;
           a.push_back(b);//该函数下面有详解
           cout<<a[0];       //输出结果为5
        vector                 // 创建一个空的vector。
        vector a1(a2)          // 复制一个vector
        vector a(n)            // 创建一个vector，含有n个数据，数据均已缺省构造产生
        vector a(n, elem)      // 创建一个含有n个elem拷贝的vector
        vector a(beg,end)      // 创建一个含有n个elem拷贝的vector
        a.assign(beg,end)      //将[beg; end)区间中的数据赋值给a
        a.assign(n,elem)          //将n个elem的拷贝赋值给a。
        
        1. push_back()   在数组的最后添加一个数据
        2. pop_back()    去掉数组的最后一个数据 
        3. at()                得到编号位置的数据
        4. begin()           得到数组头的指针
        5. end()             得到数组的最后一个单元+1的指针
        6. front()            得到数组头的引用
        7. back()            得到数组的最后一个单元的引用
        8. max_size()     得到vector最大可以是多大
        9. capacity()       当前vector分配的大小
        10. size()           当前使用数据的大小
        11. resize()         改变当前使用数据的大小，如果它比当前使用的大，者填充默认值
        12. reserve()      改变当前vecotr所分配空间的大小
        13. erase()         删除指针指向的数据项
        14. clear()          清空当前的vector
        15. rbegin()        将vector反转后的开始指针返回(其实就是原来的end-1)
        16. rend()          将vector反转构的结束指针返回(其实就是原来的begin-1)
        17. empty()        判断vector是否为空
        18. swap(a1,a2)         将c1和c2元素互换
        19. c1.swap(c2)            将c1和c2元素互换

      3.2  详细的函数实现功能：其中vector<int> c.

         1. c.clear()         移除容器中所有数据。
         2. c.empty()         判断容器是否为空。
         3. c.erase(pos)        删除pos位置的数据
         4. c.erase(beg,end) 删除[beg,end)区间的数据
         5. c.front()         传回第一个数据。
         6. c.insert(pos,elem)  在pos位置插入一个elem拷贝
         7. c.pop_back()     删除最后一个数据。
         8. c.push_back(elem) 在尾部加入一个数据。
         9. c.resize(num)     重新设置该容器的大小
         10. c.size()         回容器中实际数据的个数。
         11. c.begin()           返回指向容器第一个元素的迭代器
         12. c.end()             返回指向容器最后一个元素的迭代器

    4.内存管理与效率

      1》使用reserve()函数提前设定容量大小，避免多次容量扩充操作导致效率低下。

         关于STL容器，最令人称赞的特性之一就是是只要不超过它们的最大大小，它们就可以自动增长到足以容纳你放进去的数据。（要知道这个最大值，只要调用名叫max_size的成员函数。）对于vector 和string，如果需要更多空间，就以类似realloc的思想来增长大小。vector容器支持随机访问，因此为了提高效率，它内部使用动态数组的方式实现的。在通过 reserve() 来申请特定大小的时候总是按指数边界来增大其内部缓冲区。当进行insert或push_back等增加元素的操作时，如果此时动态数组的内存不够用，就要动态的重新分配当前大小的1.5~2倍的新内存区，再把原数组的内容复制过去。所以，在一般情况下，其访问速度同一般数组，只有在重新分配发生时，其性能才会下降。正如上面的代码告诉你的那样。而进行pop_back操作时，capacity并不会因为vector容器里的元素减少而有所下降，还会维持操作之前的大小。对于vector容器来说，如果有大量的数据需要进行push_back，应当使用reserve()函数提前设定其容量大小，否则会出现许多次容量扩充操作，导致效率低下。

      reserve成员函数允许你最小化必须进行的重新分配的次数，因而可以避免真分配的开销和迭代器/指针/引用失效。但在我解释reserve为什么可以那么做之前，让我简要介绍有时候令人困惑的四个相关成员函数。在标准容器中，只有vector和string提供了所有这些函数。
        (1) size()告诉你容器中有多少元素。它没有告诉你容器为它容纳的元素分配了多少内存。 
        (2) capacity()告诉你容器在它已经分配的内存中可以容纳多少元素。那是容器在那块内存中总共可以容纳多少元素，而不是还可以容纳多少元素。如果你想知道一个vector或string中有多少没有被占     用的内存，你必须从capacity()中减去size()。如果size和capacity返回同样的值，容器中就没有剩余空间了，而下一次插入（通过insert或push_back等）会引发上面的重新分配步骤。
        (3) resize(Container::size_type n)强制把容器改为容纳n个元素。调用resize之后，size将会返回n。如果n小于当前大小，容器尾部的元素会被销毁。如果n大于当前大小，新默认构造的元素会添    加到容器尾部。如果n大于当前容量，在元素加入之前会发生重新分配。
        (4) reserve(Container::size_type n)强制容器把它的容量改为至少n，提供的n不小于当前大小。这一般强迫进行一次重新分配，因为容量需要增加。（如果n小于当前容量，vector忽略它，这个       调用什么都不做，string可能把它的容量减少为size()和n中大的数，但string的大小没有改变。在我的经验中，使用reserve来从一个string中修整多余容量一般不如使用“交换技巧”，那是条款17       的主题。）

     这个简介表示了只要有元素需要插入而且容器的容量不足时就会发生重新分配（包括它们维护的原始内存分配和回收，对象的拷贝和析构和迭代器、指针和引用的失效）。所以，避免重新分配的关键是使用reserve尽快把容器的容量设置为足够大，最好在容器被构造之后立刻进行。

     例如，假定你想建立一个容纳1-1000值的vector<int>。没有使用reserve，你可以像这样来做：

      vector<int> v;
      for (int i = 1; i <= 1000; ++i) v.push_back(i);
      在大多数STL实现中，这段代码在循环过程中将会导致2到10次重新分配。（10这个数没什么奇怪的。记住vector在重新分配发生时一般把容量翻倍，而1000约等于210。）

     把代码改为使用reserve，我们得到这个：
        vector<int> v;
        v.reserve(1000);
          for (int i = 1; i <= 1000; ++i) v.push_back(i);
        这在循环中不会发生重新分配。

        在大小和容量之间的关系让我们可以预言什么时候插入将引起vector或string执行重新分配，而且，可以预言什么时候插入会使指向容器中的迭代器、指针和引用失效。例如，给出这段代码，

            string s;
            ...
            if (s.size() < s.capacity()) 
            {
                s.push_back('x');
            }
        push_back的调用不会使指向这个string中的迭代器、指针或引用失效，因为string的容量保证大于它的大小。如果不是执行push_back，代码在string的任意位置进行一个insert，我们仍然可以保证在插入期间没有发生重新分配，但是，与伴随string插入时迭代器失效的一般规则一致，所有从插入位置到string结尾的迭代器/指针/引用将失效。

        回到本条款的主旨，通常有两情况使用reserve来避免不必要的重新分配。第一个可用的情况是当你确切或者大约知道有多少元素将最后出现在容器中。那样的话，就像上面的vector代码，你只是提前reserve适当数量的空间。第二种情况是保留你可能需要的最大的空间，然后，一旦你添加完全部数据，修整掉任何多余的容量。

       2》使用“交换技巧”来修整vector过剩空间/内存

      有一种方法来把它从曾经最大的容量减少到它现在需要的容量。这样减少容量的方法常常被称为“收缩到合适（shrink to fit）”。该方法只需一条语句：vector<int>(ivec).swap(ivec);
        表达式vector<int>(ivec)建立一个临时vector，它是ivec的一份拷贝：vector的拷贝构造函数做了这个工作。但是，vector的拷贝构造函数只分配拷贝的元素需要的内存，所以这个临时vector没有多余的容量。然后我们让临时vector和ivec交换数据，这时我们完成了，ivec只有临时变量的修整过的容量，而这个临时变量则持有了曾经在ivec中的没用到的过剩容量。在这里（这个语句结尾），临时vector被销毁，因此释放了以前ivec使用的内存，收缩到合适。

      3》用swap方法强行释放STL Vector所占内存

            template < class T> void ClearVector( vector<T>& v )
            { 
                vector<T>vtTemp;
                vtTemp.swap( v );
            } 
            如 
                vector<int> v ;
                nums.push_back(1);
                nums.push_back(3);
                nums.push_back(2);
                nums.push_back(4);
                vector<int>().swap(v);
            
            /* 或者v.swap(vector<int>()); */
            
            /*或者{ std::vector<int> tmp = v;   v.swap(tmp);   }; //加大括号{ }是让tmp退出{ }时自动析构*/

 

    5.Vector 内存管理成员函数的行为测试

       C++ STL的vector使用非常广泛，但是对其内存的管理模型一直有多种猜测，下面用实例代码测试来了解其内存管理方式，测试代码如下：
        #include <iostream>
        #include <vector>
        using namespace std;
        
        int main()
        {
            vector<int> iVec;
            cout << "容器 大小为: " << iVec.size() << endl;
            cout << "容器 容量为: " << iVec.capacity() << endl; //1个元素， 容器容量为1
            
            iVec.push_back(1);
            cout << "容器 大小为: " << iVec.size() << endl;
            cout << "容器 容量为: " << iVec.capacity() << endl; //2个元素， 容器容量为2
            
            iVec.push_back(2);
            cout << "容器 大小为: " << iVec.size() << endl;
            cout << "容器 容量为: " << iVec.capacity() << endl; //3个元素， 容器容量为4
            
            iVec.push_back(3);
            cout << "容器 大小为: " << iVec.size() << endl;
            cout << "容器 容量为: " << iVec.capacity() << endl; //4个元素， 容器容量为4
            
            iVec.push_back(4);
            iVec.push_back(5);
            cout << "容器 大小为: " << iVec.size() << endl;
            cout << "容器 容量为: " << iVec.capacity() << endl; //5个元素， 容器容量为8
            
            iVec.push_back(6);
            cout << "容器 大小为: " << iVec.size() << endl;
            cout << "容器 容量为: " << iVec.capacity() << endl; //6个元素， 容器容量为8
            
            iVec.push_back(7);
            cout << "容器 大小为: " << iVec.size() << endl;
            cout << "容器 容量为: " << iVec.capacity() << endl; //7个元素， 容器容量为8
            
            iVec.push_back(8);
            cout << "容器 大小为: " << iVec.size() << endl;
            cout << "容器 容量为: " << iVec.capacity() << endl; //8个元素， 容器容量为8
            
            iVec.push_back(9);
            cout << "容器 大小为: " << iVec.size() << endl;
            cout << "容器 容量为: " << iVec.capacity() << endl; //9个元素， 容器容量为16
            /* vs2005/8 容量增长不是翻倍的，如 
                9个元素   容量9 
                10个元素 容量13 */
            
            /* 测试effective stl中的特殊的交换 swap() */
            cout << "当前vector 的大小为: " << iVec.size() << endl;
            cout << "当前vector 的容量为: " << iVec.capacity() << endl;
            vector<int>(iVec).swap(iVec);
            
            cout << "临时的vector<int>对象 的大小为: " << (vector<int>(iVec)).size() << endl;
            cout << "临时的vector<int>对象 的容量为: " << (vector<int>(iVec)).capacity() << endl;
            cout << "交换后，当前vector 的大小为: " << iVec.size() << endl;
            cout << "交换后，当前vector 的容量为: " << iVec.capacity() << endl;
            
            return 0;
        }
        
    6.vector的其他成员函数    
            c.assign(beg,end)：将[beg; end)区间中的数据赋值给c。
            c.assign(n,elem)：将n个elem的拷贝赋值给c。 
            c.at(idx)：传回索引idx所指的数据，如果idx越界，抛出out_of_range。 
            c.back()：传回最后一个数据，不检查这个数据是否存在。
            c.front()：传回地一个数据。 
            get_allocator：使用构造函数返回一个拷贝。 
            c.rbegin()：传回一个逆向队列的第一个数据。 
            c.rend()：传回一个逆向队列的最后一个数据的下一个位置。 
            c.~ vector <Elem>()：销毁所有数据，释放内存。    

    7.备注:在用vector的过程中的一些问题,特此列出讨论:
           1)
                vector <int > a;
                int  b = 5;
                a.push_back(b);
                此时若对b另外赋值时不会影响a[0]的值

            2)
                vector <int*> a;
                 int *b;
                 b= new int[4];
                 b[0]=0;
                 b[1]=1;
                 b[2]=2;
                 a.push_back(b);
                 delete b;          //释放b的地址空间
                 for(int i=0 ; i <3 ; i++)
                 {
                       cout<<a[0][i]<<endl;
                 }

                 此时输出的值并不是一开始b数组初始化的值,而是一些无法预计的值.

                分析:根据1) 2)的结果,可以想到,在1)中,  往a向量中压入的是b的值,即a[0]=b,此时a[0]和b是存储在两个不同的地址中的.因此改变b的值不会影响
                a[0];而在2)中,因为是把一个地址(指针)压入向量a,即a[0]=b,因此释放了b的地址也就释放了a[0]的地址,因此a[0]数组中存放的数值也就不得而知了. 
### C++中的unique(),unique_copy()

unique 与 unique_copy 属于C++的STL模板，头文件是 algorithm
作用：

去除重复的 相邻的 重复的元素，即如果两个元素 相同 但是 不相邻，则不会去除

unique 将重复的元素没有删除，而是放在了最后面，故原来是多少元素，去重后还是多少元素，保存了剔除的元素

unique_copy 是将去重后的元素重新放到另外一个数组去，没有保存剔除的元素

故通常用前，先将数组排序
用法：

unique(begin, end)  ，begin 是开始位置，end 是结束的位置，返回一个地址，

unique_copy(begin, end, tar_begin), begin是原数组的开始地址，end原数组的结束地址，tar_begin是要复制到的数组的即目标数组的开始地址，同样返回一个地址
示例：
```
#include <cstdio>
#include <cstring>
#include <iostream>
#include <algorithm>
using namespace std;
typedef long long ll;
ll k, p;
 
int main () {
    int a[] = {1, 2, 2, 2, 3, 3, 4, 5, 5, 3};
    int b[] = {1, 2, 2, 2, 3, 3, 4, 5, 5, 3};
    int len = unique(a, a+10) - a;
    //len返回的是最后一个不重复数字的数组下标位置
    printf("unique\nlen = %d\n", len);
    for(int i = 0; i < 10; i++)
    {
        if(i == len) puts("##");
        printf("%d ", a[i]);
    }
    printf("\n下面是unique_copy\n");
    int c[10];
    int d = unique_copy(b, b+10, c) - c;
    printf("len = %d\n", d);
    for(int i = 0; i < 10; i++)
    {
        printf("%d ", c[i]);
    }
    printf("\n");
    return 0;
```
输出为：
```
unique
len = 5
1 2 3 4 5 3 ##  // 最后输出的3是因为和他相邻的没有3
4 5 5 3     // 说明没有删除，只是放在后面而已 

下面是unique_copy
len = 6
1 2 3 4 5 3 2004323541 667813577 -2 2004226402   //说明删除了，因为后面是随机数嘛
```