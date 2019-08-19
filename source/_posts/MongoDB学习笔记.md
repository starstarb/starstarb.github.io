---
title: MongoDB学习笔记
tags: MongoDB
categories: 数据库
abbrlink: a36562d7
date: 2019-07-24 18:40:21
---
MongoDB学习笔记
<!--more-->

MongoDB简介

####我们为什么要去学习MongoDB####

MongoDB是一种强大、灵活、可扩展的数据存储方式。
####丰富的数据模型####

MongoDB是面向文档的数据库，不是关系型数据库。它将原来**‘行’（row）的概念换成了更加灵活的‘文档’（document）模型**。面向文档的方式可以将文档和数组内嵌进来，所以用一条记录就可以表示很复杂的层次关系。 MongoDB没有模式，文档的键不会事先定义也不会固定不变。
####容易扩展####

MongoDB所采用的面向文档的数据模型，使其可以自动的在多台服务器之间分割数据，还可以平衡集群的数据和负载，自动重排文档。
####丰富的功能####

索引
存储JavaScript
聚合
固定集合
文件存储
####不牺牲速度####

MongoDB使用了MongoDB传输协议作为与服务器交互的主要方式，它对文档进行动态填充，预分配数据文件，用空间换取性能的稳定，等等。。
####简便的管理####

MongoDB的管理理念就是尽可能的让服务器自动配置，让用户能在需要的时候调整设置。

### 文档

文档是MondoDB的核心概念，多个键及其关联的值有序的存放在一起便是文档。可以非常类似关系型数据库里的“行”。
####吐槽#### 
其实文档要比“行”复杂的多！

在JavaScript里，文档对象通常长这个样子：

{"greeting":"Hello world"}
greeting(key)/Hello world(value)
在MongoDB里可能会长这样：

{"greeting":"Hello world","foo":3} {"foo":"3","greeting":"hello world"}
当然，MongoDB里的文档通常会更加的复杂，不过，这里咱们有必要注意几点：

* 文档中的键/值对是有序的，所以上下两个文档是不同的 
* 文档的键是字符串，而值不仅可以使字符串，也可以是其他几种类型的值
* 文档的值不仅可以使字符串，也可以是其他几种类型的值
* 文档的键：
    不能有\0(因为它表示结尾)
    .和$在特定环境下使用，因为它们是保留字
     以“_”为开头的键是保留的
* MongoDB不但区分类型，还区分大小写。两个foo的值是不同的，一个数字，一个字符串，两个greeting的键也是不同的（H和h）

####小心#### 
 MongoDB文档不能有重复的键！

### 集合

* 集合就是一组文档，非常类似于关系型数据库里的表。 
* 集合的特点就是无模式，就是说一个集合里可以放各种各样的不同文档。 
* 那么随之来的问题是：“有必要使用多个集合吗？” 答案当然是有啦（暗暗一笑），下面是一些理由
* 把各种文档都存放在一个集合里，太乱！
     因为乱，所以你一旦需要查询某键的时候，就会太慢！
     将同类的文档放在一个集合里，逻辑上更合理
索引是按照集合来创建的，将同类的东西放入文档可以提高索引的效率

####举例#### 
超市里都是把同类的商品放在一起贩卖；数学上也有名为“合并同类项”的方法。 
####总结####
 集合是为了更有逻辑的分类不同类型的文档，使得MongoDB的效率提高。

### 子集合

通过用"."来按命名空间划分子集合。如： blog有两个集合posts和authors，可以这样：blog.posts,blog.authors
命名

* 不能是空字符串""
* 不能含有\0，（表示结尾）
* 不能以system开头
* 不能有保留字$
* 数据库

* 命名 ** 不能是空字符串""
不能含有\0 . \ / $ ''(空格)
全部应该小写 

####吐槽#### 
因为数据库名最终会变成文件系统里的文件，所以会有这么多限制... 
####注意#### 
有一些数据库名是保留的，如：admin、local、config

### 下载MongoDB

想要将大象放进冰箱，首先得有一只大象 ！同理，想要玩转MongoDB，咱总的想把它下载下来吧~
第一步：登上MongoDB官网，找到自己的适合的版本下载
第二步：解压（免安装），改名mongodb（举例命名，可以任个人喜好），放在你喜欢的位置（任喜好）
第三步：通过命令行:
cd mongodb(进入mongodb目录)
cd bin（进入bin目录）
./mongod(运行启动命令)
如果看到有错误莫担心，咱慢慢往下读
####常见疑难杂症#### 
你错误的情况可能是

mongodb默认的数据目录为/data/db，如果这个目录不存在或者不可写，服务器会启动失败
解决方案：mkdir -p /data/db，手动创建目录，并确保有写权利
还有可能27017端口被占用，什么意思呢？就是你已经启动了./mongod了，没有关闭，所以端口占用了，因为mongodb的默认端口为27017

解决方案：另打开命令行，输入ps -A，查看./mongod的PID（进程号），找到后输入 kill -15 PID。
还有可能是需要在sudo下才能执行

依然要再bin目录下，输入sudo ./mongod
在启动MongoDB时，输入./mongod的后面可以跟其他的配置参数，不配置即为默认，在网上可以找到相应的详细讲解配置的博客
成功后，咱另打开个命令行，依然是bin目录下，输入./mongo,启动MongoDB shell，在shell里输入help，可以查看具体方法，这里就不概述了


### windows10安装配置mongodb


### MongoDB从零起步走之insert、remove

连接MongoDB（bin目录下）

./mongo
如果觉得shell里空空的可以输入help，在刷屏的同时大致了解下有哪些方法

help
现在咱们还没有数据库，咱们创建一个，任性起名：template

use template
咱们确认下，数据库有没有创建成功

show dbs
template 0.078GB
如果存在template，就进入，如果没有，在最后保存的时候就会创建template

###insert###

发现已经创建成功，继续走，现在咱们创建一个集合,任性起名：room,再插入点数据
```
db.room.insert({"desk":1,"bed":1,"window":2})
db.room.find()
{ "_id" : ObjectId("55081e42591555a6c35dd695"), "desk" : 1, "bed" : 1, "window" : 2 }
```
####知识点####

insert会给文档增加一个_id（如果原来没有的话），这个_id是文档的唯一标示，然后保存到数据库中
###remove###

现在咱们不想要这个room文档了，那么来删除它

db.room.remove({})
注意，在remove() 里传入了空对象{},意为删除全部数据，除了这样删除，我们还可以这样删除

db.room.remove({"desk":1})
####Tips####

db.dropDatabase()
删除数据库

db.collection.drop()
删除集合
### Update
update方法很强大，它有两个参数，一是查询文档，用来找出需要更新的文档，另一个是修改器（modifier）文档，描述对找到的文档做哪些修改。

