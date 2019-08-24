---
title: Java开发
categories: Java基础
tags: Java基础
abbrlink: 8c2959bc
date: 2019-08-21 21:53:54
password: 753159
message: Welcome to my blog, enter password to read.  
abstract: Welcome to my blog, enter password to read.  
---
写代码时遇到的问题记录

<!--more-->
### Java注解基本原理
     SpringBoot基于注解开发的，方便快捷，同时代码较为简洁。
     注解的语法比较简单，除了@符号的使用之外，它基本与Java固有语法一致。Java SE5内置了三种标准注解：
```
     @Override，表示当前的方法定义将覆盖超类中的方法。

     @Deprecated，使用了注解为它的元素编译器将发出警告，因为注解@Deprecated是不赞成使用的代码，被弃用的代码。

     @SuppressWarnings，关闭不当编译器警告信息。
```
     上面这三个注解多少我们都会在写代码的时候遇到。Java还提供了其他4种注解，专门负责新注解的创建。
```
@Target

表示该注解可以用于什么地方，可能的ElementType参数有：

CONSTRUCTOR：构造器的声明

FIELD：域声明（包括enum实例）

LOCAL_VARIABLE：局部变量声明

METHOD：方法声明

PACKAGE：包声明

PARAMETER：参数声明

TYPE：类、接口（包括注解类型）或enum声明



@Retention

表示需要在什么级别保存该注解信息。可选的RetentionPolicy参数包括：

SOURCE：注解将被编译器丢弃

CLASS：注解在class文件中可用，但会被VM丢弃

RUNTIME：VM将在运行期间保留注解，因此可以通过反射机制读取注解的信息。



@Document

将注解包含在Javadoc中

@Inherited

允许子类继承父类中的注解
```


    
### 定义一个注解的方式：   
```
 @Target(ElementType.METHOD)
 @Retention(RetentionPolicy.RUNTIME)
 public @interface Test {

}
```
除了@符号，注解很像是一个接口。定义注解的时候需要用到元注解，上面用到了@Target和@RetentionPolic。 

     在注解中一般会有一些元素表示某些值。注解的元素看起来就像接口的方法，唯一的区别在于可以为其制定默认值。没有元素的注解称为标记注解，上面的@Test就是一个标记注解。 

     注解的可用的类型包括以下几种：所有基本类型、String、Class、enum、Annotation.
     以上类型的数组形式,元素不能有不确定的值，即要么有默认值，要么在使用注解的时候提供元素的值。
     而且元素不能使用null作为默认值。
     注解在只有一个元素且该元素的名称是value的情况下，在使用注解的时候可以省略“value=”，直接写需要的值即可。 

     下面看一个定义了元素的注解。

```
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
public @interface UseCase {
     public String id();
     public String description() default "no description";
 }
```
  定义了注解，必然要去使用注解。 

```
 1 public class PasswordUtils {
 2      @UseCase(id = 47, description = "Passwords must contain at least one numeric")
 3      public boolean validatePassword(String password) {
 4          return (password.matches("\\w*\\d\\w*"));
 5      }
 6  
 7      @UseCase(id = 48)
 8      public String encryptPassword(String password) {
 9          return new StringBuilder(password).reverse().toString();
10      }
11  }
```
 

使用注解最主要的部分在于对注解的处理，那么就会涉及到注解处理器。

     从原理上讲，注解处理器就是通过反射机制获取被检查方法上的注解信息，然后根据注解元素的值进行特定的处理。

```
public static void main(String[] args) {
     List<Integer> useCases = new ArrayList<Integer>();
     Collections.addAll(useCases, 47, 48, 49, 50);
     trackUseCases(useCases, PasswordUtils.class);
 }
 
 public static void trackUseCases(List<Integer> useCases, Class<?> cl) {
     for (Method m : cl.getDeclaredMethods()) {
         UseCase uc = m.getAnnotation(UseCase.class);
         if (uc != null) {
             System.out.println("Found Use Case:" + uc.id() + " "
                         + uc.description());
             useCases.remove(new Integer(uc.id()));
         }
     }
     for (int i : useCases) {
         System.out.println("Warning: Missing use case-" + i);
     }
 }

Found Use Case:47 Passwords must contain at least one numeric

Found Use Case:48 no description

Warning: Missing use case-49

Warning: Missing use case-50
``` 

 上面的三段代码结合起来是一个跟踪项目中用例的简单例子

