---
title: js学习计划第一天
categories: 前端
tags: js
abbrlink: '62e93227'
date: 2019-07-07 14:23:43
password: 753159
message: Welcome to my blog, enter password to read.  
abstract: Welcome to my blog, enter password to read.  
---
JS代码学习基础
<!--more-->

JavaScript是一种解释型语言，不是编译型语言，

计算机不能理解js代码，需要解释js代码，并将其转换成计算机能理解的机器码，如此，计算机才能识别代码。

当浏览器遇到js代码时，将js代码传递给一个称为“解释器”的程序，解释器将javaScript代码转换为计算机能理解的机器码。

javascript的转换在代码__运行__时进行，每次运行都需要重复地进行转换。
而编译型语言则是在编译时转换，且只转换一次。

JavaScript不能使用XML的自闭语法。


CDN（Content Delivery NetWorks）内容分发网络，保存javascript库的服务器。

使用外部文件的优点：
* 促进代码的重用
* 浏览器会缓存他们，节省下载时间，减少占用带宽
```
document.bgColor="red";

<p id="results"></p>
<script>
docment.getElementById("results").innerHTML="Hello World!";
//提取id为results的文档元素，并把该元素的HTML设置为HelloWorld！
</script>
```
alert()函数，显示一个消息框，以便向用户发送通知或警告信息。
在alert()函数中的括号中指定消息框的消息，它成为函数的参数。（parameter）

javaScript是一种弱类型语言，对如何使用不同类型的数据并没有严格的要求，js会自动确定它们的类型。

将文本放在引号（“”）中，js会将其识别为文本，而不是代码。

常用转义序列：
\b 退格字符
\' 单引号
\"  双引号
\\ 反斜线

## JS基础算法总结
### 1、判断一个单词是否是回文 
题目：如果给定的字符串是回文，返回true，反之，返回false。如果一个字符串忽略标点符号、大小写和空格，正着读和反着读一模一样，那么这个字符串就是palindrome(回文)。

注意你需要去掉字符串多余的标点符号和空格，然后把字符串转化成小写来验证此字符串是否为回文。

函数参数的值可以为”racecar”，”RaceCar”和”race CAR”。 
主要使用：split();reverse();join()。
```
Answer：

function palindrome(str) {
  // 转换成小写用正则过滤掉符号
  var newStr = str.toLowerCase().replace(/[^a-z0-9]/g,"");
  //验证反转后是否相等
  return newStr.split("").reverse().join("") === newStr;
}
palindrome("eye");
如果要求不忽略大小写符号等，更简单：

function palindrome(str){
    return str == str.split('').reverse().join('');
}
palindrome("Rar");
```

### 2、去掉一个整型数组中的重复值 
去掉一个整型数组中的重复值

比如输入: [1,13,24,11,11,14,1,2]

输出: [1,13,24,11,14,2]

需要去掉重复的11 和 1 这两个元素。

主要考察个人对Object的使用，利用key来进行筛选;indexOf()。 
```
Answer

var student = ['qiang','ming','tao','li','liang','you','qiang','you','qiang','tao'];

function unique(arr){
    var temp = new Array();
    for(var i in arr){
        if(temp.indexOf(arr[i]) == -1{
        //indexOf() 方法可返回某个指定的字符串值在字符串中首次出现的位置。
            temp.push(arr[i]);
        }
    }
    return temp;
}
unique(student);
```
#### 3、翻转字符串&阶乘 
```
Answer：

//翻转字符串
function reverseString(str){
    str = str.split('').reverse().join('');
}
reverseString('hello');
//阶乘
function factorial(num){
    if(num <= 0){
        return 1;
    }else{
        return num*arguments.callee(num-1);
    }
}
factorial(5);
这段代码在W3c中并不能完美执行，但可以在浏览器中运行，原因是：argument.callee不是一个标准函数。

使用函数arguments.callee 函数解耦 
arguments有一个名叫callee属性，该属性是一个指针，指向拥有arguments对象的函数。 
原函数的执行与函数factorialize紧紧耦合到了一起，这不优雅，但可以使用。
下面这段代码可以完美运行：

function factorial(num) {
  if (num>0){
    return (num * factorial(num - 1));}
  else
  return (1);
}
factorial(5);
```
#### 4、寻找字符串中最长的并输出其长度

本来想的将数组中的每一个元素转换为一个新的数组，再将数组排序，取最长的字符串，将这个长度值返回。 
最后用了for循环： 
```
Answer：

function findLongestWord(arr){
    var Array = arr.split(' ');
    var longest = 0;
    for(var i = 0;i < Array.length;i++){
        if(Array[i].length > longest){
            longest = Array[i].length;
        }
    }
    return longest;
}
findLongestWord("The quick brown fox jumped over the lazy dog");
```
#### 5、确保字符串的每个单词首字母都大写，其余部分小写