####亮点#### 
更新操作是原子的，若两个更新同时发生，先到达服务器的现执行，接着执行另一个。所以，互相冲突的更新可以火速传递，并不会互相干扰，虽然这是一个拼速度的年代，但是后更新的会取得“胜利”（后发制人！）

因为使用原子的 更新修改器 进行更新操作极为高效，所以，就决定用它了！

###$inc### 
例如：你需要存储一个网站的访问次数
```
db.web.insert({"url":"www.example","count":2})
{ "_id" : ObjectId("55082435591555a6c35dd697"), "url" : "www.example", "count" : 2 }
这时，可以用$inc来原子的增加（减少）count的值

db.web.update({"url":"www.example"},{$inc:{"count":1}})
{ "_id" : ObjectId("55082435591555a6c35dd697"), "url" : "www.example", "count" : 3 }
$inc指定是对数字的增减操作，count指定操作对象，1指定步长（正增负减）
```
###$set & $unset###

用$set指定一个键的值，如果不存在，就创建它。这对更新模式或者增加用户定义很有帮助。
```
db.user.insert({"name":"qianjiahao"})
此用户现在只有姓名信息，现在需要给他添加email

db.user.update({"name":"qianjiahao"},{"$set":{"email":"example@example.com"}})
{ "_id" : ObjectId("55082691591555a6c35dd698"), "name" : "qianjiahao", "email" : "example@example.com" }
比如现在他又要添加他的个人爱好，他可能有很多的爱好，所以爱好应该是一个数组

db.user.update({"name":"qianjiahao"},{"$set":{"hobby":["swimming","running","reading"]}})
{ "_id" : ObjectId("55082691591555a6c35dd698"), "name" : "qianjiahao", "email" : "example@example.com", "hobby" : [ "swimming", "running", "reading" ] }
假如他现在又没有爱好了...

db.user.update({"name":"qianjiahao"},{"$unset":{"hobby":1}})
{ "_id" : ObjectId("55082691591555a6c35dd698"), "name" : "qianjiahao", "email" : "example@example.com" }
爱好就消失了...
```
###$push### 
数组修改器，既然名字都这样叫了，那么这个修改器就只能对数组进行操作啦。
```
db.user.update({"name":"qianjiahao"},{"$push":{"hobby":"sleeping"}})
{ "_id" : ObjectId("55082691591555a6c35dd698"), "name" : "qianjiahao", "email" : "example@example.com", "hobby" : [ "swimming", "running", "reading", "sleeping" ] }
但是这里有个问题，万一你不确定hobby里面是否有一个值，如“singing”，那么也许下面这个方法更适合
```
###$addToSet & $each ### 
用$addToSet更新可以避免重复，将它与$each组合起来，可以一次性添加多条（就算后添加的值已存在也没有关系）
```
db.user.update({"name":"qianjiahao"},{"$addToSet":{"hobby":{"$each":["singing","eating","dancing"]}}})
{ "_id" : ObjectId("55082691591555a6c35dd698"), "name" : "qianjiahao", "email" : "example@example.com", "hobby" : [ "swimming", "running", "reading", "sleeping", "singing", "eating", "dancing" ] }
```
###$pop### 
如果将数组看做队列，可以用$pop方法删除第一个或者最后一个元素
 {$pop:{"key":-1}}，{$pop:{"key":1}}

###$pull### 
它可以删除所匹配的值，如果[1,1,2,1] 执行pull 1 后，只剩下[2]

###定位修改器###
 如果要操作数组中的值，可以用值在数组中的位置当做参数来删除
```
db.user.update({"name":"qianjiahao"},{"$set":{"hobby.0":"crying"}})
{ "_id" : ObjectId("55082691591555a6c35dd698"), "name" : "qianjiahao", "email" : "example@example.com", "hobby" : [ "crying", "running", "reading", "sleeping", "singing", "eating", "dancing" ] }
现在哭成了主要爱好了，这不行，咱得改了，但是如果不事先去查，我们并不知道哭是这个hobby数组的第几个值，MongoDB为我们考虑到了这点，使用$来代替位置。

db.user.update({"hobby":"crying"},{"$set":{"hobby.$":"smiling"}})
{ "_id" : ObjectId("55082691591555a6c35dd698"), "name" : "qianjiahao", "email" : "example@example.com", "hobby" : [ "smiling", "running", "reading", "sleeping", "singing", "eating", "dancing" ] }
```

####upsert##
 upsert是一个选项，它是update的第三个参数，并不是一个方法。它是一种特殊的更新，要是没有文档符合匹配，那么它就会根据条件和更新文档为基础，创建新的文档，如有匹配，则正常更新。咱们之前见到的所有update操作，都是建立在有文档的基础之上的。upsert非常方便，不必预制集合，同一套代码既可以创建又可以更新。

超市需要修改商品的价格，比如将苹果的价格上调0.5元，但是店主不确定自己是否有购进苹果（偶尔会犯糊涂），那么他可以这样
```
db.supermarket.update({"name":"apple"},{$set:{"price":5.5}},True)
如果MongoDB之前有苹果的记录，那么就会update苹果的价格，如果没有就会创建新的文档

{ "_id" : ObjectId("55083db0720f2a27156f66ed"), "name" : "apple", "price" : 5.5 }
```
###save###
```
save是一个shell函数，调用它，可以在文档不存在时插入，存在时更新，它只有一个参数：文档。如果文档有 _id 这个 键，那么save会调用upsert，否则会调用insert，非常方便。
```
###多文档更新###

当一次更新一个文档无法满足我们的脚步时，我们可以选择一次更新多个文档，及在update的第四个参数的位置添上true，及做多文档更新，建议就算不做多文档更新也显式的在第四个参数上置false，这样明确易懂，也可以在默认参数变化时从容应对。

运行getLastError命令可以帮助我们获取反馈信息
```
db.count.update({"x":3},{$inc:{"x":2}},false,true)
db.runCommand({getLastError:1})
{ "connectionId" : 199, "updatedExisting" : true, "n" : 1, "syncMillis" : 0, "writtenTo" : null, "err" : null, "ok" : 1 }
```
###安全VS性能###
 insert、remove、update这些操作都是瞬间的，他们不需要等待数据库响应，可以理解为子弹从枪膛里射出但不关心射没射中！并不是说这些操作是异步的，而是说后面的事你不用担心了，说白了，子弹射出去后怎么飞、怎么击中目标那都是子弹的事了，咱也管不了了。

这个特点是速度快，快通常挺好，但是万一遇到点什么岔子，小到踢断网线，中到停电淹水，大到火山爆发。借用周星驰电影里的一句话：“世事难预料”，可能有些情况还能接受，但是如果是安全性要求很高的应用场景，那就不能接受了。

所以，安全性要求高的解决方案是：执行完操作，立刻返回getLastError，如果出错，一般都会抛出一个可捕获的异常，好及时处理，如果成功了，会给出额外的信息作为反馈。