###   java ArrayList详解
 
 1. 什么是ArrayList 
     ArrayList就是动态数组，用MSDN中的说法，就是Array的复杂版本，它提供了如下一些好处： 
       * 动态的增加和减少元素 
       * 实现了ICollection和IList接口 
       * 灵活的设置数组的大小

 2. 如何使用ArrayList 
    最简单的例子： 
```
ArrayList List = new ArrayList(); 
for( int i=0;i <10;i++ ) //给数组增加10个Int元素 
List.Add(i); 
List.RemoveAt(5);//将第6个元素移除 
for( int i=0;i <3;i++ ) //再增加3个元素 
List.Add(i+20); 
Int32[] values = (Int32[])List.ToArray(typeof(Int32));//返回ArrayList包含的数组
```
这是一个简单的例子，虽然没有包含ArrayList所有的方法，但是可以反映出ArrayList最常用的用法

  3. ArrayList重要的方法和属性 
      __构造器__ 
ArrayList提供了三个构造器： 
* public ArrayList(); 
默认的构造器，将会以默认（16）的大小来初始化内部的数组 
* public ArrayList(ICollection); 
用一个ICollection对象来构造，并将该集合的元素添加到ArrayList 
* public ArrayList(int); 
用指定的大小来初始化内部的数组

   __IsSynchronized属性和ArrayList.Synchronized方法__
     IsSynchronized属性指示当前的ArrayList实例是否支持线程同步，
    而ArrayList.Synchronized静态方法则会返回一个ArrayList的线程同步的封装。 
    如果使用非线程同步的实例，那么在多线程访问的时候，需要自己手动调用lock来保持线程同步，例如： 
    ```
ArrayList list = new ArrayList(); 
lock( list.SyncRoot ) 
//当ArrayList为非线程包装的时候，SyncRoot属性其实就是它自己
//，但是为了满足ICollection的SyncRoot定义，这里还是使用SyncRoot来保持源代码的规范性 
{ 
list.Add( “Add a Item” ); 
}
```
如果使用ArrayList.Synchronized方法返回的实例，那么就不用考虑线程同步的问题，这个实例本身就是线程安全的，实际上ArrayList内部实现了一个保证线程同步的内部类，ArrayList.Synchronized返回的就是这个类的实例，它里面的每个属性都是用了lock关键字来保证线程同步。

    __Count属性和Capacity属性__ 
Count属性是目前ArrayList包含的元素的数量，这个属性是只读的。 
Capacity属性是目前ArrayList能够包含的最大数量，可以手动的设置这个属性，但是当设置为小于Count值的时候会引发一个异常。

__Add、AddRange、Remove、RemoveAt、RemoveRange、Insert、InsertRange__
  这几个方法比较类似 
* Add方法用于添加一个元素到当前列表的末尾 
* AddRange方法用于添加一批元素到当前列表的末尾 
* Remove方法用于删除一个元素，通过元素本身的引用来删除 
* RemoveAt方法用于删除一个元素，通过索引值来删除 
* RemoveRange用于删除一批元素，通过指定开始的索引和删除的数量来删除 
* Insert用于添加一个元素到指定位置，列表后面的元素依次往后移动 
* InsertRange用于从指定位置开始添加一批元素，列表后面的元素依次往后移动

另外，还有几个类似的方法： 

* Clear方法用于清除现有所有的元素 
* Contains方法用来查找某个对象在不在列表之中
__TrimSize方法__ 
这个方法用于将ArrayList固定到实际元素的大小，当动态数组元素确定不在添加的时候，可以调用这个方法来释放空余的内存。 