思路：先将所有的字母都转换为小写，转换为数组，再将数组中字符首字母设置大写，再将其转换为字符串。
``` 
Answer：

function titleCase(str) {
    var newStr = str.toLowerCase().split(' ');
    for (var i = 0; i < newStr.length; i++) {
        newStr[i] = newStr[i][0].toUpperCase() + newStr[i].substring(1, newStr[i].length);
        //substring() 方法用于提取字符串中介于两个指定下标之间的字符。此处提取的是每一个分割字符除首字母外的部分
    }
    var string = newStr.join(' ');
    return string;
}
titleCase("you are my sunshine");
```
##### 6、寻找数组中的最大值 
右边大数组中包含了4个小数组，分别找到每个小数组中的最大值，然后把它们串联起来，形成一个新数组。

提示：你可以用for循环来迭代数组，并通过arr[i]的方式来访问数组的每个元素。 
```
Answer：

function largestOfFour(arr) {
    // You can do this!
    var maxNum = [];
    for (i = 0; i < arr.length; i++) {
        var temp = arr[i][0];
        for (j = 1; j < arr[i].length;j++) {
            if (arr[i][j] > temp) {
                temp = arr[i][j];
            }
        }
        maxNum[i] = temp;
    }
    return maxNum;
}
largestOfFour([[4, 5, 1, 3], [13, 27, 18, 26], [32, 35, 37, 39], [1000, 1001, 857, 1]]);
```
#### 7、确认末尾字符算法 
检查一个字符串(str)是否以指定的字符串(target)结尾。

如果是，返回true;如果不是，返回false。

这道题比较简单，但一开始的时候我把target想成是一个字符了，没有好好读题。还是写好在执行的过程中发现执行总是不通过。

思路：分割成数组，利用JS方法取到和target字符相同长度的字符，再和target做比较。（后来发现并不需要分割数组，因为target本身就是个字符串。。。） 
```
Answer：

function confirmEnding(str,target){
    var arr = str.substr((str.length - target.length),str.length-1);
    if(arr == target){
        return true;
    }else{
        return false;
    }
}
confirmEnding("He has to give me a new name", "name");
附substr()方法
```
#### 8、重复操作算法 
循环拼接一个指定的字符串 num次，如果num是一个负数,则返回一个空字符串。 
```
Answer：

function repeat(str,num){
    var a = '';
    if(str < 0){
        str = a;
    }else{
        for(var i = 0;i < num;i++){
            a += str;
        }
    }
    return a;
}
repeat("abc", 3);
```

#### 9、截断一个字符串 


截断一个字符串！

如果字符串的长度比指定的参数num长，则把多余的部分用…来表示。

切记，插入到字符串尾部的三个点号也会计入字符串的长度。

但是，如果指定的参数num小于或等于3，则添加的三个点号不会计入字符串的长度。
``` 
Answer：

function truncate(str,num){
    var a = '';
    if(num >= str.length){
        return str;
    }
    if(num <= 3){
        for(i = 0;i < num;i++){
            a += str[i];
        }
        return a + '...';
    }
    if(num > 3 && num < str.length){
    //如果指定参数大于3且小于字符串长度，遍历得到加上...共num个长度的字符
        for(i = 0;i < num - 3;i++){
            a += str[i];
        }function truncate(str, num) {
    var a = '';
    if (num >= str.length) { //如果指定参数大于字符串长度，则返回字符串
        return str;
    }
    if (num <= 3) { //如果指定参数小于3，遍历得到和指定参数相同长度的字符
        for (i = 0; i < num; i++) {
            a += str[i];
        }
        return a + '...';
    }
    if (num > 3 && num < str.length) { //如果指定参数大于3且小于字符串长度，遍历得到加上...共num个长度的字符
        for (i = 0; i < num - 3; i++) {
            a += str[i];
        }
        return a + '...';
    }
}
truncate("A-tisket a-tasket A green and yellow basket", 11);
truncate("areww",2);
```
#### 10、数组分割算法 
把一个数组arr按照指定的数组大小size分割成若干个数组块。

例如:chunk([1,2,3,4],2)=[[1,2],[3,4]];

chunk([1,2,3,4,5],2)=[[1,2],[3,4],[5]];
```
Answer：

function chunk(arr, size) {
    var len = parseInt(arr.length / size); //数组的长度除于指定的size得到一个整数。
    var remain = arr.length % size; //数组的长度除于指定的size取余
    var a = [],
        count = 1,
        sot = remain > 0 ? len + 1 : len; //sot为新数组的长度
    for (var f = 1; f <= sot; f++) {
        var start = size * (f - 1); //数组一般从0开始。
        var end = (f > len) ? ((f - 1) * size + remain) : f * size;
        //结束是按size,就是一个数学逻辑式。如果f>len，一般就是最后一次循环，只需要用前面的加上最后余的数，如果没有或没有余数，就直接f*size就是结束的点。
        var mylocates = arr.slice(start, end); //slice() 方法可从已有的数组中返回选定的元素。
        a.push(mylocates);
    }
    return a;
}
chunk(["a", "b", "c", "d"], 2);
```
#### 11、数组截断算法 
返回一个数组被截断n个元素后还剩余的元素，截断从索引0开始。 
```
Answer

function slasher(arr, howMany) {
    var a = [];
    for (var i = 0, j = 0; i < arr.length; i++) {
        if (i > howMany - 1) {
            a[j] = arr[i];
            j++;
        }
    }
    return a;
}

slasher([1, 2, 3], 2);
```
#### 12、数组查询算法 
如果数组第一个字符串元素包含了第二个字符串元素的所有字符，函数返回true。