但这样一来，性能方面肯定就不如之前了，因为用户会额外等待数据库做出响应。

至于具体是对安全性要求高，还是对性能要求高，要视具体情况而定。

####第一个参数#### 
MongoDB使用find来进行查询，查询呢，就是返回一个集合中文档的子集，子集合的范围从0个文档到整个集合。

find的第一个参数决定了要返回那些文档，其形式也就一个文档，说明了要执行的查询细节。

通常呢，大家看到的find查询基本都长这个样子

db.user.find({})
db.user.find({"name":"qianjiahao"})
db.user.find({"name":"qianjiahao","email":"example@example.com"})
####第二个参数#### 
但是，有的时候，我们并不希望将文档中的所有键/值对都返回，这时，我们可以在find方法的第二个参数上指明我们希望返回的信息。
```
db.user.find({},{"name":1,"email":1})
上面的语句意思是：我们只想得到name和email，其他的不关心。像这样指明返回信息的做法肯定是有好处的，它可以帮助我们节省传输的数据量，又能节省客户端解码文档的时间和内存消耗。 比如，现在有这两条数据

{ "_id" : ObjectId("5509087e08fa61313b5a8230"), "name" : "william", "email" : "example@example.com" }

{ "_id" : ObjectId("5509088b08fa61313b5a8231"), "name" : "jack", "email" : "example@example1.com" }

我们只想得到name，连 _id 都不想要，那么可以这样

db.user.find({},{"name":1,"_id":0})
{ "name" : "william" }

{ "name" : "jack" }
```
####注意####
 数据库关心的查询文档的值，必须是常量（在你自己的代码里可以是正常的变量），换句话说，不可以引用文档中其他键的值！

###查询条件###

####$lt $lte $gt $gte#### 
以上四个分别表示为：< 、 <= 、 > 、 >= 。 通常的做法是将他们组合起来，以便查找一个范围。 比如，查询年龄在18到25岁（含）的人，我们可以这样
```
db.user.find({"age":{"$gte":18,"$lte":25}})
这样的范围查询对查询日期特别有用 比如，查询在2015年1月1日后注册的用户

start = new Date("01/01/2015") db.user.find({"register":{"$gte":start}})
```
####注意####
 不要去匹配精确的日期，而是用范围来对日期进行查询

####$ne####
 $ne表示不相等

db.user.find({"name":{"$ne":"william"}})
###OR查询###
####$in####
  $in可以查询一个键的多个值 举例，每个人有爱好，假定为一个,数据太多，咱们用第二个参数来过滤一下
```
db.user.find({},{"_id":0})
{ "hobby" : "swimming", "gender" : "female" }

{ "hobby" : "dancing", "gender" : "male" }

{ "hobby" : "singing", "gender" : "male" }

我们想查询喜欢dancing和swimming和的人，可以得到如下结果

db.user.find({"hobby":{"$in":["dancing","swimming"]}},{"_id":0})
{ "hobby" : "swimming", "gender" : "female" }

{ "hobby" : "dancing", "gender" : "male" } 若只查询会跳舞的人

db.user.find({"hobby":{"$in":["dancing"]}},{"_id":0})
{ "hobby" : "dancing", "gender" : "male" }

既然$in,那么与之相对的就$nin,可以查询到不包括指明信息的文档
```
###$or#### 
我们再添加一个游泳的人，并用$in查询游泳的人
```
db.user.find({"hobby":{"$in":["swimming"]}},{"_id":0})
{ "hobby" : "swimming", "gender" : "female" }

{ "hobby" : "swimming", "gender" : "male" }

$in 是对单个键进行的查询，用$or查询可以匹配多个键

db.user.find({"$or":[{"hobby":"swimming"},{"gender":"female"}]},{"_id":0})
{ "hobby" : "swimming", "gender" : "female" }

{ "hobby" : "swimming", "gender" : "male" } 现在，我们把查询条件的female改成male

db.user.find({"$or":[{"hobby":"swimming"},{"gender":"male"}]},{"_id":0})
{ "hobby" : "swimming", "gender" : "female" }

{ "hobby" : "dancing", "gender" : "male" }

{ "hobby" : "singing", "gender" : "male" }

{ "hobby" : "singing", "gender" : "male" }

{ "hobby" : "dancing", "gender" : "male" }

{ "hobby" : "swimming", "gender" : "male" }

现在我们可以得出结论，OR查询（$in 和 $or）是尽可能的获取更多的匹配项。 OR查询其实是取并集，满足其中一条及以上，即可被查询到。
```
####$not#### 
not 是元条件句，可以用于任何条件之上，意为取反

####注意#### 
一个键不能对应多个更新修改器 但是可以对应多个条件查询句 比如，可以这样
```
db.user.find({"age":{"$gt":18,"$lt":30}})
但是，不可以这样

{"$inc":{"age":1},"$set":{"age":20}}
因为他们修改了两次age
```
###查询数组###

####$all#### 
利用all来查询所以满足的匹配项,已知数据库有这些数据
```
db.user.find({},{"_id":0})
{ "fruit" : [ "apple", "banana", "cherry" ] }

{ "fruit" : [ "apple", "banana", "pear" ] }

{ "fruit" : [ "apple", "watermelon", "pear" ] }
```
需要查询出有apple和cherry的文档
```
db.user.find({"fruit":{$all:["apple","cherry"]}},{"_id":0})

{ "fruit" : [ "apple", "banana", "cherry" ] }
```
与之前的OR查询（$or $in）相比，$all的查询模式更像是去取交集，即同时满足查询条件才能被查询出来。

####$size####

size可以获得指定数组长度的文档
```
db.user.find({"fruit":{"$size":3}},{"_id":0})
{ "fruit" : [ "apple", "banana", "cherry" ] }

{ "fruit" : [ "apple", "banana", "pear" ] }

{ "fruit" : [ "apple", "watermelon", "pear" ] }
```
###游标### 
数据库使用游标来控制find的执行结果。

客户端对游标的实现通常能够对最终结果进行有效控制。

可以限制结果的数量，略过部分结果，对任意方向任意键的组合对结果进行排序，或者去执行一些功能强大的操作。

我们来看一例
```
var cursor = db.foo.find().sort({"x":1}).limit(3).skip(5)
```
上面代码的意思是：对foo集合查询文档，参照x键的值升序排序，仅显示跳过前五个后的前三个文档。

在接触过Jquery后，感觉这样的链式函数调用很眼熟。

其实他们的原理是一样的，因为几乎所有的游标对象的方法都会返回游标本身，

所以才可以使用如此优雅的方式去调用函数。

####注意#### 
在调用find的时候，shell并不立即查询数据库，

而是等待正要获得结果的时候才发送查询，这样在执行之前可以给查询附加额外的选项。

####limit、sort和skip####

limit指的是上限
sort里 1 为升序，-1为降序，如果sort里有多个值，那就按照多个键的顺序，依次排序
skip...就是skip（额）
用这三个方法可以通常可以用来制作分页。