__ToArray方法__
  这个方法把ArrayList的元素Copy到一个新的数组中。 

   4. ArrayList与数组转换
   
例1： 
``` 
ArrayList List = new ArrayList(); 
List.Add(1); 
List.Add(2); 
List.Add(3);

Int32[] values = (Int32[])List.ToArray(typeof(Int32));
```
例2： 
```
ArrayList List = new ArrayList(); 
List.Add(1); 
List.Add(2); 
List.Add(3);

Int32[] values = new Int32[List.Count]; 
List.CopyTo(values);
```
上面介绍了两种从ArrayList转换到数组的方法

例3： 
```
ArrayList List = new ArrayList(); 
List.Add( “string” ); 
List.Add( 1 ); 
//往数组中添加不同类型的元素

object[] values = List.ToArray(typeof(object)); //正确 
string[] values = (string[])List.ToArray(typeof(string)); //错误
```
和数组不一样，因为可以转换为Object数组，所以往ArrayList里面添加不同类型的元素是不会出错的，但是当调用ArrayList方法的时候，要么传递所有元素都可以正确转型的类型或者Object类型，否则将会抛出无法转型的异常。

   5. ArrayList最佳使用建议 
    这一节我们来讨论ArrayList与数组的差别，以及ArrayList的效率问题 
   __ArrayList是Array的复杂版本__ 
     ArrayList内部封装了一个Object类型的数组，从一般的意义来说，它和数组没有本质的差别，甚至于ArrayList的许多方法，如Index、IndexOf、Contains、Sort等都是在内部数组的基础上直接调用Array的对应方法。 
   __内部的Object类型的影响__
    对于一般的引用类型来说，这部分的影响不是很大，但是对于值类型来说，往ArrayList里面添加和修改元素，都会引起装箱和拆箱的操作，频繁的操作可能会影响一部分效率。 
    但是恰恰对于大多数人，多数的应用都是使用值类型的数组。 
    消除这个影响是没有办法的，除非你不用它，否则就要承担一部分的效率损失，不过这部分的损失不会很大。 
   __数组扩容__
    这是对ArrayList效率影响比较大的一个因素。 
    每当执行Add、AddRange、Insert、InsertRange等添加元素的方法，都会检查内部数组的容量是否不够了，如果是，它就会以当前容量的两倍来重新构建一个数组，将旧元素Copy到新数组中，然后丢弃旧数组，在这个临界点的扩容操作，应该来说是比较影响效率的。 
    例1：
    比如，一个可能有200个元素的数据动态添加到一个以默认16个元素大小创建的ArrayList中，将会经过： 
   16 * 2 * 2 * 2 * 2 = 256 
   四次的扩容才会满足最终的要求，那么如果一开始就以： 
    ArrayList List = new ArrayList( 210 ); 
    的方式创建ArrayList，不仅会减少4次数组创建和Copy的操作，还会减少内存使用。

    例2：预计有30个元素而创建了一个ArrayList： 
    ArrayList List = new ArrayList(30); 
    在执行过程中，加入了31个元素，那么数组会扩充到60个元素的大小，而这时候不会有新的元素再增加进来，而且有没有调用TrimSize方法，那么就有1次扩容的操作，并且浪费了29个元素大小的空间。如果这时候，用： 
    ArrayList List = new ArrayList(40); 
    那么一切都解决了。 

     所以说，正确的预估可能的元素，并且在适当的时候调用TrimSize方法是提高ArrayList使用效率的重要途径。 

__频繁的调用IndexOf、Contains等方法（Sort、BinarySearch等方法经过优化，不在此列）引起的效率损失__

  首先，我们要明确一点，ArrayList是动态数组，它不包括通过Key或者Value快速访问的算法，所以实际上调用IndexOf、Contains等方法是执行的简单的循环来查找元素，所以频繁的调用此类方法并不比你自己写循环并且稍作优化来的快，如果有这方面的要求，建议使用Hashtable或SortedList等键值对的集合。 
