---
title: MarkDown 学习
tags: MarkDown
categories: MarkDown
abbrlink: 40fb2c33
date: 2019-07-03 22:53:18
---
MarkDown学习笔记
<!--more-->
<big>
# 一级标题
## 二级标题
### 三级标题
#### 四级标题
##### 五级标题




加粗符

加粗可以使用“**” 或者 “__”来表示加粗。
例：**加粗**  或者 __加粗__ 使字体变粗

斜体符

强调可以使用 “*” 或者 “_” 来表示斜体强调
例：*斜体* 或者 _斜体_ 

引用使用符号“>”来表示接下来的都将是引用的语句，不同软件和平台对于引用支持显示不同


邮箱

邮箱地址链接跟平时输入邮箱一样，只需要在前后加上一个"<>"就可以了
例：邮箱<230326401@qq.com>
       


链接

最简单直接用“<>”将链接放进去
例：<http://www.baidu.com>
http://www.baidu.com

内联的链接地址：即自定义一个名称可以指定链接的地址
语法：[显示的名称](具体的链接地址)
例：[这是一个内联的地址](http://www.baidu.com) 

图片
图片的使用和链接很相似，几乎使用同样的语法结构，只需要在"[]"前加上一个 “!” 即可

添加表格

ABCD | EFGH | IGKL
-----|------|----
a    | b    | c
d    | e    | f
g    | h    | i
</big>

## 补充MarkDown高级语法学习，官方文档学习，进阶学习

Markdown 的目标是实现「易读易写」。

可读性，无论如何，都是最重要的。一份使用 Markdown 格式撰写的文件应该可以直接以纯文本发布，并且看起来不会像是由许多标签或是格式指令所构成。Markdown 语法受到一些既有 text-to-HTML 格式的影响，包括 Setext、atx、Textile、reStructuredText、Grutatext 和 EtText，而最大灵感来源其实是纯文本电子邮件的格式

### 区块引用 类似于email中>的引用方式

Markdown 也允许你偷懒只在整个段落的第一行最前面加上 > ：
```
> This is a blockquote with two paragraphs. Lorem ipsum dolor sit amet,
consectetuer adipiscing elit. Aliquam hendrerit mi posuere lectus.
Vestibulum enim wisi, viverra nec, fringilla in, laoreet vitae, risus.
```

块引用可以嵌套（例如：引用内的引用），只要根据层次加上不同数量的 > ：
```
 This is the first level of quoting.
>
> > This is nested blockquote.
>
> Back to the first level.
```
引用的区块内也可以使用其他的 Markdown 语法，包括标题、列表、代码区块等：
```
> ## 这是一个标题。
> 
> 1.   这是第一行列表项。
> 2.   这是第二行列表项。
> 
> 给出一些例子代码：
> 
>     return shell_exec("echo $input | $markdown_script");
```
任何像样的文本编辑器都能轻松地建立 email 型的引用。例如在 BBEdit 中，你可以选取文字后然后从选单中选择增加引用阶层。


### 列表
Markdown 支持有序列表和无序列表。

无序列表使用星号、加号或是减号作为列表标记：
```
*   Red
*   Green
*   Blue
```
等同于：
```
+   Red
+   Green
+   Blue
```
也等同于：
```
-   Red
-   Green
-   Blue
```

有序列表则使用数字接着一个英文句点：
```
1.  Bird
2.  McHale
3.  Parish
```
列表项目可以包含多个段落，每个项目下的段落都必须缩进 4 个空格或是 1 个制表符：
```
1.  This is a list item with two paragraphs. Lorem ipsum dolor
    sit amet, consectetuer adipiscing elit. Aliquam hendrerit
    mi posuere lectus.

    Vestibulum enim wisi, viverra nec, fringilla in, laoreet
    vitae, risus. Donec sit amet nisl. Aliquam semper ipsum
    sit amet velit.

2.  Suspendisse id sem consectetuer libero luctus adipiscing.
```
如果要在列表项目内放进引用，那 > 就需要缩进：
```
*   A list item with a blockquote:

    > This is a blockquote
    > inside a list item.
```

在行首出现数字-句点-空白，要避免这样的状况，你可以在句点前面加上反斜杠。
```
1986\. What a great season.
```
### 代码区块

和程序相关的写作或是标签语言原始码通常会有已经排版好的代码区块，通常这些区块我们并不希望它以一般段落文件的方式去排版，而是照原来的样子显示，Markdown 会用 <pre> 和 <code> 标签来把代码区块包起来。

要在 Markdown 中建立代码区块很简单，只要简单地缩进 4 个空格或是 1 个制表符就可以，例如，下面的输入：
```
这是一个普通段落：

    这是一个代码区块。
```

分隔线

你可以在一行中用三个以上的星号、减号、底线来建立一个分隔线，行内不能有其他东西。你也可以在星号或是减号中间插入空格。下面每种写法都可以建立分隔线：
```
* * *

***

*****

- - -

---------------------------------------

```

# 区段元素
###链接

Markdown 支持两种形式的链接语法： 行内式和参考式两种形式。

不管是哪一种，链接文字都是用 [方括号] 来标记。

要建立一个行内式的链接，只要在方块括号后面紧接着圆括号并插入网址链接即可，如果你还想要加上链接的 title 文字，只要在网址后面，用双引号把 title 文字包起来即可，例如：
```
This is [an example](http://example.com/ "Title") inline link.

[This link](http://example.net/) has no title attribute.
```
会产生：
```
<p>This is <a href="http://example.com/" title="Title">
an example</a> inline link.</p>

<p><a href="http://example.net/">This link</a> has no
title attribute.</p>
```
如果你是要链接到同样主机的资源，你可以使用相对路径：
```
See my [About](/about/) page for details.
```
参考式的链接是在链接文字的括号后面再接上另一个方括号，而在第二个方括号里面要填入用以辨识链接的标记：
```
This is [an example][id] reference-style link.
```
你也可以选择性地在两个方括号中间加上一个空格：
```
This is [an example] [id] reference-style link.
```
接着，在文件的任意处，你可以把这个标记的链接内容定义出来：
```
[id]: http://example.com/  "Optional Title Here"
```
链接内容定义的形式为：

*    方括号（前面可以选择性地加上至多三个空格来缩进），里面输入链接文字
*    接着一个冒号
*    接着一个以上的空格或制表符
*    接着链接的网址
*    选择性地接着 title 内容，可以用单引号、双引号或是括弧包着
下面这三种链接的定义都是相同：
```
[foo]: http://example.com/  "Optional Title Here"
[foo]: http://example.com/  'Optional Title Here'
[foo]: http://example.com/  (Optional Title Here)

```

你也可以把 title 属性放到下一行，也可以加一些缩进，若网址太长的话，这样会比较好看：
```
[id]: http://example.com/longish/path/to/resource/here
    "Optional Title Here"
```
网址定义只有在产生链接的时候用到，并不会直接出现在文件之中。

链接辨别标签可以有字母、数字、空白和标点符号，但是并不区分大小写，因此下面两个链接是一样的：
```
[link text][a]
[link text][A]
```
隐式链接标记功能让你可以省略指定链接标记，这种情形下，链接标记会视为等同于链接文字，要用隐式链接标记只要在链接文字后面加上一个空的方括号，如果你要让 "Google" 链接到 google.com，你可以简化成：
```
[Google][]
```
然后定义链接内容：
```
[Google]: http://google.com/
```
由于链接文字可能包含空白，所以这种简化型的标记内也许包含多个单词：
```
Visit [Daring Fireball][] for more information.
```
然后接着定义链接
```
[Daring Fireball]: http://daringfireball.net/
```
链接的定义可以放在文件中的任何一个地方，我比较偏好直接放在链接出现段落的后面，你也可以把它放在文件最后面，就像是注解一样。

下面是一个参考式链接的范例：
```
I get 10 times more traffic from [Google] [1] than from
[Yahoo] [2] or [MSN] [3].

  [1]: http://google.com/        "Google"
  [2]: http://search.yahoo.com/  "Yahoo Search"
  [3]: http://search.msn.com/    "MSN Search"
```
如果改成用链接名称的方式写：
```
I get 10 times more traffic from [Google][] than from
[Yahoo][] or [MSN][].

  [google]: http://google.com/        "Google"
  [yahoo]:  http://search.yahoo.com/  "Yahoo Search"
  [msn]:    http://search.msn.com/    "MSN Search"
```
上面两种写法都会产生下面的 HTML。
```
<p>I get 10 times more traffic from <a href="http://google.com/"
title="Google">Google</a> than from
<a href="http://search.yahoo.com/" title="Yahoo Search">Yahoo</a>
or <a href="http://search.msn.com/" title="MSN Search">MSN</a>.</p>
```
下面是用行内式写的同样一段内容的 Markdown 文件，提供作为比较之用：
```
I get 10 times more traffic from [Google](http://google.com/ "Google")
than from [Yahoo](http://search.yahoo.com/ "Yahoo Search") or
[MSN](http://search.msn.com/ "MSN Search").
```
参考式的链接其实重点不在于它比较好写，而是它比较好读，比较一下上面的范例，使用参考式的文章本身只有 81 个字符，但是用行内形式的却会增加到 176 个字元，如果是用纯 HTML 格式来写，会有 234 个字元，在 HTML 格式中，标签比文本还要多。

使用 Markdown 的参考式链接，可以让文件更像是浏览器最后产生的结果，让你可以把一些标记相关的元数据移到段落文字之外，你就可以增加链接而不让文章的阅读感觉被打断。

## 强调

Markdown 使用星号（ * ）和底线（ _ ）作为标记强调字词的符号，被 * 或 _ 包围的字词会被转成用 `<em> `标签包围，用两个 * 或 _ 包起来的话，则会被转成 `<strong>`，例如：
```
*single asterisks*

_single underscores_

**double asterisks**

__double underscores__
```
会转成：
```
<em>single asterisks</em>

<em>single underscores</em>

<strong>double asterisks</strong>

<strong>double underscores</strong>
```
你可以随便用你喜欢的样式，唯一的限制是，你用什么符号开启标签，就要用什么符号结束。

强调也可以直接插在文字中间：
```
un*frigging*believable
```
但是如果你的 * 和 _ 两边都有空白的话，它们就只会被当成普通的符号。

如果要在文字前后直接插入普通的星号或底线，你可以用反斜线：
```
\*this text is surrounded by literal asterisks\*
```
### 代码

如果要标记一小段行内代码，你可以用反引号把它包起来（\`），例如：

```
Use the `printf()` function.
```

会产生：
```
<p>Use the <code>printf()</code> function.</p>
```
如果要在代码区段内插入反引号，你可以用多个反引号来开启和结束代码区段：

``There is a literal backtick (`) here.``