####辩证的看待游标#### 
在服务器端，游标消耗内存和其他资源。

游标遍历尽了结果以后，或者客户端发来消息要求终止，数据库将会释放这些资源，释放掉的资源可以被数据库换作他用。

这非常有必要，所以尽量保证尽快释放资源（在合理的前提下）

还有一些情况导致游标终止

首先，当游标完成了匹配结果的迭代时，它会清除自身。
另外，当游标在客户端已不在作用域内时，驱动会向服务器发送专门的消息，让其销毁。
最后，即便用户没有迭代完所有的结果，并且游标还在作用域内，10分钟不使用，数据库游标也会自动销毁。

### MongoDB的文档必须有一个_id键。

目的是为了确认在集合里的每个文档都能被唯一标识。
```
ObjectId 是 _id 的默认类型。

ObjectId 采用12字节的存储空间，每个字节两位16进制数字，是一个24位的字符串。
12位生成规则：

[0,1,2,3] [4,5,6] [7,8] [9,10,11]
```
时间戳 |机器码 |PID |计数器

前四字节是时间戳，可以提供秒级别的唯一性。

接下来三字节是所在主机的唯一标识符，通常是机器主机名的散列值。

接下来两字节是产生ObjectId的PID，确保同一台机器上并发产生的ObjectId是唯一的。

前九字节保证了同一秒钟不同机器的不同进程产生的ObjectId时唯一的。

最后三字节是自增计数器，确保相同进程同一秒钟产生的ObjectId是唯一的。
## 索引

MongoDB中的索引，可以看作是书的目录。 想象一下给你一本没有目录的书，然后让你去查询指定内容，我只想说，我不是电脑，我很蛋疼！ 让你翻没有目录的书，就跟让电脑查询没有索引的集合一样，从头查询到尾，毫无头绪，我们称这种行为是“表扫描”

### 索引简介

通常我们要查询一个user的名字,我们是这样的
```
db.user.find({"username":"william"})
```
如果我们经常需要对username进行查询，那么我们可以为它建立索引
```
db.user.ensureIndex({"username":1})
```
对于同一个集合，索引只需要创建一次，反复创建是徒劳的。

对某个键创建索引会加速对某个键的查询，然而，对于其他键可能没有帮助，即便是查询包含了该索引的键。

总是让电脑进行“毫无头绪”的表扫描也是挺折磨电脑的。 所幸MongoDB也不笨，它会在每次查询后，重排查询项的顺序，自己建立索引。 比如在进行这个查询时，{"x":"foo","y":"bar"}，就已经有了 {"x":1,"y":1} 索引了。 每个集合的默认最大索引数是64个。

### 优vs缺

* 优：创建索引可以加速我们的查询，提高效率。
* 缺：每次插入、更新和删除时都会产生额外开销
索引有时并不是最有效的方法，如果查询要返回集合中一半以上的结果，那么通常用表扫描会比用索引效率高一点

### 索引内嵌文档中的键

对应内嵌文档中的键，我们可以这样添加索引``` db.blog.ensureIndex({"comments.date":1})```

### 为排序创建索引

随着集合的增长，需要针对排序中大量的排序做索引。 如果对没有索引的键调用sort，MongoDB需要将所有数据调到内存中来进行排序。 因此，可以做无索引排序是有上限的，那就是不可能在内存中作T级别的数据的排序。 一旦集合大到不能在内存中排序，MongoDB就会报错。 按照顺序来索引，以便让MongoDB按照顺序提取数据，这样就能排序大规模数据了。

### 索引的名字

集合中每个索引都有一个字符串类型的名字，来唯一标识索引。 服务器通过名字来操作或者删除索引。 要注意的是，索引名有字符个数限制，所以索引创建时一定要用自定义的名字，如
```
db.user.ensureIndex({"username":1,.......,"date":1},{"name":"XXXX"})
```
一旦创建索引不成功，可以用getLastError来查看错误提示。

### 唯一索引

唯一索引可以确保集合的每一个文档的指定键都有唯一值。
```
db.user.ensureIndex({"username":1},{"uniquer":true})
```
#### 注意

insert不会检查文档是否插入过了，所以为了避免插入的文档中有与唯一键重复的值，要用安全插入才能满足。

其实_id 是我们最熟悉的唯一索引，它在集合创建时产生，只不过它不能删除！

### 复合唯一索引

只要满足索引的其中之一不同即可
```
db.blog.ensureIndex({"username":1,"blogname":1})
```
作者和作品名其中之一不同即可创建复合唯一索引。

### explain() hint()

explain()可以返回查询使用的索引情况
hint()可以强制cursor使用某索引 但是通常MongoDB 的查询优化器非常智能，会替你选择改用哪一个索引。
地理空间索引

现在有一种查询变得越来越流行（尤其是移动设备）：找到离当前位置最近的N个场所。 MongoDB专为平面坐标查询做了专门的索引，称为地理空间索引。

同样需要用ensureIndex创建，不过，参数是两个 "2d"
```
db.map.ensureIndex({"gps":"2d"})
```
gps键的值必须是某种形式的一对值：一个包含两个元素的数组或者包含两个键的内嵌文档，像这样
```
{"gps":[0,100]}
{"gps":{"x":-30,"y":30}}
```
还可以用min和max设定选项的最大值和最小值

现在我们可以查询
```
db.map.find({"gps":{"$near":[40,-73]}})
```
这会按照点（40，-73）由近及远的方式将map集合的所有文档都返回

不仅如此，还可以指定形状，比如矩形
```
db.map.find({"gps":{"$within":{"$box":[ [10,20],[15,30] ]}}})
$box是两个元素的数组，第一个元素指左下方，第二个指右上角。 还可以用它来找到园内的点

db.map.find({"gps":{"$within":{"$center":[[10,30],5],}}})
```

###聚合管道###

注明：本篇的所有例子来自MongoDB官网示例。
####功能####

聚合管道的功能简单来说就分两种：

