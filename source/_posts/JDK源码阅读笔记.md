---
title: JDK源码阅读笔记
tags: JDK
categories: 源代码
abbrlink: b6ab39d1
date: 2019-08-02 11:46:06
---
源代码阅读：基于JDK12.0。
<!--more-->
# ArrayList源码阅读
__ArrayList简介__
* ArrayList 是一个数组队列，相当于 动态数组。
* 与Java中的数组相比，它的容量能动态增长。它继承于AbstractList，实现了List, RandomAccess, Cloneable, java.io.Serializable这些接口。

* ArrayList 继承了AbstractList，实现了List。它是一个数组队列，提供了相关的添加、删除、修改、遍历等功能。

* ArrayList 实现了RandmoAccess接口，即提供了随机访问功能。RandmoAccess是java中用来被List实现，为List提供快速访问功能的。在ArrayList中，我们即可以通过元素的序号快速获取元素对象；这就是快速随机访问。稍后，我们会比较List的“快速随机访问”和“通过Iterator迭代器访问”的效率。

* ArrayList 实现了Cloneable接口，即覆盖了函数clone()，能被克隆。

* ArrayList 实现java.io.Serializable接口，这意味着ArrayList支持序列化，能通过序列化去传输* 。

* 和Vector不同，ArrayList中的操作不是线程安全的！所以，建议在单线程中才使用ArrayList，而在多线程中可以选择Vector或者CopyOnWriteArrayList。

### ArrayList属性

ArrayList属性主要就是当前数组长度size，以及存放数组的对象elementData数组，除此之外还有一个经常用到的属性就是从AbstractList继承过来的modCount属性，代表ArrayList集合的修改次数。
```
public class ArrayList<E> extends AbstractList<E>
        implements List<E>, RandomAccess, Cloneable, java.io.Serializable
{
	//序列化ID
    private static final long serialVersionUID = 8683452581122892189L;

    /**
     * Default initial capacity.
       默认初始容量=10
     */
    private static final int DEFAULT_CAPACITY = 10;

    /**
     * Shared empty array instance used for empty instances.
       用于空实例的共享空数组实例
     */
    private static final Object[] EMPTY_ELEMENTDATA = {};

    /**
      Shared empty array instance used for default sized empty instances. We
      distinguish this from EMPTY_ELEMENTDATA to know how much to inflate when
      first element is added.
      共享空数组实例，用于默认大小的空实例。
      我们将其与上面的EMPTY_ELEMENTDATA区分开来，当添加第一个元素时，我们将了解动态数组扩大了多少容量。
      
     */
    private static final Object[] DEFAULTCAPACITY_EMPTY_ELEMENTDATA = {};

    /**
      The array buffer into which the elements of the ArrayList are stored.
      The capacity of the ArrayList is the length of this array buffer. Any
      empty ArrayList with elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA
      will be expanded to DEFAULT_CAPACITY when the first element is added.
     存储ArrayList元素的数组缓冲区。
     ArrayList的容量是这个数组缓冲区的长度。
     任何空的ArrayList在使用时，如果 elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA
     那么在添加第一个元素时扩展为DEFAULT_CAPACITY=10  容量
     */
    transient Object[] elementData; // non-private to simplify nested class access
    //非私有以简化嵌套类访问，当前数据对象存放地方，当前对象不参与序列化

    /**
     * The size of the ArrayList (the number of elements it contains).
     *ArrayList的大小(它包含的元素的数量)。
     * @serial
     */
    private int size;
 /*Java中对象的序列化是指：将对象转换成以字节序列的形式来表示，这些字节序列包含了对象的数据和信息，一个序列化后的对象可以被写到数据库或文件中，也可用于网络传输，一般当我们使用缓存cache(内存空间不够有可能会本地存储到硬盘)或远程调用rpc(网络传输)的时候，经常需要让我们的实体类实现Serializable接口，目的就是为了让其可序列化。
  只要某个类实现了Serilizable接口，这个类所有的属性和方法都会自动序列化。
   transient
   作用：让某些被修饰符的成员属性变量不被序列化。
   什么时候可以使用该关键字呢？
   1. 类中的字段值可以根据其他字段推导出来，如一个长方形类有三个属性：长、宽、面积。那么在序列化的时候，面积这个属性就没必要被序列化了。
   2. 实际开发过程中，某些敏感信息不希望在网络中被传输，这些信息对应的变量就可以加上transient关键字。即，这个字段的生命周期仅存于调用者的内存中，而不会被写到磁盘里持久化。
   总而言之，java 的transient关键字为我们提供了便利，你只需要实现Serilizable接口，将不需要序列化的属性前添加关键字transient，序列化对象的时候，这个属性就不会序列化到指定的目的地中。*/
    /**
      The maximum size of array to allocate (unless necessary).
      Some VMs reserve some header words in an array.
      Attempts to allocate larger arrays may result in
      OutOfMemoryError: Requested array size exceeds VM limit
      要分配的数组的最大大小(除非必要)。
      一些VM在数组中保留一些头信息。
      尝试分配更大的数组可能会导致
      OutOfMemoryError:请求的数组大小超过VM限制
     */
    private static final int MAX_ARRAY_SIZE = Integer.MAX_VALUE - 8;



    //省略了里面的方法
}
```
### ArrayList构造函数
```
/**
     * Constructs an empty list with the specified initial capacity.
     
       @param  initialCapacity  the initial capacity of the list
       @throws IllegalArgumentException if the specified initial capacity
             is negative

构造具有指定初始容量的空列表。
@param 参数 ：initialCapacity 列表的初始容量
如果指定初始容量，@抛出IllegalArgumentException异常
   带int类型的构造函数
如果传入参数，则代表指定ArrayList的初始数组长度，传入参数如果是大于等于0，则使用用户的参数初始化，如果用户传入的参数小于0，则抛出异常，构造方法如下：
     */
    public ArrayList(int initialCapacity) {
        if (initialCapacity > 0) {
            this.elementData = new Object[initialCapacity];
        } else if (initialCapacity == 0) {
            this.elementData = EMPTY_ELEMENTDATA;
        } else {
            throw new IllegalArgumentException("Illegal Capacity: "+
                                               initialCapacity);
        }
    }

    /**
     * Constructs an empty list with an initial capacity of ten.
     构造一个初始容量为10的空列表。
     注意：此时我们创建的ArrayList对象中的elementData中的长度是1，size是0,当进行第一次add的时候，elementData将会变成默认的长度：10.
     无参构造函数
     如果不传入参数，则使用默认无参构建方法创建ArrayList对象，如下：
     */
    public ArrayList() {
        this.elementData = DEFAULTCAPACITY_EMPTY_ELEMENTDATA;
    }

    /**
     * Constructs a list containing the elements of the specified
       collection, in the order they are returned by the collection's
       iterator.
     
      @param c the collection whose elements are to be placed into this list
      @throws NullPointerException if the specified collection is null

      构造一个包含指定元素的列表
      集合，按照顺序，它们将通过集合迭代器返回。
     @param参数： c集合，它的元素将被放置到这个列表中
       如果指定的集合为空，@抛出NullPointerException


带Collection对象的构造函数:
1）将collection对象转换成数组，然后将数组的地址的赋给elementData。
2）更新size的值，同时判断size的大小，如果是size等于0，直接将空对象EMPTY_ELEMENTDATA的地址赋给elementData
3）如果size的值大于0，则执行Arrays.copyOf方法，把collection对象的内容（可以理解为深拷贝）copy到elementData中。

注意：this.elementData = arg0.toArray(); 这里执行的简单赋值时浅拷贝，所以要执行Arrays.copyOf() 做深拷贝

     */
    public ArrayList(Collection<? extends E> c) {
        elementData = c.toArray();
        if ((size = elementData.length) != 0) {

            // defend against c.toArray (incorrectly) not returning Object[]
            // (see e.g. https://bugs.openjdk.java.net/browse/JDK-6260652)

            if (elementData.getClass() != Object[].class)
                elementData = Arrays.copyOf(elementData, size, Object[].class);
        } else {
            // replace with empty array.

            this.elementData = EMPTY_ELEMENTDATA;
        }
    }

```
__遇到的问题：__
？是“任意类”的意思，extends继承不多说，E是指定类型。
E是element 元素,用来定义泛型,可以不写,但是取值的时候要强转,用了泛型就不需要强转(强制类型转换) 