```
ArrayList al=new ArrayList();

al.Add("How"); 
al.Add("are"); 
al.Add("you!");

al.Add(100); 
al.Add(200); 
al.Add(300);

al.Add(1.2); 
al.Add(22.8);
```
   __ToArray方法__
这个方法把ArrayList的元素Copy到一个新的数组中。 

__使用ArrayList类__
ArrayList类实现了List接口，由ArrayList类实现的List集合采用数组结构保存对象。
  数组结构的优点是便于对集合进行快速的随机访问，如果经常需要根据索引位置访问集合中的对象，使用由ArrayList类实现的List集合的效率较好。
  数组结构的缺点是向指定索引位置插入对象和删除指定索引位置对象的速度较慢，如果经常需要向List集合的指定索引位置插入对象，
  或者是删除List集合的指定索引位置的对象，使用由ArrayList类实现的List集合的效率则较低，
  并且插入或删除对象的索引位置越小效率越低，原因是当向指定的索引位置插入对象时，
  会同时将指定索引位置及之后的所有对象相应的向后移动一位。当删除指定索引位置的对象时，
  会同时将指定索引位置之后的所有对象相应的向前移动一位。如果在指定的索引位置之后有大量的对象，
  将严重影响对集合的操作效率。


* 看一个模仿经常需要随机访问集合中对象的例子。
在编写该例子时，用到了Java.lang.Math类的random()方法，通过该方法可以得到一个小于10的double型随机数，将该随机数乘以5后再强制转换成整数，将得到一个0到4的整数，并随机访问由ArrayList类实现的List集合中该索引位置的对象，具体代码如下：
```
public static void main(String[] args) {
String a = "A", b = "B", c = "C", d = "D", e = "E";
List<String> list = new ArrayList<String>();
list.add(a);      // 索引位置为 0
list.add(b);      // 索引位置为 1
list.add(c);      // 索引位置为 2
list.add(d);      // 索引位置为 3
list.add(e);      // 索引位置为 4
System.out.println(list.get((int) (Math.random() * 5)));     // 模拟随机访问集合中的对象
}
```
实验代码：

```
import java.util.ArrayList;
import java.util.Iterator;
public class SimpleTest {
 
 
public static void main(String []args){
  
  ArrayList list1 = new ArrayList();  
  list1.add("one");
  list1.add("two");
  list1.add("three");
  list1.add("four");
  list1.add("five");
  list1.add(0,"zero");  
  System.out.println("<--list1中共有>" + list1.size()+ "个元素");  
  System.out.println("<--list1中的内容:" + list1 + "-->");
  
  ArrayList list2 = new ArrayList();
  list2.add("Begin");
  list2.addAll(list1);
  list2.add("End");
  System.out.println("<--list2中共有>" + list2.size()+ "个元素");  
  System.out.println("<--list2中的内容:" + list2 + "-->");
  
  ArrayList list3 =  new ArrayList();
  list3.removeAll(list1);
  System.out.println("<--list3中是否存在one: "+ (list3.contains("one")? "是":"否")+ "-->");
  
  list3.add(0,"same element");
  list3.add(1,"same element");
  System.out.println("<--list3中共有>" + list3.size()+ "个元素");  
  System.out.println("<--list3中的内容:" + list3 + "-->");
  System.out.println("<--list3中第一次出现same element的索引是" + list3.indexOf("same element") + "-->");
  System.out.println("<--list3中最后一次出现same element的索引是" + list3.lastIndexOf("same element") + "-->");
  
  
  System.out.println("<--使用Iterator接口访问list3->");
  Iterator it = list3.iterator();
  while(it.hasNext()){
   String str = (String)it.next();
   System.out.println("<--list3中的元素:" + list3 + "-->");
  }
  
  System.out.println("<--将list3中的same element修改为another element-->");
  list3.set(0,"another element");
  list3.set(1,"another element");
     System.out.println("<--将list3转为数组-->");
    // Object []  array =(Object[]) list3.toArray(new   Object[list3.size()] );
     Object [] array = list3.toArray();
     for(int i = 0; i < array.length ; i ++){
      String str = (String)array[i];
      System.out.println("array[" + i + "] = "+ str);      
     }     
     
     System.out.println("<---清空list3->");
     list3.clear();
     System.out.println("<--list3中是否为空: " + (list3.isEmpty()?"是":"否") + "-->");
     System.out.println("<--list3中共有>" + list3.size()+ "个元素"); 
  
  //System.out.println("hello world!");
 }
}
```

