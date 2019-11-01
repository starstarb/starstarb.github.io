---
title: 算法笔记训练指南：A+B和C
copyright: true
tags: 算法刷题
categories: 算法刷题
abbrlink: 135b1a23
date: 2019-10-28 14:38:36
---
A+B和C
<!--more-->
A+B和C

链接：https://www.nowcoder.com/questionTerminal/33ceeced8acc475aa0be9603936890a6


给定区间[-2的31次方, 2的31次方]内的3个整数A、B和C，请判断A+B是否大于C。

输入描述:

输入第1行给出正整数T(<=10)，是测试用例的个数。随后给出T组测试用例，每组占一行，顺序给出A、B和C。整数间以空格分隔。



输出描述:

对每组测试用例，在一行中输出“Case #X: true”如果A+B>C，否则输出“Case #X: false”，其中X是测试用例的编号（从1开始）。

示例1
输入

4<br>
1 2 3<br>
2 3 4<br>
2147483647 0 2147483646<br>
0 -2147483648 -2147483647<br>

输出

Case #1: false <br>
Case #2: true <br>
Case #3: true<br>
Case #4: false<br>


```
#include<iostream>
using namespace std;
int main()
{
	int i;
	int T;//测试数据组数 
	long long  a,b,c;//三个整数 
	int flag=1;
	cin>>T;
	for(int i=0;i<T;i++)
	{
		/*cin>>a>>b>>c;
	    if(a+b>c)*/
	    cin>>a>>b>>c;
		if(a+b>c)
		{
			cout<<"Case #"<<flag++<<": true"<<endl;
		 } 
		 else cout<<"Case #"<<flag++<<": false"<<endl;
	}
	return 0;
}
```