首先，E是泛型。这是java 1.5以后引入的。
从此以后，定义一个空的ArrayList需要这样，比如我需要一个MyClass的顺序表，则：
```
ArrayList<MyClass> myList = new ArrayList<MyClass>();
```
这是因为ArrayList类的声明是public class ArrayList<E>。

而此时，MyClass就是“指定类型”E了，意味着所有ArrayList类中声明的E, 换成了MyClass。

同理如果要使用public ArrayList(Collection<? extends E> c)来构造一个ArrayList，则：

如我有MySubClass是MyClass的子类。
```
Collection<MyClass> myCollection; //myCollection里有很多元素。
Collection<MySubClass> mySubCollection; //mySubCollection里有很多元素。
ArrayList<MyClass> myList = new ArrayList<MyClass>(myCollection);
```
也可以：
```
ArrayList<MyClass> myList = new ArrayList<MyClass>(mySubCollection);
```
意思是，凡是MyClass或者MyClass的子类的Collection均可以构造成ArrayList<MyClass>

### ArrayList的add方法
```
/**
      This helper method split out from add(E) to keep method
      bytecode size under 35 (the -XX:MaxInlineSize default value),
      which helps when add(E) is called in a C1-compiled loop.
这个helper方法从add(E)中分离出来以保留方法
字节码大小小于35 (-XX:MaxInlineSize默认值)，
这有助于在c1编译的循环中调用add(E)。
     */
    private void add(E e, Object[] elementData, int s) {
        if (s == elementData.length)
        elementData = grow();
        elementData[s] = e;
        size = s + 1;
    }

    /**
      Appends the specified element to the end of this list.
     
      @param e element to be appended to this list
      @return {@code true} (as specified by {@link Collection#add})

      将指定的元素追加到此列表的末尾。
      要添加到此列表中的@param e元素
     @return {@code true}(由{@link Collection#add}指定)
     */
    public boolean add(E e) {
        modCount++;
        add(e, elementData, size);
        return true;
    }

    /**
      Inserts the specified element at the specified position in this
      list. Shifts the element currently at that position (if any) and
      any subsequent elements to the right (adds one to their indices).
     
     @param index index at which the specified element is to be inserted
     @param element element to be inserted
     @throws IndexOutOfBoundsException {@inheritDoc}
     将指定元素插入其中的指定位置
    列表。将当前位于该位置(如果有)的元素移动，并
     右边的任何后续元素(将一个元素添加到它们的索引中)。
    @param索引要插入指定元素的索引
    要插入的@param元素
     @throws IndexOutOfBoundsException {@inheritDoc}
     /
    public void add(int index, E element) {
        rangeCheckForAdd(index);
        modCount++;
        final int s;
        Object[] elementData;
        if ((s = size) == (elementData = this.elementData).length)
            elementData = grow();
        System.arraycopy(elementData, index,
                         elementData, index + 1,
                         s - index);
        elementData[index] = element;
        size = s + 1;
    }

```