这段语法会产生：
```
<p><code>There is a literal backtick (`) here.</code></p>
```
代码区段的起始和结束端都可以放入一个空白，起始端后面一个，结束端前面一个，这样你就可以在区段的一开始就插入反引号：
```
A single backtick in a code span: `` ` ``

A backtick-delimited string in a code span: `` `foo` ``
```
会产生：
```
<p>A single backtick in a code span: <code>`</code></p>

<p>A backtick-delimited string in a code span: <code>`foo`</code></p>
```
在代码区段内，& 和方括号都会被自动地转成 HTML 实体，这使得插入 HTML 原始码变得很容易，Markdown 会把下面这段：
```
Please don't use any `<blink>` tags.
```
转为：
```
<p>Please don't use any <code><blink></code> tags.</p>
```
你也可以这样写：
```
`&#8212;` is the decimal-encoded equivalent of `&mdash;`.
```
以产生：
```
<p><code>&amp;#8212;</code> is the decimal-encoded
equivalent of <code>&amp;mdash;</code>.</p>
```
### 图片

很明显地，要在纯文字应用中设计一个「自然」的语法来插入图片是有一定难度的。

Markdown 使用一种和链接很相似的语法来标记图片，同样也允许两种样式： 行内式和参考式。

行内式的图片语法看起来像是：
```
![Alt text](/path/to/img.jpg)