举例，[“hello”, “Hello”]应该返回true，因为在忽略大小写的情况下，第二个字符串的所有字符都可以在第一个字符串找到。

[“hello”, “hey”]应该返回false，因为字符串”hello”并不包含字符”y”。

[“Alien”, “line”]应该返回true，因为”line”中所有字符都可以在”Alien”找到。 
```
Answer

function mutation(arr) {
    var a = [];
    var num = 0;
    arr[0] = arr[0].toUpperCase();
    arr[1] = arr[1].toUpperCase();
    for (var i = 0; i < arr[1].length; i++) {
        for (var j = 0; j < arr[0].length; j++) {
            if (arr[1][i] == arr[0][j]) {
                num++;
                break;
            }
        }
    }
    if (num == arr[1].length) {
        return true;
    } else {
        return false;
    }
}
mutation(["hello", "Hello"]);
```
#### 13、删除数组中的所有假值 
删除数组中的所有假值。

在JavaScript中，假值有false、null、0、”“、undefined 和 NaN。假值只需要用！arr[i]表示。
```
function bouncer(arr){
    for(var i=arr.length-1;i>-1;i--){
        if(!arr[i]){
            arr.splice(i,1);
        }
    }
    return arr;
}
bouncer([7,"ate","",false,9]);
```
#### 14、去除数组中任意多个值 
实现一个 destroyer

函数，第一个参数是初始数组，后跟一个或多个参数。从初始数组中删除与这些参数具有相同值的所有元素。 
```
Answer：

function destroyer(arr, p1, p2, p3) {
    for (var i = 0; i < arr.length; i++) {
        if (arr[i] === p1 || arr[i] === p2 || arr[i] === p3) {
            arr.splice(i, 1);
            i--;
        }
    }
    return arr;
}
destroyer([1, 2, 3, 4, 2, 1, 3, 4], 2, 3, 4);
```
#### 15、数组排序并返回索引值 
先给数组排序，然后找到指定的值在数组的位置，最后返回位置对应的索引。

举例：where([1,2,3,4], 1.5) 应该返回 1。因为1.5插入到数组[1,2,3,4] 后变成[1,1.5,2,3,4]，而1.5对应的索引值就是1。

同理，where([20,3,5], 19) 应该返回 2。因为数组会先排序为 [3,5,20]， 19插入到数组[3,5,20]后变成[3,5,19,20]，而19对应的索引值就是2。 
```
Answer：

function where(arr, num) {
    var i = 0,
        len = arr.length,
        j, d;
    for (; i < len; i++) {//插入值
        for (j = 0; j < len; j++) {
            if (arr[i] < arr[j]) {
                d = arr[j];
                arr[j] = arr[i];
                arr[i] = d;
            }
        }
    }
    //返回索引值
    for (i = 0; i < arr.length; i++) {
        if (arr[i] == num) {
            return i;
        }
        if (num > arr[i] && num < arr[i + 1]) {
            return i + 1;
        }
        if (num > arr[arr.length - 1]) {
            return arr.length;
        }
    }
}

where([40, 60], 50);
```
#### 16、位移密码 
著名的凯撒密码Caesar cipher，又叫移位密码。

移位密码也就是密码中的字母会按照指定的数量来做移位。

一个常见的案例就是ROT13密码，字母会移位13个位置。由’A’ ↔ ‘N’, ‘B’ ↔’O’，以此类推。

写一个ROT13函数，实现输入加密字符串，输出解密字符串。

所有的字母都是大写，不要转化任何非字母形式的字符(例如：空格，标点符号) // 遇到这些特殊字符，就跳过它们。 
```
Answer：

function rot13(str) {

    var strup = str.toUpperCase(); //把所有字母都转成大写
    var charcodearr = [];
    var rotcodearr = [];
    for (var i = 0; i < strup.length; i++) { //获取strup每个字母的code并push进数组charcodearr里
        var strcode = (strup.charCodeAt(i));
        charcodearr.push(strcode);
    }
    for (var j = 0; j < charcodearr.length; j++) { //凯撒加密,加密后的值push进数组rotcodearr里
        if (charcodearr[j] < 65) {
            rotcodearr.push(charcodearr[j]);
        } else if (charcodearr[j] < 78) {
            rotcodearr.push(charcodearr[j] + 13);
        } else if (charcodearr[j] < 91) {
            rotcodearr.push(charcodearr[j] - 13);
        } else if (charcodearr[j] > 91) {
            rotcodearr.push(charcodearr[j]);
        }
    }

    return String.fromCharCode.apply(this, rotcodearr);
    //成string并返回成字母
}
rot13("SERR PBQR PNZC");
26个字母的unicode码在65(A)与90(Z)之间,第13位M(77); 
后13位字母减去13后放入； 
前13位字母加上13后放入； 
通过.fromCharCode()转化为字母，将数组转化为字符串；
```