对文档进行“过滤”，也就是筛选出符合条件的文档;
对文档进行“变换”，也就是改变文档的输出形式。
####$project#### 
1.我们有这样的数据
```
  {
     "_id" : 1,
     title: "abc123",
     isbn: "0001122223334",>
     author: { last: "zzz", first: "aaa" },
     copies: 5
  }
```
现在使用project来变换输出
```
 db.books.aggregate( 
     [
          { $project : { title : 1 , author : 1 } } 
     ]
 )
```
可以得
```
 { 
    "_id" : 1,
    "title" : "abc123", 
    "author" : { "last" : "zzz", "first" : "aaa" } 
 }

在$project里，我们指明（筛选）了要显示的数据，title和author，_id是自带的，可以用 _id：0 来将其过滤掉
```
2.我们现在有基础数据
```
 {
     "_id" : 1,
     title: "abc123",
     isbn: "0001122223334",
     author: { last: "zzz", first: "aaa" },
     copies: 5
 }
```
但是我们需要变换他的输出形式，我们就可以这样
```
db.books.aggregate(
 [
   {
      $project: {
         title: 1,
         isbn: {
            prefix: { $substr: [ "$isbn", 0, 3 ] },
            group: { $substr: [ "$isbn", 3, 2 ] },
            publisher: { $substr: [ "$isbn", 5, 4 ] },
            title: { $substr: [ "$isbn", 9, 3 ] },
            checkDigit: { $substr: [ "$isbn", 12, 1] }
         },
         lastName: "$author.last",
         copiesSold: "$copies"
      }
   }
 ]
 )
```
在isbn内部的键 prefix,group,publisher,title，checkDigit,外部的lastName,copiesSold都是我们自己定义的。 $substr取字串，$isbn是字串键名，第二参数是字串起始位置，第三参数是取几个。

最后结果
```
 {
  "_id" : 1,
  "title" : "abc123",
  "isbn" : {
      "prefix" : "000",
      "group" : "11",
      "publisher" : "2222",
      "title" : "333",
      "checkDigit" : "4"
  },
  "lastName" : "zzz",
  "copiesSold" : 5
 }
 ```
数据源没有变，但是我们改变的数据显示的方式。

####$match####
 过滤数据，过滤完的数据，接下来用作其他用。 

1.例子
```
  db.articles.aggregate(
     [
          { $match : { author : "dave" } }  
     ]
  );
```
过滤条件为键 author 值为 dave

结果为
```
 {
  "result" : [
               {
                 "_id" : ObjectId("512bc95fe835e68f199c8686"),
                 "author": "dave",
                 "score" : 80
               },
               { "_id" : ObjectId("512bc962e835e68f199c8687"),
                  "author" : "dave",
                  "score" : 85
               }
            ],
  "ok" : 1
 }
```
2.再看一例
```
 db.articles.aggregate(    
 [                   
               { $match : { score : { $gt : 70, $lte : 90 } } },      
               { $group: { _id: null, count: { $sum: 1 } } }
 ] 
 );
```
这次有两步：

第一步，过滤 键 score 值 大于70 且 小于等于90 的文档，
再用group 对文档用 count 统计，统计方式 $sum 求和，步长为1。
因为group操作必须有个_id,所以给其置null。
结果为
```
 {
  "result" : [
               {
                 "_id" : null,
                 "count" : 3
               }
             ],
  "ok" : 1 
 }
 ```
####$cond#### 
判断用的，可以跟if then 语句

1.举例
```
{ "_id" : 1, "item" : "abc1", qty: 300 }

{ "_id" : 2, "item" : "abc2", qty: 200 }

{ "_id" : 3, "item" : "xyz1", qty: 250 }
```
现在我们想根据qty的值来生成新的数据（值）

```
db.inventory.aggregate( 
[
  {
     $project:
       {
         item: 1,
         discount:
           {
             $cond: { if: { $gte: [ "$qty", 250 ] }, then: 30, else: 20 }
           }
       }
  }
 ]
 )
 ```
结果为
```
 { "_id" : 1, "item" : "abc1", "discount" : 30 }
 { "_id" : 2, "item" : "abc2", "discount" : 20 }
 { "_id" : 3, "item" : "xyz1", "discount" : 30 }
 ```
可以发现，discount是我们新的键，它根据cond的if判断后，分别被赋上了相应的值（then和else可以省略）

####$limit#### 
限制个数

1.例子
```
db.article.aggregate(
    { $limit : 5 }
);
```
值得注意的是，当聚合操作中同时出现sort和limit， sort只会对通过limit的数据排序，内存中也仅会存储通过limit的数据。

####$skip#### 
略过N个

1.例子
```
db.article.aggregate(
   { $skip : 5 }
);
```
####$unwind#### 
拆解数组集合

1.例子
```
 { 
      "_id" : 1, 
      "item" : "ABC1", 
      sizes: [ "S", "M", "L"] 
 }
 ```
现在对sizes进行拆解
```
 db.inventory.aggregate( 
    [ 
        { $unwind : "$sizes" }
    ] 
 )
```
结果
```
 { "_id" : 1, "item" : "ABC1", "sizes" : "S" }
 { "_id" : 1, "item" : "ABC1", "sizes" : "M" }
 { "_id" : 1, "item" : "ABC1", "sizes" : "L" }
```
我们可以看到sizes里每一个数据被拆解到每一个文档里了，除了sizes 的值不同外，其他相同。

$unwind与$group组合可以实现distinct

####$group#### 
先分组，再合并

1.例子
```
{ "_id" : { "month" : 3, "day" : 15, "year" : 2014 }, 
      "totalPrice" : 50, "averageQuantity" : 10, "count" : 1 }

{ "_id" : { "month" : 4, "day" : 4, "year" : 2014 }, 
      "totalPrice" : 200, "averageQuantity" : 15, "count" : 2 }

{ "_id" : { "month" : 3, "day" : 1, "year" : 2014 }, 
      "totalPrice" : 40, "averageQuantity" : 1.5, "count" : 2 }

_id 为分组依据，_id 为null，及不分组，直接合并。
```
#### 合并依据：

* 键 totalPrice 保存 键 price 和 键 quantity 值 的乘积 的和

* 键averageQuantity 保存 键 quantity 的值的平均值

* 键 count 作统计
```
db.sales.aggregate( [ { $group : { _id : null, totalPrice: { $sum: { $multiply: [ "$price", "$quantity" ] } }, averageQuantity: { $avg: "$quantity" }, count: { $sum: 1 } } } ] )
```
结果
```
 { "_id" : null, "totalPrice" : 290, "averageQuantity" : 8.6, "count" : 5 }
```
2.再看一例
```
 { "_id" : 1, "item" : "abc", "price" : 10, "quantity" : 2, 
            "date" : ISODate("2014-03-01T08:00:00Z") }

 { "_id" : 2, "item" : "jkl", "price" : 20, "quantity" : 1, 
            "date" : ISODate("2014-03-01T09:00:00Z") }

 { "_id" : 3, "item" : "xyz", "price" : 5, "quantity" : 10, 
            "date" : ISODate("2014-03-15T09:00:00Z") }

 { "_id" : 4, "item" : "xyz", "price" : 5, "quantity" : 20, 
            "date" : ISODate("2014-04-04T11:21:39.736Z") }

 { "_id" : 5, "item" : "abc", "price" : 10, "quantity" : 10, 
            "date" : ISODate("2014-04-04T21:23:13.331Z") }
            ```
咱们依据 键 item 分组
```
  db.sales.aggregate( [ { $group : { _id : "$item" } } ] )
  ```
结果
```
 { "_id" : "xyz" }
 { "_id" : "jkl" }
 { "_id" : "abc" }
 ```