![Alt text](/path/to/img.jpg "Optional title")
```
详细叙述如下：

    一个惊叹号 !
    接着一个方括号，里面放上图片的替代文字
    接着一个普通括号，里面放上图片的网址，最后还可以用引号包住并加上 选择性的 'title' 文字。

参考式的图片语法则长得像这样：
```
![Alt text][id]
```
「id」是图片参考的名称，图片参考的定义方式则和连结参考一样：
```
[id]: url/to/image  "Optional title attribute"
```
到目前为止， Markdown 还没有办法指定图片的宽高，如果你需要的话，你可以使用普通的 <\img> 标签。

### 反斜杠

Markdown 可以利用反斜杠来插入一些在语法中有其它意义的符号，例如：如果你想要用星号加在文字旁边的方式来做出强调效果（但不用 <\em> 标签），你可以在星号的前面加上反斜杠：
```
\*literal asterisks\*
```
Markdown 支持以下这些符号前面加上反斜杠来帮助插入普通的符号：
```
\   反斜线
`   反引号
*   星号
_   底线
{}  花括号
[]  方括号
()  括弧
#   井字号
+   加号
-   减号
.   英文句点
!   惊叹号
```
### 自动链接

Markdown 支持以比较简短的自动链接形式来处理网址和电子邮件信箱，只要是用方括号包起来， Markdown 就会自动把它转成链接。一般网址的链接文字就和链接地址一样，例如：
```
<http://example.com/>
```
Markdown 会转为：
```
<a href="http://example.com/">http://example.com/</a>
```
邮址的自动链接也很类似，只是 Markdown 会先做一个编码转换的过程，把文字字符转成 16 进位码的 HTML 实体，这样的格式可以糊弄一些不好的邮址收集机器人，例如：
```
<address@example.com>
```
Markdown 会转成：
```
<a href="mailto:addre
ss@example.co
m">address@exa
mple.com</a>
```
在浏览器里面，这段字串（其实是 <\a href="mailto:address\@example.com">address\@example.com</a>）会变成一个可以点击的「address\@example.com」链接。

（这种作法虽然可以糊弄不少的机器人，但并不能全部挡下来，不过总比什么都不做好些。不管怎样，公开你的信箱终究会引来广告信件的。）

### MarkDown常用数学符号：