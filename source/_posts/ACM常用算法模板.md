---
title: ACM常用算法模板
tags: 算法模板
categories: 算法模板
abbrlink: a95aaa1f
date: 2019-07-28 10:33:21
top: 7
updated: 2019-08-28 10:33:21
---
算法模板学习
![](https://github.com/starstarb/clouding/raw/master/head/fb4cd4a65d7e467b45eba431e63eb5f9.jpg)
<!--more-->

#### 文本输入输出
```
void fre(){
	freopen("C:\\Users\\Desktop\\输入文本.txt", "r", stdin);
	freopen("C:\\Users\\Desktop\\输出文本.txt", "w", stdout);
}    //注意都是双\

```

### 数组去重
```
Eg:1 2 2 3 4 =》 1 2 3 4 2
int b[N];
sort(b,b+N);
int len = unique(b, b + n) - b;//返回的是4的位置
```
### GCD、LCM
```
int gcd(int a,int b){  return b?gcd(b,a%b):a;}    //最大公约数 b==0
lcm = a * b / gcd(a,b)    //最小公倍数
```
### 字符串 <-> 整型
```
sscanf(s,"%d",&n);//从字符串s中读入整数n
sprintf(s,"%d",n);//将n转换为字符串s
```
### getchar()读入转str
```
while ((x1[0] = getchar()) && x1[0] != '\n'){
	int len1 = 1;
	while ((x1[len1] = getchar()) && x1[len1] != ' ')len1++;
	x1[len1] = '\0';
	string s1 = x1;
}
```
### char[]转Int
```
char s[100];
int x=atoi(s);
```
### 同余
```
(a+b)%m=(a%m)+(b%m)
(a-b)%m=(a%m)-(b%m)  //稍微留意负数情况
(a*b)%m=(a%m)*(b%m)
m^n%c=(m%c)^n%c;
```

### 求n!位数
```
for(int i=2;i<=n;i++) 
    len+=log10(i*1.0);  
ans=(int)len+1;
```
斯特林公式：
```
len=0.5*log10(2*3.1415927*n)+n*log10(n/2.718281828459); 
ans=(int)len+1;
```
### 容斥原理
N个方格m种颜色问有多少种染色方案：

设F(m)为在n个方格上使用m种颜色任意染色的方案数

F(m)=m^n，使用快速幂求一次F(m)的时间为O(log(n))

Ans=C(m,m)F(m)-C(m,m-1)F(m-1)+C(m,m-2)F(m-2)-C(m,m-3)F(m-3)+……

用杨辉三角预处理出C(m,n)花费O(m^2)

总时间复杂度为O(m^2+mlog(n))

### Log与自然对数e

Log（x）表示ln（x） ，其他例如：log10（x） ，log2（x），用exp（x）来表示e^

###  快速幂
```
long long Pow(long long a,long long n){
	long long ret=1;
	while(n){
		if(n&1)ret*=a;
		a*=a;
		n>>=1;
	}
	return ret;
}
long long Mod_Pow(long long a,long long n,long long mod){
	long long ret=1;
	while(n){
		if(n&1)ret=(ret*a)%mod;
		a=(a*a)%mod;
		n>>=1;
	}
	return ret;
}
```
 快速幂算法——可迅速求出a^b。其主要理论依据如下：

 1，当b为偶数时，a^b可以转为a^2的b/2次方。

 2，当b为奇数时，a^b可以转为a^2的b/2次方，再乘以a。
而a^2的b/2次方，可以使用上述方式转为a^4的b/4次方再乘以某个数。
在每一次进行循环时，如果b为奇数，则a^b可以转为a^2的b/2次方乘以a。所以每一次进行a^2计算时，需要根据b是否为奇数决定是否在最终的结果上乘以a。
a = a* a;此步计算完成后，则a是下一个进行平方运算的数。这样当所有的循环结束后，a就是a^k，其中k是离b最近的，且为2的整数次方的数。

利用快速幂方法可以迅速求出一个数的任意次方。再结合a* b%m=(a%m)* (b%m)%m，只是要相乘后再取模，就可以先取模再相乘，然后再取模的规律。我们不考虑a最终会用来做什么，反正a就是用来相乘然后取模的，所以可以直接将a进行取模，然后再进行相乘。
同理，对于if判断中的操作也是如此：因为要相乘再取模，所以就先取模再相乘。



### 求1到n的数的异或和O（1）
```
unsigned xor_n(unsigned n){
	unsigned t = n & 3;
	if (t & 1) return t / 2u ^ 1;   //照着打就行，u默认为unsigned int
	return t / 2u ^ n;
}
```
### 卡特兰数
给定一个凸n边形，问将其划分成三角形的方法数
```
1,1,2,5,14,42,132….

h(n)=h(n-1)*(4*n-2)/(n+1)
h(n)=C(2n,n)/(n+1)
h(0)=1
```
### 错排公式
给定n种颜色篮子和n种颜色球，求全放错情况数
```
D(n)=(n-1)*(D(n-2)+D(n-1))
D(1)=0,D(2)=1
```
### 素数
（N以内有大概num=N/ln（x）个素数，N越大越准）
1. 简单素数打表：O(n* sqrt(n))
```
void prim(){
	int num = 0;
	for (int i = 2; i < maxn; i++){
		int k = 1;
		for (int j = 2; j <= (int)sqrt(i);j++)
			if (i%j == 0) { k = 0; break; }
		if (k) pri[num++] = i;
	}
}
```
2. 素数筛法：O(nlogn)
```
vector<int> pri;
void prim(){
	vis[0] = vis[1] = true;
	for (int i = 2; i < maxn; i++)
		if (!vis[i]){
			pri.push_back(i);
			for (int j = i + i; j < maxn; j += i)
				vis[j] = true;
		}
}
```
3. 高效素数打表:O(n)
(线性筛法--欧拉	)
```
void prim(){
	memset(vis, 0, sizeof(vis));
	int num = 0;
	for (int i = 2; i <= M; ++i){
		if (!vis[i])  pri[num++] = i;   
		for (int j = 1; ((j <= num) && (i * pri[j] <= M)); ++j){
			vis[i * pri[j]] = 1;
			if (i % pri[j] == 0) break;   
		}
	}
}//1不是素数，最小素数为2
```

### 分解质因数
```
vector<int> V[N]; 
void getDiv(int x, int idx) { //得到的是所有种类 eg：12=》2，3
	if (x == 1) {
		V[idx].push_back  (1);
		return;
	}
	int len = (int)sqrt(x);
	for (int i = 2; i <= (int)len; i++)
		if (x % i == 0)
			V[idx].push_back  (i), V[idx].push_back  (x / i);
	if (len * len == x)
		V[idx].pop_back();
	V[idx].push_back  (x);
}
 
void fun(long long x) {//得到的是所有质因数 eg：12=》2，2，3
	p.clear();
	for (long long i = 2; i <=sqrt(x); i++){
		while (x%i == 0){
			p.push_back(i);
			x /= i;
		}	
	}    
       if (x != 1) p.push_back(x);   
 }
```

### 吉姆拉尔森公式
```
int CaculateWeekDay(int y,int m, int d)  
{  
    if(m==1||m==2) {//把一月和二月看为是上一年的十三月和十四月
        m+=12;  
        y--;  
    }  
    int iWeek=(d+2*m+3*(m+1)/5+y+y/4-y/100+y/400)%7;  
    return iWeek;  
}  //给定年月日O（1）算周数 
```
### 高精度
```
/*
**高精度，支持乘法和加法
*/
struct  Big_Num
{
	const  static  int   mod = 10000;//每4位数存一次
	const  static  int   DLEN = 4;
	int  num[600],  len;  
	Big_Num()
	{
		memset(num, 0, sizeof(num));
		len = 1;
	}
	Big_Num(int   v)
	{
		memset(num, 0, sizeof(num));
		len = 0;
		do
		{
			num[len++] = v%mod;
			v /= mod;
		} while (v);
	}
	Big_Num(const char  s[])
	{
		memset(num, 0, sizeof(num));
		int L = strlen(s);
		len = L / DLEN;
		if (L%DLEN)len++;
		int index = 0;
		for (int i = L - 1; i >= 0; i -= DLEN)
		{
			int t = 0;
			int k = i - DLEN + 1;
			if (k < 0)k = 0;
			for (int j = k; j <= i; j++)
				t = t * 10 + s[j] - '0';
			num[index++] = t;
		}
	}
 
 
	Big_Num  operator +(const Big_Num &b)const
	{
		Big_Num  res;
		res.len = max(len, b.len);
		for (int i = 0; i <= res.len; i++)
			res.num[i] = 0;
		for (int i = 0; i < res.len; i++)
		{
			res.num[i] += ((i < len) ? num[i] : 0) + ((i < b.len) ? b.num[i] : 0);
			res.num[i + 1] += res.num[i] / mod;
			res.num[i] %= mod;
		}
		if (res.num[res.len] > 0)res.len++;
		return  res;
	}
	Big_Num  operator *(const Big_Num &b)const
	{
		Big_Num  res;
		for (int i = 0; i < len; i++)
		{
			int up = 0;
			for (int j = 0; j < b.len; j++)
			{
				int temp = num[i] * b.num[j] + res.num[i + j] + up;
				res.num[i + j] = temp%mod;
				up = temp / mod;
			}
			if (up != 0)
				res.num[i + b.len] = up;
		}
		res.len = len + b.len;
		while (res.num[res.len - 1] == 0 && res.len > 1)res.len--;
		return  res;
	}
	void  output()
	{
		printf("%d", num[len - 1]);
		for (int i = len - 2; i >= 0; i--)
			printf("%04d", num[i]);
		printf("\n");
	}
};
```
### 完全高精度
```
#include<iostream>
#include<algorithm>
#include<cstdio>
#include<cmath>
#include<string>
#include<cstring>
#include<vector>
using namespace std;
const int maxn = 1e5 + 50;
/*
*完全大数模板
*输入cin>>a
*输出a.print();
*注意这个输入不能自动去掉前导0的，可以先读入到char数组，去掉前导0，再用构造函数。
*/
#define MAXN 9999
#define MAXSIZE 1010
#define DLEN 4
class  BigNum
{
private:
	int a[500];                               //可以控制大数的位数
	int  len;
public:
	BigNum() { len = 1; memset(a, 0, sizeof(a)); }//构造函数
	BigNum(const   int);                      //将一个int类型的变量转化成大数
	BigNum(const   char*);                    //将一个字符串类型的变量转化为大数
	BigNum(const BigNum &);                   //拷贝构造函数
	BigNum &operator=(const  BigNum &);       //重载赋值运算符，大数之间进行赋值运算
	friend istream&   operator >> (istream&, BigNum&);//重载输入运算符
	friend ostream&   operator<<(ostream&, BigNum&);//重载输出运算符
	BigNum  operator+(const  BigNum &)const;  //重载加法运算符，两个大数之间的相加运算
	BigNum  operator-(const  BigNum &)const;  //重载减法运算符，两个大数之间的相减运算
	BigNum  operator*(const  BigNum &)const;  //重载乘法运算符，两个大数之间的相乘运算
	BigNum  operator/(const   int &)const;    //重载除法运算符，大数对一个整数进行相除运算
	BigNum  operator^(const   int &)const;    //大数的n次方运算
	int   operator%(const   int &)const;      //大数对一个int类型的变量进行取模运算
	bool  operator>(const BigNum  &T)const;   //大数和另一个大数的大小比较
	bool  operator>(const   int &t)const;     //大数和一个int类型的变量的大小比较
	void  print();                            //输出大数
};
BigNum::BigNum(const   int b)   //将一个int类型的变量转化为大数
{
	int  c, d = b;
	len = 0;
	memset(a, 0, sizeof(a));
	while (d>MAXN)
	{
		c = d - (d / (MAXN + 1))*(MAXN + 1);
		d = d / (MAXN + 1);
		a[len++] = c;
	}
	a[len++] = d;
}
BigNum::BigNum(const   char *s)  //将一个字符串类型的变量转化为大数
{
	int  t, k, index, L, i;
	memset(a, 0, sizeof(a));
	L = strlen(s);
	len = L / DLEN;
	if (L%DLEN)len++;
	index = 0;
	for (i = L - 1; i >= 0; i -= DLEN)
	{
		t = 0;
		k = i - DLEN + 1;
		if (k<0)k = 0;
		for (int j = k; j <= i; j++)
			t = t * 10 + s[j] - '0';
		a[index++] = t;
	}
}
BigNum::BigNum(const BigNum  &T) :len(T.len)  //拷贝构造函数
{
	int  i;
	memset(a, 0, sizeof(a));
	for (i = 0; i<len; i++)
		a[i] = T.a[i];
}
BigNum &  BigNum::operator=(const  BigNum &n)  //重载赋值运算符，大数之间赋值运算
{
	int  i;
	len = n.len;
	memset(a, 0, sizeof(a));
	for (i = 0; i<len; i++)
		a[i] = n.a[i];
	return *this;
}
istream&  operator >> (istream &in, BigNum &b)
{
	char  ch[MAXSIZE * 4];
	int  i = -1;
	in >> ch;
	int L = strlen(ch);
	int  count = 0, sum = 0;
	for (i = L - 1; i >= 0;)
	{
		sum = 0;
		int  t = 1;
		for (int j = 0; j < 4 && i >= 0; j++, i--, t *= 10)
		{
			sum += (ch[i] - '0')*t;
		}
		b.a[count] = sum;
		count++;
	}
	b.len = count++;
	return  in;
}
ostream&  operator<<(ostream&  out, BigNum& b)  //重载输出运算符
{
	int  i;
	cout << b.a[b.len - 1];
	for (i = b.len - 2; i >= 0; i--)
	{
		printf("%04d", b.a[i]);
	}
	return  out;
}
BigNum  BigNum::operator+(const  BigNum &T)const   //两个大数之间的相加运算
{
	BigNum t(*this);
	int  i, big;
	big = T.len>len ? T.len : len;
	for (i = 0; i<big; i++)
	{
		t.a[i] += T.a[i];
		if (t.a[i]>MAXN)
		{
			t.a[i + 1]++;
			t.a[i] -= MAXN + 1;
		}
	}
	if (t.a[big] != 0)
		t.len = big + 1;
	else  t.len = big;
	return  t;
}
BigNum  BigNum::operator-(const  BigNum &T)const  //两个大数之间的相减运算
{
	int  i, j, big;
	bool  flag;
	BigNum  t1, t2;
	if (*this>T)
	{
		t1 = *this;
		t2 = T;
		flag = 0;
	}
	else
	{
		t1 = T;
		t2 = *this;
		flag = 1;
	}
	big = t1.len;
	for (i = 0; i<big; i++)
	{
		if (t1.a[i]<t2.a[i])
		{
			j = i + 1;
			while (t1.a[j] == 0)
				j++;
			t1.a[j--]--;
			while (j>i)
				t1.a[j--] += MAXN;
			t1.a[i] += MAXN + 1 - t2.a[i];
		}
		else  t1.a[i] -= t2.a[i];
	}
	t1.len = big;
	while (t1.a[len - 1] == 0 && t1.len>1)
	{
		t1.len--;
		big--;
	}
	if (flag)
		t1.a[big - 1] = 0 - t1.a[big - 1];
	return  t1;
}
BigNum  BigNum::operator*(const  BigNum &T)const  //两个大数之间的相乘
{
	BigNum  ret;
	int  i, j, up;
	int  temp, temp1;
	for (i = 0; i<len; i++)
	{
		up = 0;
		for (j = 0; j<T.len; j++)
		{
			temp = a[i] * T.a[j] + ret.a[i + j] + up;
			if (temp>MAXN)
			{
				temp1 = temp - temp / (MAXN + 1)*(MAXN + 1);
				up = temp / (MAXN + 1);
				ret.a[i + j] = temp1;
			}
			else
			{
				up = 0;
				ret.a[i + j] = temp;
			}
		}
		if (up != 0)
			ret.a[i + j] = up;
	}
	ret.len = i + j;
	while (ret.a[ret.len - 1] == 0 && ret.len>1)ret.len--;
	return  ret;
}
BigNum  BigNum::operator/(const    int &b)const  //大数对一个整数进行相除运算
{
	BigNum  ret;
	int  i, down = 0;
	for (i = len - 1; i >= 0; i--)
	{
		ret.a[i] = (a[i] + down*(MAXN + 1)) / b;
		down = a[i] + down*(MAXN + 1) - ret.a[i] * b;
	}
	ret.len = len;
	while (ret.a[ret.len - 1] == 0 && ret.len>1)
		ret.len--;
	return  ret;
}
int  BigNum::operator%(const    int &b)const  //大数对一个 int类型的变量进行取模
{
	int  i, d = 0;
	for (i = len - 1; i >= 0; i--)
		d = ((d*(MAXN + 1)) % b + a[i]) % b;
	return  d;
}
BigNum  BigNum::operator^(const    int &n)const  //大数的n次方运算
{
	BigNum  t, ret(1);
	int  i;
	if (n<0)exit(-1);
	if (n == 0)return  1;
	if (n == 1)return *this;
	int  m = n;
	while (m>1)
	{
		t = *this;
		for (i = 1; (i << 1) <= m; i <<= 1)
			t = t*t;
		m -= i;
		ret = ret*t;
		if (m == 1)ret = ret*(*this);
	}
	return  ret;
}
bool  BigNum::operator>(const  BigNum &T)const
//大数和另一个大数的大小比较
{
	int  ln;
	if (len>T.len)return  true;
	else  if (len == T.len)
	{
		ln = len - 1;
		while (a[ln] == T.a[ln] && ln >= 0)
			ln--;
		if (ln >= 0 && a[ln]>T.a[ln])
			return  true;
		else
			return  false;
	}
	else
		return  false;
}
bool  BigNum::operator>(const   int &t)const  //大数和一个int类型的变量的大小比较
{
	BigNum  b(t);
	return *this>b;
}
void  BigNum::print()   //输出大数
{
	int  i;
	printf("%d", a[len - 1]);
	for (i = len - 2; i >= 0; i--)
		printf("%04d", a[i]);
	printf("\n");
}
BigNum f[110];//卡特兰数
int  main()
{
	f[0] = 1;
	for (int i = 1; i <= 100; i++)
		f[i] = f[i - 1] * (4 * i - 2) / (i + 1);//卡特兰数递推式
	int  n;
	while (~scanf("%d", &n))
	{
		if (n == -1)break;
		f[n].print();
	}
	return  0;
}
```
### strtok和sscanf结合输入
```
int  main()
{
	char s[] = "ab-cd:ef;gh: i-jkl;mnop;qrs-tu:vwx-y;z";
	char *delim = "-: ";   //分割条件字符串，目前里面有三个字符‘-’,’：’,‘  ’
	char *p;            //当目标字符串s中遇到分割条件字符时自动改写成‘\0’
	printf("%s ", strtok(s, delim));
	while ((p = strtok(NULL, delim)))
		printf("%s ", p);
	printf("\n");
}
int  main()
{
	int a, b, c;
	char str[] = "2018:7:15";
	sscanf(str, "%d:%d:%d", &a, &b, &c);
	cout << a << " " << b << " " << c << endl;
}
```
### 解决爆栈，手动加栈
```
#pragma comment(linker,  "/STACK:1024000000,1024000000")
```
### STL
1. 优先队列     priority_queue

empty()如果队列为空返回真

pop()删除对顶元素

push()加入一个元素

size()返回优先队列中拥有的元素个数

top()返回优先队列队顶元素

在默认的优先队列中，优先级高的先出队。在默认的int型中先出队的为较大的数。
```
priority_queue<int>q1;//大的先出对
priority_queue<int,vector<int>,greater<int>  >q2; //小的先出队
自定义比较函数：
struct  cmp
{
bool operator ()(int x, int  y)
{
return x > y; // x小的优先级高
//也可以写成其他方式，如： return p[x] > p[y];表示p[i]小的优先级高
}
};
priority_queue<int, vector<int>,  cmp>q;//定义方法
//其中，第二个参数为容器类型。第三个参数为比较函数。
结构体排序：
struct  node
{
int x, y;
friend bool operator < (node a, node  b)
{
return a.x > b.x; //结构体中，x小的优先级高
}
};
priority_queue<node>q;//定义方法
//在该结构中，y为值, x为优先级。
//通过自定义operator<操作符来比较元素中的优先级。
//在重载”<”时，最好不要重载”>”，可能会发生编译错误
```
2. set 和 multiset

set和   multiset用法一样，就是  multiset允许重复元素。

元素放入容器时，会按照一定的排序法则自动排序，默认是按照 less<>排序规则来排序。不

能修改容器里面的元素值，只能插入和删除。

自定义 int排序函数：（默认的是从小到大的，下面这个从大到小）
```
struct classcomp  {
bool operator() (const int& lhs, const int& rhs)  const
{return  lhs>rhs;}
};//这里有个逗号的，注意
multiset<int,classcomp> fifth;
// class as Compare
上面这样就定义成了从大到小排列了。
结构体自定义排序函数：
（定义 set或者   multiset的时候定义了排序函数，定义迭代器时一样带上排序函数）
struct  Node
{
int  x,y;
};
struct classcomp//先按照 x从小到大排序，x相同则按照y从大到小排序
{
bool  operator()(const Node &a,const Node &b)const
{
if(a.x!=b.x)return  a.x<b.x;
else  return  a.y>b.y;
}
}; //注意这里有个逗号
multiset<Node,classcomp>mt;
multiset<Node,classcomp>::iterator  it;
Multiset      //该函数是set的多重集合形式可保存eg：1 2 2 3 3 4 5
multimap<int, int>a;
int main(){
	a.insert(pair<int,int>(1, 1));
	a.insert(pair<int, int>(2, 1));
	a.insert(pair<int, int>(1, 2));
	for (multimap<int, int>::iterator it = a.begin(); it != a.end(); it++)
		cout << (*it).first<<" "<<(*it).second << endl;
	return 0;
}
```
主要函数：

begin()返回指向第一个元素的迭代器

clear()清除所有元素

count()返回某个值元素的个数

empty()如果集合为空，返回     （true）

end()返回指向最后一个元素的迭代器

erase()删除集合中的元素         (参数是一个元素值，或者迭代器 )

find()返回一个指向被查找到元素的迭代器

insert()在集合中插入元素

size()集合中元素的数目

lower_bound()返回指向大于（或等于）某值的第一个元素的迭代器

upper_bound()返回大于某个值元素的迭代器

equal_range()返回集合中与给定值相等的上下限的两个迭代器

(注意对于  multiset删除操作之间删除值会把所以这个值的都删掉，删除一个要用迭代器  )

3. lower_bound and upper_bound

lower_bound()  //在first和last中的前闭后开区间进行二分查找（故要先sort()），返回大于或等于val的第一个元素位置。如果所有元素都小于val，则返回last的位置（且a[last]不存在）

Eg:    Pos= lower_bound(a,a+N,val)-a;

upper_bound()  //也是前闭后开区间，且返回第一个大于val的位置，如果所有元素都小于val，则返回last的位置。

Eg:    Pos= upper_bound (a,a+N,val)-a;
4.next_permutation and prev_permutation

next_permutation（）    //排列组合使用，eg：1 2 3 4 =》1 2 4 3 =》1 3 2 4 =》…

prev_permutation（）  //和上面的相反，返回上一项结果

Eg:     next_permutation（a,a+N）;               //执行成功返回1，否则返回0

5. reverse()

//可以对数组，字符串，vector等进行翻转操作123=》321

Eg：  reverse(a，a+N);

6. Map
```
map<int, char>m;
	map<int, char>::iterator it;
	m[8] = 'a';
	m[6] = 'b';		
	m[11] = 'c';
	it = m.begin();
	cout << it->first << " " << it->second << endl; //输出6 b
	it = m.end(); it--;
	cout << it->first << " " << it->second << endl; //输出11 c
	//map按照平衡二叉树原理类似，begin（）为最左下角（min）
	                             //end（）为最右下角（max）
```

7. 迭代器
```
在C++11情况下可以使用auto
For（auto it=a.begin();it!=a.end();i++）
任何情况都可以的
for(vector<int>::iterator it=a.begin();it!=a.end();it++)
//注意，这里vector可以这么用，快很多：for(int i=0;i<a.size();i++)  a[i]即为队列中第i个
```