####$sort####
 排序

1.例子
```
db.users.aggregate(
  [
     { $sort : { age : -1, posts: 1 } }
  ]
)
```
对键age 顺序排序，对键 posts 逆序排序

####$group#### 
先分组，再合并

1.例子
```
{ "_id" : { "month" : 3, "day" : 15, "year" : 2014 }, 
      "totalPrice" : 50, "averageQuantity" : 10, "count" : 1 }

{ "_id" : { "month" : 4, "day" : 4, "year" : 2014 }, 
      "totalPrice" : 200, "averageQuantity" : 15, "count" : 2 }

{ "_id" : { "month" : 3, "day" : 1, "year" : 2014 }, 
      "totalPrice" : 40, "averageQuantity" : 1.5, "count" : 2 }

_id 为分组依据，_id 为null，及不分组，直接合并。
  ```
#### 合并依据：

* 键 totalPrice 保存 键 price 和 键 quantity 值 的乘积 的和

* 键averageQuantity 保存 键 quantity 的值的平均值

* 键 count 作统计
```
db.sales.aggregate( [ { $group : { _id : null, totalPrice: { $sum: { $multiply: [ "$price", "$quantity" ] } }, averageQuantity: { $avg: "$quantity" }, count: { $sum: 1 } } } ] )
```
结果
```
 { "_id" : null, "totalPrice" : 290, "averageQuantity" : 8.6, "count" : 5 }
```
2.再看一例
```
 { "_id" : 1, "item" : "abc", "price" : 10, "quantity" : 2, 
            "date" : ISODate("2014-03-01T08:00:00Z") }

 { "_id" : 2, "item" : "jkl", "price" : 20, "quantity" : 1, 
            "date" : ISODate("2014-03-01T09:00:00Z") }

 { "_id" : 3, "item" : "xyz", "price" : 5, "quantity" : 10, 
            "date" : ISODate("2014-03-15T09:00:00Z") }

 { "_id" : 4, "item" : "xyz", "price" : 5, "quantity" : 20, 
            "date" : ISODate("2014-04-04T11:21:39.736Z") }

 { "_id" : 5, "item" : "abc", "price" : 10, "quantity" : 10, 
            "date" : ISODate("2014-04-04T21:23:13.331Z") }
```
咱们依据 键 item 分组
```
  db.sales.aggregate( [ { $group : { _id : "$item" } } ] )
  ```
结果
```
 { "_id" : "xyz" }
 { "_id" : "jkl" }
 { "_id" : "abc" }
 ```

####$sort####
 排序

1.例子
```
db.users.aggregate(
  [
     { $sort : { age : -1, posts: 1 } }
  ]
)
```
对键age 顺序排序，对键 posts 逆序排序

####$out#### 
创建指定副本集合

1.例子
```
   { "_id" : 8751, "title" : "The Banquet", "author" : "Dante", "copies" : 2 }

   { "_id" : 8752, "title" : "Divine Comedy", "author" : "Dante", "copies" : 1 }

   { "_id" : 8645, "title" : "Eclogues", "author" : "Dante", "copies" : 2 }

   { "_id" : 7000, "title" : "The Odyssey", "author" : "Homer", "copies" : 10 }

   { "_id" : 7020, "title" : "Iliad", "author" : "Homer", "copies" : 10 }
   ```
对其按author分组，然后out一个新集合authors
```
  db.books.aggregate( [
                  { $group : { _id : "$author", books: { $push: "$title" } } },
                  { $out : "authors" }
  ] )
结果

 { "_id" : "Homer", "books" : [ "The Odyssey", "Iliad" ] }
 { "_id" : "Dante", "books" : [ "The Banquet", "Divine Comedy", "Eclogues" ] }
 ```
我们现在看到的只是数据映射，不是实体文档，

但是在authors里，有映射副本存成的文档。

也就是说 $out 可以创建新的集合，存储聚合后的文档映射。

### 布尔操作符

注明：本篇所有示例均来自MongoDB官方文档提供示例
####$and#### 
判断依据：

* true：判断一个或多个表达式，如果所有表达式全为真 或 没有表达式，返回 true
* false：其他情况返回 false
1.例子
```
    Example	 	                                   Result
    { $and: [ 1, "green" ] }	 	               true                   
    { $and: [ ] }	 	                           true                   
    { $and: [ [ null ], [ false ], [ 0 ] ] }       true                   
    { $and: [ null, true ] }	 	               false                  
    { $and: [ 0, true ] }	 	                   false    
    ```              
__注意__

如果发现有 null、0、undefined，$and就会判断为false，但如果他们在数组中，则反之。
__解释：__

有多个表达式
无表达式
有多个表达式（虽然有0和null，但是0和null在数组中，视为表达式）
有多个表达式，但其中有null
有多个表达式，但其中还有0
2.例子
```
  { "_id" : 1, "item" : "abc1", description: "product 1", qty: 300 }

  { "_id" : 2, "item" : "abc2", description: "product 2", qty: 200 }

  { "_id" : 3, "item" : "xyz1", description: "product 3", qty: 250 }

  { "_id" : 4, "item" : "VWZ1", description: "product 4", qty: 300 }

  { "_id" : 5, "item" : "VWZ2", description: "product 5", qty: 180 }
  ```
按要求对 键qty值大于100 和 键qty值小于250 进行$and 操作，再赋给新键result
```
 db.inventory.aggregate([{
   $project:
      {
        item: 1,
        qty: 1,
        result: { $and: [ { $gt: [ "$qty", 100 ] }, { $lt: [ "$qty", 250 ] } ] }
      }
   }])
```
结果
```
 { "_id" : 1, "item" : "abc1", "result" : false }
 { "_id" : 2, "item" : "abc2", "result" : true }
 { "_id" : 3, "item" : "xyz1", "result" : false }
 { "_id" : 4, "item" : "VWZ1", "result" : false }
 { "_id" : 5, "item" : "VWZ2", "result" : true }
```
因为200 和 180 是介于100和250之间的，所以他们的键result的值为true

####$or####
 判断依据

true：对于一个或多个表达式，只要任意表达式的值为true，返回true
false：其他情况，返回false
1.例子
```
  Example	 	                     Result
  { $or: [ true, false ] }	 	      true
  { $or: [ [ false ], false ] }	 	  true
  { $or: [ null, 0, undefined ] }	  false
  { $or: [ ] }	 	                  false
  ```
__注意__

如果发现有 null、0、undefined，$and就会判断为false，但如果他们在数组中，则反之。
__解释__

