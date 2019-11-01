---
title: 算法笔记训练指南：部分A+B
copyright: true
tags: 算法刷题
categories: 算法刷题
abbrlink: 2bddccd7
date: 2019-10-28 14:38:57
---
部分A+B
<!--more-->
链接：https://www.nowcoder.com/questionTerminal/fb581ea099a14f5d97c6149cbeee249f
来源：牛客网

正整数A的“DA（为1位整数）部分”定义为由A中所有DA组成的新整数PA。例如：给定A = 3862767，DA = 6，则A的“6部分”PA是66，因为A中有2个6。

现给定A、DA、B、DB，请编写程序计算PA + PB。

输入描述:

输入在一行中依次给出A、DA、B、DB，中间以空格分隔，其中0 < A, B < 1010。



输出描述:

在一行中输出PA + PB的值。

示例1
输入

3862767 6 13530293 3

输出

399
```
#include<iostream>
using namespace std;
int main()
{
	long long  a,a1,b,b1;//计算a1+b1
	ios::sync_with_stdio(false);
	cin>>a>>a1>>b>>b1;
	long long pa=0,pb=0;
	while(a!=0){
		if(a%10==a1) pa=pa*10+a1;
		a/=10;
	}
	while(b!=0){
		if(b%10==b1) pb=pb*10+b1;
		b/=10;
	}
	cout<<pa+pb<<endl;
	return 0;
}
```