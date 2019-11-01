---
title: 算法笔记训练指南：Shortest Distance
copyright: true
tags: 算法刷题
categories: 算法刷题
abbrlink: 1ed7dde9
date: 2019-10-28 14:38:48
---
Shortest Distance
<!--more-->
链接：https://www.nowcoder.com/questionTerminal/3432b59d01834e53bda8f5eb439bed9b
来源：牛客网

The task is really simple: given N exits on a highway which forms a simple cycle, you are supposed to tell the shortest distance between any pair of exits.

输入描述:

Each input file contains one test case.  For each case, the first line contains an integer N (in [3, 105]), followed by N integer distances D1 D2 ... DN, where Di is the distance between the i-th and the (i+1)-st exits, and DN is between the N-th and the 1st exits.  All the numbers in a line are separated by a space.  The second line gives a positive integer M (<=104), with M lines follow, each contains a pair of exit numbers, provided that the exits are numbered from 1 to N.  It is guaranteed that the total round trip distance is no more than 107.



输出描述:

For each test case, print your results in M lines, each contains the shortest distance between the corresponding given pair of exits.

示例1
输入

5 1 2 4 14 9 <br>
3  <br>
1 3 <br>
2 5<br>
4 1

输出

3 <br>
10 <br>
7   <br>
```
#include<iostream>
#include<algorithm>
using namespace std;
const int MAXN=100005;
int dis[MAXN],a[MAXN]; //dis存放1号到i+1号的距离 a存放i到i+1号顶点距离 
int main(){
	ios::sync_with_stdio(false);
	int n,sum=0,query,left,right;
	cin>>n;
	for(int i=1;i<=n;i++){
		cin>>a[i];
		sum+=a[i];
		dis[i]=sum;//顺时针存 
	}
	cin>>query;
	for(int i=0;i<query;i++){
		cin>>left>>right;
		if(left>right) swap(left,right);
		int temp=dis[right-1]-dis[left-1];
		cout<<min(temp,sum-temp)<<endl;
	}
	return 0;
} 
```