有多个表达式
有多个表达式
有三个表达式，但其中有 null、0、undefined
没有表达式
2.例子
```
  { "_id" : 1, "item" : "abc1", description: "product 1", qty: 300 }

  { "_id" : 2, "item" : "abc2", description: "product 2", qty: 200 }

  { "_id" : 3, "item" : "xyz1", description: "product 3", qty: 250 }

  { "_id" : 4, "item" : "VWZ1", description: "product 4", qty: 300 }

  { "_id" : 5, "item" : "VWZ2", description: "product 5", qty: 180 }
```
按要求对 键qty值大于250 或 键qty值小于200 进行$or 操作，再赋给新键result
```
   db.inventory.aggregate([{
     $project:
        {
           item: 1,
           result: { $or: [ { $gt: [ "$qty", 250 ] }, { $lt: [ "$qty", 200 ] } ] }
        }
   }])
```
结果
```
 { "_id" : 1, "item" : "abc1", "result" : true }
 { "_id" : 2, "item" : "abc2", "result" : false }
 { "_id" : 3, "item" : "xyz1", "result" : false }
 { "_id" : 4, "item" : "VWZ1", "result" : true }
 { "_id" : 5, "item" : "VWZ2", "result" : true }
```
因为200 和 250 是不再范围之外的，所以他们的键result的值为false

####$not####
 判断布尔值，然后置反
```
 Example	 	                Result
 { $not: [ true ] }	 	        false
 { $not: [ [ false ] ] }	 	false
 { $not: [ false ] }	      	true
 { $not: [ null ] }	 	        true
 { $not: [ 0 ] }	 	        true
```
__注意__

如果发现有 null、0、undefined，$and就会判断为false，但如果他们在数组中，则反之。
__解释__

一个表达式，值为true，置反为false
一个表达式，[false]布尔值为true，置反为false
一个表达式，值为false，置反为true
一个表达式，值为false，置反为true
一个表达式，值为false，置反为true
2.例子
```
 { "_id" : 1, "item" : "abc1", description: "product 1", qty: 300 }

 { "_id" : 2, "item" : "abc2", description: "product 2", qty: 200 }

 { "_id" : 3, "item" : "xyz1", description: "product 3", qty: 250 }

 { "_id" : 4, "item" : "VWZ1", description: "product 4", qty: 300 }

 { "_id" : 5, "item" : "VWZ2", description: "product 5", qty: 180 }
```
对大于250的值置反
```
db.inventory.aggregate([{
   $project:
      {
        item: 1,
        result: { $not: [ { $gt: [ "$qty", 250 ] } ] }
      }
 }])
```
结果
```
{ "_id" : 1, "item" : "abc1", "result" : false }
{ "_id" : 2, "item" : "abc2", "result" : true }
{ "_id" : 3, "item" : "xyz1", "result" : true }
{ "_id" : 4, "item" : "VWZ1", "result" : false }
{ "_id" : 5, "item" : "VWZ2", "result" : true }
300大于250，置反后result为false
```
### 集合操作符

####$setEquals#### 
检查是否有重复的值

> 判断条件 有重复值，返回true，否则，返回false

1.例子
```
  Example	 	                                            Result
  { $setEquals: [ [ "a", "b", "a" ], [ "b", "a" ] ] }	 	true
  { $setEquals: [ [ "a", "b" ], [ [ "a", "b" ] ] ] }	 	false
```
2.例子
```
       { "_id" : 1, "A" : [ "red", "blue" ], "B" : [ "red", "blue" ] }
       { "_id" : 2, "A" : [ "red", "blue" ], "B" : [ "blue", "red", "blue" ] }
       { "_id" : 3, "A" : [ "red", "blue" ], "B" : [ "red", "blue", "green" ] }
       { "_id" : 4, "A" : [ "red", "blue" ], "B" : [ "green", "red" ] }
       { "_id" : 5, "A" : [ "red", "blue" ], "B" : [ ] }
       { "_id" : 6, "A" : [ "red", "blue" ], "B" : [ [ "red" ], [ "blue" ] ] }
       { "_id" : 7, "A" : [ "red", "blue" ], "B" : [ [ "red", "blue" ] ] }
       { "_id" : 8, "A" : [ ], "B" : [ ] }
       { "_id" : 9, "A" : [ ], "B" : [ "red" ] }
```
对键A和键B的值进行查重布尔判断
```
 db.experiments.aggregate(
    [
         { $project: { A: 1, B: 1, sameElements: { $setEquals: [ "$A", "$B" ] }, _id: 0 } }
    ]
 )
```
结果
```
    { "A" : [ "red", "blue" ], "B" : [ "red", "blue" ], "sameElements" : true }

    { "A" : [ "red", "blue" ], "B" : [ "blue", "red", "blue" ], "sameElements" : true }

    { "A" : [ "red", "blue" ], "B" : [ "red", "blue", "green" ], "sameElements" : false }

    { "A" : [ "red", "blue" ], "B" : [ "green", "red" ], "sameElements" : false }

    { "A" : [ "red", "blue" ], "B" : [ ], "sameElements" : false }

    { "A" : [ "red", "blue" ], "B" : [ [ "red" ], [ "blue" ] ], "sameElements" : false }

    { "A" : [ "red", "blue" ], "B" : [ [ "red", "blue" ] ], "sameElements" : false }

    { "A" : [ ], "B" : [ ], "sameElements" : true }

    { "A" : [ ], "B" : [ "red" ], "sameElements" : false }
```
注意，空值和空值可以视为值相同

####$setIntersection#### 
获取交集，忽略重复，忽略顺序

> 判断条件：对一个或多个数组判断，返回依次出现在每个数组中的值，简单说就是取交集

1.例子
```
  Example	 	                                                Result
  { $setIntersection: [ [ "a", "b", "a" ], [ "b", "a" ] ] }	 	[ "b", "a" ]
  { $setIntersection: [ [ "a", "b" ], [ [ "a", "b" ] ] ] }	 	[ ]
```
2.例子
```
  { "_id" : 1, "A" : [ "red", "blue" ], "B" : [ "red", "blue" ] }
  { "_id" : 2, "A" : [ "red", "blue" ], "B" : [ "blue", "red", "blue" ] }
  { "_id" : 3, "A" : [ "red", "blue" ], "B" : [ "red", "blue", "green" ] }
  { "_id" : 4, "A" : [ "red", "blue" ], "B" : [ "green", "red" ] }
  { "_id" : 5, "A" : [ "red", "blue" ], "B" : [ ] }
  { "_id" : 6, "A" : [ "red", "blue" ], "B" : [ [ "red" ], [ "blue" ] ] }
  { "_id" : 7, "A" : [ "red", "blue" ], "B" : [ [ "red", "blue" ] ] }
  { "_id" : 8, "A" : [ ], "B" : [ ] }
  { "_id" : 9, "A" : [ ], "B" : [ "red" ] }
```
对键A和键B 取交集
```
 db.experiments.aggregate(
   [
       { $project: { A: 1, B: 1, commonToBoth: { $setIntersection: [ "$A", "$B" ] }, _id: 0 } }
   ]
 )
```
结果
```
  { "A" : [ "red", "blue" ], "B" : [ "red", "blue" ], "commonToBoth" : [ "blue", "red" ] }
  { "A" : [ "red", "blue" ], "B" : [ "blue", "red", "blue" ], "commonToBoth" : [ "blue", "red" ] }
  { "A" : [ "red", "blue" ], "B" : [ "red", "blue", "green" ], "commonToBoth" : [ "blue", "red" ] }
  { "A" : [ "red", "blue" ], "B" : [ "green", "red" ], "commonToBoth" : [ "red" ] }
  { "A" : [ "red", "blue" ], "B" : [ ], "commonToBoth" : [ ] }
  { "A" : [ "red", "blue" ], "B" : [ [ "red" ], [ "blue" ] ], "commonToBoth" : [ ] }
  { "A" : [ "red", "blue" ], "B" : [ [ "red", "blue" ] ], "commonToBoth" : [ ] }
  { "A" : [ ], "B" : [ ], "commonToBoth" : [ ] }
  { "A" : [ ], "B" : [ "red" ], "commonToBoth" : [ ] }
```
__注意：__

