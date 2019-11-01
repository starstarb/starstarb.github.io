---
title: 'java的源码及应用之: 单元测试及多模块依赖将如何进行单元测试'
copyright: true
abbrlink: b9df760
date: 2019-10-02 12:58:07
tags: java
categories: java
---
单元测试
<!--more-->
##### 单元测试目的
保证代码正确性，完整记录问题的所在和缺陷以及正确的现状，方便进行代码的修复，重构和改进。

##### 单元测试做什么
1. 接口功能性测试：接口功能的正确性,即保证接口能够被正常调用，并输出有效数据!
* 是否被顺利调用
* 参数是否符合预期
2. 局部数据结构测试：保证数据结构的正确性
* 变量是否有初始值或在某场景下是否有默认值
* 变量是否溢出
3. 边界条件测试：测试
* 变量无赋值(null)
* 变量是数值或字符
* 主要边界：最大值，最小值，无穷大
* 溢出边界：在边界外面取值+/-1
* 临近边界：在边界值之内取值+/-1
* 字符串的边界，引用 "变量字符"的边界
* 字符串的设置，空字符串
* 字符串的应用长度测试
* 空白集合
* 目标集合的类型和应用边界
* 集合的次序
* 变量是规律的，测试无穷大的极限，无穷小的极限

4. 所有独立代码测试：保证每一句代码，所有分支都测试完成，主要包括代码覆盖率，异常处理通路测试
* 语句覆盖率：每个语句都执行到了
* 判定覆盖率：每个分支都执行到了
* 条件覆盖率：每个条件都返回布尔
* 路径覆盖率：每个路径都覆盖到了

5. 异常模块测试，后续处理模块测试:是否包闭当前异常或者对异常形成消化,是否影响结果!


##### JUNIT4
(1)：业务流程的一般是按照需求的预期效果，跑完整个业务流程，包括以前开发的流程
* 是否实现了预期
* 是否影响到了以前的流程
* 全流程是否顺利
* 数据是否符合预期

(2)：代码测试：

```
@BeforeClass 全局只会执行一次，而且是第一个运行
@Before 在测试方法运行之前运行
@Test 测试方法
@After 在测试方法运行之后允许
@AfterClass 全局只会执行一次，而且是最后一个运行
@Ignore 忽略此方法
```
JUNIT4是以org.junit为框架进行的测试，以注解的形式来识别代码中需要测试的方法!

**注意：**

对于每一个测试，我们都应该保持独立测试，以确保测试结果是有意义的。在程序中，经常会出现，当测试完一个方法后，其参数已经被系统保持或持久化下来。无疑会造成下一次的测试测试数据或者状态的不合理性!为了解决问题，对于此类场景，我们的测试代码必须具备初始化和收尾的能力。也即是@Before和@After的意义所在!

同理@AfterClass和BeforeClass即是为了满足测试中，那些体积非常大，但只要一次初始化的代码块!

(3)：断言测试与及常用断言：

   * assertEquals
```
Assert.assertEquals("此处输出提示语", 5, result);
 
===================================================================================================================
解析:"此处输出提示语" 为错误时你个人想要输出的错误信息; 5  是指你期望的值；result 是指你调用程序后程序输出给你的结果
 
@Test(expected = NullPointerException.class)  
解析:在注解的时候添加expected 为忽略此异常
@Test(timeout  =   5000 ):超时设置
@Test(expected  =  XXXXException. class)：期望出现异常，如果出现该异常则成功，否则测试失败
@Ignore() ：用户方法之上，被注解的方法会被成功需忽略
===================================================================================================================
 
fail("Not yet implemented")
解析:放在方法中，如果我顺利地执行，我就报失败出来。就是说按道理不应该执行到这里的，但是偏偏执行了,说明程序有问题
===================================================================================================================
Assert.assertTrue("msg",boolean)与Assert.assertFalse("msg",boolean)
解析:如果和预期一样为true则成功，否则失败输出msg;如果和预期一样为false则成功，否则失败并输出
```

**断言列表:**
* assertTrue(String message, boolean condition)             要求condition == true
* assertFalse(String message, boolean condition)            要求condition == false
* assertEquals(String message, XXX expected,XXX actual) 要求expected期望的值能够等于actual
* assertArrayEquals(String message, XXX[] expecteds,XXX [] actuals) 要求expected.equalsArray(actual)
* assertNotNull(String message, Object object)          要求object!=null
* assertNull(String message, Object object)             要求object==null
* assertSame(String message, Object expected, Object actual)     要求expected == actual
* assertNotSame(String message, Object unexpected,Object actual) 要求expected != actual
* assertThat(String reason, T actual, Matcher matcher)  要求matcher.matches(actual) == true

* fail(String message) 要求执行的目标结构必然失败，同样要求代码不可达,即是这个方法在程序运行后不会成功返回，如果成功返回了则报错




参考：https://blog.csdn.net/qq_36505948/article/details/82797240
实例：https://blog.csdn.net/qingtian_1993/article/details/79491534