## java中的Pair功能
配对(Pair)。配对提供了一种方便方式来处理简单的键值关联，当我们想从方法返回两个值时特别有用。

在核心Java库中可以使用配对(Pair)的实现。除此之外，某些第三方库，比如Apache Commons和Vavr，已经在各自的api中公开了这个功能。

#####  核心java配对实现

###### Pair类

Pair类在javafx.util 包中，类构造函数有两个参数，键及对应值：
```
    Pair<Integer, String> pair = new Pair<>(1, "One");
    Integer key = pair.getKey();
    String value = pair.getValue();
```
示例描述使用Pair类实现简单Integer到String的映射。示例中getKey方法返回key对象，getValue方法返回对应值对象。

#### AbstractMap.SimpleEntry 和 AbstractMap.SimpleImmutableEntry

SimpleEntry定义在抽象类AbstractMap里面，其构造方法与Pair类似：
```
    AbstractMap.SimpleEntry<Integer, String> entry 
      = new AbstractMap.SimpleEntry<>(1, "one");
    Integer key = entry.getKey();
    String value = entry.getValue();
```
其键和值可以通过标准的getter和setter方法获得。

另外AbstractMap 类还包含一个嵌套类，表示不可变配对：SimpleImmutableEntry 类。
```
    AbstractMap.SimpleImmutableEntry<Integer, String> entry
      = new AbstractMap.SimpleImmutableEntry<>(1, "one");
```
应用方式与可变的配对一样，除了配置的值不能修改，尝试修改会抛出UnsupportedOperationException异常。

##### Apache Commons

在Apache Commons库中，org.apache.commons.lang3.tuple 包中提供Pair抽象类，不能被直接实例化。 
其有两个子类，分别代表可变与不可变配对：ImmutablePair 和 MutablePair。两者都实现了访问key/value以及setter和getter方法：
```
    ImmutablePair<Integer, String> pair = new ImmutablePair<>(2, "Two");
    Integer key = pair.getKey();
    String value = pair.getValue();
```
尝试在ImmutablePair 执行setValue方法，会抛出UnsupportedOperationException异常。但在可变配对上执行完全正常：

    Pair<Integer, String> pair = new MutablePair<>(3, "Three");
    pair.setValue("New Three");

##### Vavr库

Vavr库中不可变的Tuple2类提供配对功能：
```
    Tuple2<Integer, String> pair = new Tuple2<>(4, "Four");
    Integer key = pair._1();
    String value = pair._2();
```
在这个实现中，创建对象后不能修改，所以更新方法返回改变后的新实例：
```
    tuplePair = pair.update2("New Four");
```

#### java中Pop和 poll 的区别：
poll是队列数据结构实现类的方法，从队首获取元素，同时获取的这个元素将从原队列删除； 
pop是栈结构的实现类的方法，表示返回栈顶的元素，同时该元素从栈中删除，当栈中没有元素时，调用该方法会发生异常