$setIntersection操作忽略重复、忽略顺序
空集和任何相交为空集
####$setUnion#### 
取并集，忽略重复，忽略顺序

1.例子
```
  Example	 	                                                Result
  { $setUnion: [ [ "a", "b", "a" ], [ "b", "a" ] ] }	 	    [ "b", "a" ]
  { $setUnion: [ [ "a", "b" ], [ [ "a", "b" ] ] ] }	 	        [ [ "a", "b" ], "b", "a" ]
```
2.例子
```
  { "_id" : 1, "A" : [ "red", "blue" ], "B" : [ "red", "blue" ] }
  { "_id" : 2, "A" : [ "red", "blue" ], "B" : [ "blue", "red", "blue" ] }
  { "_id" : 3, "A" : [ "red", "blue" ], "B" : [ "red", "blue", "green" ] }
  { "_id" : 4, "A" : [ "red", "blue" ], "B" : [ "green", "red" ] }
  { "_id" : 5, "A" : [ "red", "blue" ], "B" : [ ] }
  { "_id" : 6, "A" : [ "red", "blue" ], "B" : [ [ "red" ], [ "blue" ] ] }
  { "_id" : 7, "A" : [ "red", "blue" ], "B" : [ [ "red", "blue" ] ] }
  { "_id" : 8, "A" : [ ], "B" : [ ] }
  { "_id" : 9, "A" : [ ], "B" : [ "red" ] }
```
我们对键A和键B 取并集
```
  db.experiments.aggregate(
     [
        { $project: { A:1, B: 1, allValues: { $setUnion: [ "$A", "$B" ] }, _id: 0 } }
     ]
  )
```
结果
```
   { "A": [ "red", "blue" ], "B": [ "red", "blue" ], "allValues": [ "blue", "red" ] }
   { "A": [ "red", "blue" ], "B": [ "blue", "red", "blue" ], "allValues": [ "blue", "red" ] }
   { "A": [ "red", "blue" ], "B": [ "red", "blue", "green" ], "allValues": [ "blue", "red", "green" ] }
   { "A": [ "red", "blue" ], "B": [ "green", "red" ], "allValues": [ "blue", "red", "green" ] }
   { "A": [ "red", "blue" ], "B": [ ], "allValues": [ "blue", "red" ] }
   { "A": [ "red", "blue" ], "B": [ [ "red" ], [ "blue" ] ], "allValues": [ "blue", "red", [ "red" ], [ "blue" ] ] }
   { "A": [ "red", "blue" ], "B": [ [ "red", "blue" ] ], "allValues": [ "blue", "red", [ "red", "blue" ] ] }
   { "A": [ ], "B": [ ], "allValues": [ ] }
   { "A": [ ], "B": [ "red" ], "allValues": [ "red" ] }
```
注意：

$setUnion忽略重复、忽略顺序
空集和其他集合去并集为其他集合
####$setDifference#### 
找不同，忽略重复，忽略顺序 判断依据：
* 只看后者比前者多了什么
* 忽略后者比前者少了什么
1. 例子
```
   Example	 	                                                Result
   { $setDifference: [ [ "a", "b", "a" ], [ "b", "a" ] ] }	 	[ ]
   { $setDifference: [ [ "a", "b" ], [ [ "a", "b" ] ] ] }	 	[ "a", "b" ]
```
__解释：__

* 忽略重复、顺序后，后者跟前者一样，所以空集
* 忽略重复、顺序后，后者比前者多了一个子集合，所以是那个多出来的子集合
2. 例子
```
  { "_id" : 1, "A" : [ "red", "blue" ], "B" : [ "red", "blue" ] }
  { "_id" : 2, "A" : [ "red", "blue" ], "B" : [ "blue", "red", "blue" ] }
  { "_id" : 3, "A" : [ "red", "blue" ], "B" : [ "red", "blue", "green" ] }
  { "_id" : 4, "A" : [ "red", "blue" ], "B" : [ "green", "red" ] }
  { "_id" : 5, "A" : [ "red", "blue" ], "B" : [ ] }
  { "_id" : 6, "A" : [ "red", "blue" ], "B" : [ [ "red" ], [ "blue" ] ] }
  { "_id" : 7, "A" : [ "red", "blue" ], "B" : [ [ "red", "blue" ] ] }
  { "_id" : 8, "A" : [ ], "B" : [ ] }
  { "_id" : 9, "A" : [ ], "B" : [ "red" ] }
```
我们用键B比较键A，找出多出来的部分
```
 db.experiments.aggregate(
    [
      { $project: { A: 1, B: 1, inBOnly: { $setDifference: [ "$B", "$A" ] }, _id: 0 } }
    ]
 )
```
结果
```
 { "A" : [ "red", "blue" ], "B" : [ "red", "blue" ], "inBOnly" : [ ] }
 { "A" : [ "red", "blue" ], "B" : [ "blue", "red", "blue" ], "inBOnly" : [ ] }
 { "A" : [ "red", "blue" ], "B" : [ "red", "blue", "green" ], "inBOnly" : [ "green" ] }
 { "A" : [ "red", "blue" ], "B" : [ "green", "red" ], "inBOnly" : [ "green" ] }
 { "A" : [ "red", "blue" ], "B" : [ ], "inBOnly" : [ ] }
 { "A" : [ "red", "blue" ], "B" : [ [ "red" ], [ "blue" ] ], "inBOnly" : [ [ "red" ], [ "blue" ] ] }
 { "A" : [ "red", "blue" ], "B" : [ [ "red", "blue" ] ], "inBOnly" : [ [ "red", "blue" ] ] }
 { "A" : [ ], "B" : [ ], "inBOnly" : [ ] }
 { "A" : [ ], "B" : [ "red" ], "inBOnly" : [ "red" ] }
```
__注意__

* 忽略顺序、重复
* 我们只关心多了什么，不关心少了什么