#### Java集合之LinkedList
   LinkedList是基于链表实现。
   LinkedList是一种双向链表
   1. 链表中任意一个存储单元都可以通过向前或向后寻址的方式获取到其前一个存储单元和其后一个存储单元。
   2. 链表的尾结点的后一个结点是链表的头结点，链表的头结点的前一个结点是链表的尾结点。

   LinkedList底层的数据结构是基于双向循环链表的，且头结点不存放数据。

   双向链表，存在“节点”这样一种数据结构，保存业务数据、前一个节点的位置信息和后一个节点位置信息。

   私有属性：
   LinkedList定义了两个私有属性：
   ```
   private transient Entry<E> header =new Entry<E>(null,null,null);

   private transient int size=0;

   ```
  header是双向链表的头节点，它是双向链表节点所对应的类Entry的实例。Entry中包含成员变量：previous,next,element。
  其中，previous是该节点的上一个节点，next是该节点的下一个节点，element是该节点所包含的值。
  size是双向链表中节点实例的个数。

 __节点类Entry类的代码__
 ```
 private static class Entry<E>{
  E element;
  Entry<E> next;
  Entry<E> previous;

  Entry(E element,Entry<E> next,Entry<E> previous)
  {
    this.element=element;
    this.next=next;
    this.previous=previous;

  }
 }

 ```
LinkedList的Entry的“E element”，就是它真正存储的数据。
"Entry<E> next"和"Entry<E> previous"表示的是这个存储单元的前一个存储单元的引用地址和后一个存储单元的引用地址。

__构造函数__
LinkedList提供了两个构造方法，
```
public LinkedList()
{
  header.next=header.previous=header;
}
public LinkedList(Collection<? extends E> c)
{
  this();
  addAll(c);
}

```
第一个构造方法不接受参数，将header实例的previous和next全部指向header实例（注意，这个是一个双向循环链表，如果不是循环链表，空链表的情况应该是header节点的前一节点和后一节点均为null），这样整个链表其实就只有header一个节点，用于表示一个空的链表。

第二个构造方法接受一个Collection参数c，调用第一个构造方法构造一个空的链表，之后通过addAll将c中的元素全部添加到链表中。

__注意__
 LinkedList是否允许空：允许
 LinkedList是否允许重复数据：允许
 LinkedList是否有序：有序
 LinkedList是否线程安全：非线程安全

__添加元素__
  LinkedList添加一个元素，代码如下：
  ```
  public static void main(String[] args)
  {
    List<String> list=new LinkedList<String>();
    list.add("123");
    list.add("465");
  }

  ```
我们逐步分析main函数中的代码是如何执行的，看一下LinkedList源码：
```
public class LinkedList<E>
    extends AbstractSequentiaList<E>
    implements List<E>,Deque<E>,Cloneable,java.io.Serializable
    {
      private transient Entry<E> header=new Entry<E>(null,null,null);
      private transient int size=0;
      public LinkedList()
      {
        header.next=header.previous=header;
      }
    }
```
分析上段代码，先理解transient,在理解transient之前，先搞清楚Java中序列化的含义：
  Java中对象的序列化是指：将对象转换成以字节序列的形式来表示，这些字节序列包含了对象的数据和信息，一个序列化后的对象可以被写到数据库或文件中，也可用于网络传输，一般当我们使用缓存cache(内存空间不够有可能会本地存储到硬盘)或远程调用rpc(网络传输)的时候，经常需要让我们的实体类实现Serializable接口，目的就是为了让其可序列化。
  只要某个类实现了Serilizable接口，这个类所有的属性和方法都会自动序列化。
__transient__
   作用：让某些被修饰符的成员属性变量不被序列化。
   什么时候可以使用该关键字呢？
   1. 类中的字段值可以根据其他字段推导出来，如一个长方形类有三个属性：长、宽、面积。那么在序列化的时候，面积这个属性就没必要被序列化了。
   2. 实际开发过程中，某些敏感信息不希望在网络中被传输，这些信息对应的变量就可以加上transient关键字。即，这个字段的生命周期仅存于调用者的内存中，而不会被写到磁盘里持久化。
   总而言之，java 的transient关键字为我们提供了便利，你只需要实现Serilizable接口，将不需要序列化的属性前添加关键字transient，序列化对象的时候，这个属性就不会序列化到指定的目的地中。
    

### Java中Class类详解、用法及泛化






###   负载均衡面试题