---
title: DP问题归类
tags: DP
categories: DP问题
abbrlink: '42862e9'
date: 2019-07-22 12:42:52
---
DP算法题训练
<!--more-->
### 最长回文子串
给定一个字符串 s，找到 s 中最长的回文子串。你可以假设 s 的最大长度为 1000。

示例 1：

输入: "babad"
输出: "bab"
注意: "aba" 也是一个有效答案。
示例 2：

输入: "cbbd"
输出: "bb"

链接：https://leetcode-cn.com/problems/longest-palindromic-substring
算法思路：
1. 暴力法(会超时)
2. 将字符串s反转得到字符串rev，再求他们的最长公共子串，再判断该最长公共子串是否就是我们要找的最长回文子串。(依然会超时)
3. 动态规划
   初始状态：
   * dp[i][i]=1;//单个字符是回文串
   * dp[i][i+1]=1 if s[i]=s[i+1];//连续两个相同字符是回文串

4. 中心扩展法
   回文中心的两侧互为镜像，因此，回文可以从他的中心展开，并且只有2n-1个这样的中心(一个元素为中心的情况有n个，两个元素为中心的情况有n-1个)
5. Manacher：
   主要思想是，把给定的字符串的每一个字母当做中心，向两边扩展，这样来找最长的子回文串，这个叫中心扩展法，但是这个方法还要考虑到处理 abba 这种偶数个字符的回文串。Manacher 法将所有的字符串全部变成奇数个字符。


代码实现：



```
//1.暴力求解
class Solution{
	public:
	string longestPalindrome(string s)
	{
		string res="";
		string temp="";
		for(int i=0;i<s.length();i++)
		{
			for(int j=i;j<s.length();j++)
			{
				temp=temp+s[j];
				string tem=temp;
				std::reverse(tem.begin(),tem.end());
				if(temp==tem)
				res=res.length()>temp.length()?res:temp;
			}
			temp="";
		}
		return res;
	}
}




//2.
class Solution{
	public:
	    string longestPalindrome(string s)
	    {
	    	if(s.length()==1) return s;
	    	string rev=s;
	    	string res;
	    	std::reverse(rev.begin(),rev.end());
	    	if(rev==s)return s;
	    	int len=0;
	    	for(int i=0;i<s.length();i++)
	    	{
	    		string temp;
	    		for(int j=i;j<s.length();j++)
	    		{
	    			temp=temp+s[j];
	    			if(len>=temp.length())
	    			continue;
	    			else if(rev.find(temp)!=-1)
	    			{
	    				string q=temp;
	    				std::reverse(q.begin(),q.end());
	    				if(q==temp)
	    				{
	    					len=temp.length();
	    					res=temp;
	    				}
	    			}
	    			else break;
	    		}
	    		temp="";
	    	}
	    	return res;
	    }
}


\\DP求解
初始状态：
 * dp[i][i]=1;//单个字符
 * dp[i][i+1] if s[i]=s[i+1];//连续两个相同字符是回文串
  class Solution{
 	public:
 	string longestPalindrome(string s)
 	{
 		int len=s.size();
 		if(len==0||len==1) return s;
 		int start;
 		int max=1;
 		vector<vector<int>> dp(len ,vector<int>(len));
 		for(int i=0;i<len;i++)
 		{
 			dp[i][i]=1;
 			if(i<len-1&&s[i]==s[i+1])
 			{
 				dp[i][i+1]=1;
 				max=2;
 				start=i;
 			}
 		}
 		for(int j=3;j<=len;j++)
 		{
 			for(int i=0;i+j-1<len;i++)
 			{
 				int m=j+i-1;
 				if(s[i]==s[m]&&dp[i+1][m-1]==1)
 				{
 					dp[i][m]=1;
 					start=i;
 					max=j;
 				}
 			}
 		}
 		return s.substr(start,max);

 	}
 };
```
### 解决这类 “最优子结构” 问题，可以考虑使用 “动态规划”：

1. 定义 “状态”；
2. 找到 “状态转移方程”。

> 记号说明： 下文中，使用记号 s[l, r] 表示原始字符串的一个子串，l、r 分别是区间的左右边界的索引值，使用左闭、右闭区间表示左右边界可以取到。
举个例子，当 s = 'babad' 时，s[0, 1] = 'ba' ，s[2, 4] = 'bad'。

1、定义 “状态”，这里 “状态”数组是二维数组。

dp[l][r] 表示子串 s[l, r]（包括区间左右端点）是否构成回文串，是一个二维布尔型数组。即如果子串 s[l, r] 是回文串，那么 dp[l][r] = true。

2、找到 “状态转移方程”。

首先，我们很清楚一个事实：

1、当子串只包含 1 个字符，它一定是回文子串；

2、当子串包含 2 个以上字符的时候：如果 s[l, r] 是一个回文串，例如 “abccba”，那么这个回文串两边各往里面收缩一个字符（如果可以的话）的子串 s[l + 1, r - 1] 也一定是回文串，
即：
```如果 dp[l][r] == true 成立，一定有 dp[l + 1][r - 1] = true 成立。```

   根据这一点，我们可以知道，给出一个子串 s[l, r] ，如果 s[l] != s[r]，那么这个子串就一定不是回文串。如果 s[l] == s[r] 成立，就接着判断 s[l + 1] 与 s[r - 1]，这很像中心扩散法的逆方法。

 事实上，当 s[l] == s[r] 成立的时候，dp[l][r] 的值由 dp[l + 1][r - l] 决定，这一点也不难思考：当左右边界字符串相等的时候，整个字符串是否是回文就完全由“原字符串去掉左右边界”的子串是否回文决定。
 但是这里还需要再多考虑一点点：
 “原字符串去掉左右边界”的子串的边界情况。

1、当原字符串的元素个数为 3 个的时候，如果左右边界相等，那么去掉它们以后，只剩下 1 个字符，它一定是回文串，故原字符串也一定是回文串；

2、当原字符串的元素个数为 2 个的时候，如果左右边界相等，那么去掉它们以后，只剩下 0 个字符，显然原字符串也一定是回文串。
把上面两点归纳一下，只要 s[l + 1, r - 1] 至少包含两个元素，就有必要继续做判断，否则直接根据左右边界是否相等就能得到原字符串的回文性。而“s[l + 1, r - 1] 至少包含两个元素”等价于 l + 1 < r - 1，整理得 l - r < -2，或者 r - l > 2。

综上，如果一个字符串的左右边界相等，以下二者之一成立即可： 
```
1、去掉左右边界以后的字符串不构成区间，即“ s[l + 1, r - 1] 至少包含两个元素”的反面，即 l - r >= -2，或者 r - l <= 2； 2、去掉左右边界以后的字符串是回文串，具体说，它的回文性决定了原字符串的回文性。
```
于是整理成“状态转移方程”：
```
dp[l, r] = (s[l] == s[r] and (l - r >= -2 or dp[l + 1, r - 1]))
```
或者
```
dp[l, r] = (s[l] == s[r] and (r - l <= 2 or dp[l + 1, r - 1]))
```
编码实现细节：因为要构成子串 l 一定小于等于 r ，我们只关心 “状态”数组“上三角”的那部分取值。
```
理解上面的“状态转移方程”中的 (r - l <= 2 or dp[l + 1, r - 1]) 这部分是关键，因为 or 是短路运算，因此，如果收缩以后不构成区间，那么就没有必要看继续 dp[l + 1, r - 1] 的取值。
```
读者可以思考一下：为什么在动态规划的算法中，不用考虑回文串长度的奇偶性呢。想一想，答案就在状态转移方程里面。

具体编码细节在代码的注释中已经体现。

参考代码：
Python:
```
class Solution:
    def longestPalindrome(self, s: str) -> str:
        size = len(s)
        if size <= 1:
            return s
        # 二维 dp 问题
        # 状态：dp[l,r]: s[l:r] 包括 l，r ，表示的字符串是不是回文串
        # 设置为 None 是为了方便调试，看清楚代码执行流程
        dp = [[False for _ in range(size)] for _ in range(size)]

        longest_l = 1
        res = s[0]

        # 因为只有 1 个字符的情况在最开始做了判断
        # 左边界一定要比右边界小，因此右边界从 1 开始
        for r in range(1, size):
            for l in range(r):
                # 状态转移方程：如果头尾字符相等并且中间也是回文
                # 在头尾字符相等的前提下，如果收缩以后不构成区间（最多只有 1 个元素），直接返回 True 即可
                # 否则要继续看收缩以后的区间的回文性
                # 重点理解 or 的短路性质在这里的作用
                if s[l] == s[r] and (r - l <= 2 or dp[l + 1][r - 1]):
                    dp[l][r] = True
                    cur_len = r - l + 1
                    if cur_len > longest_l:
                        longest_l = cur_len
                        res = s[l:r + 1]
            # 调试语句
            # for item in dp:
            #     print(item)
            # print('---')
        return res


```

Java:
```
class Solution {
    public String longestPalindrome(String s) {
        int len=s.length();
        if(len<=1)
            return s;
        int longestPalindrome=1;
        String longestPalindromeStr=s.substring(0,1);
        boolean[][] dp=new boolean[len][len];
        // abcdedcba
        //   l   r
        // 如果 dp[l, r] = true 那么 dp[l + 1, r - 1] 也一定为 true
        // 关键在这里：[l + 1, r - 1] 一定至少有 2 个元素才有判断的必要
        // 因为如果 [l + 1, r - 1] 只有一个元素，不用判断，一定是回文串
        // 如果 [l + 1, r - 1] 表示的区间为空，不用判断，也一定是回文串
        // [l + 1, r - 1] 一定至少有 2 个元素 等价于 l + 1 < r - 1，即 r - l >  2

        // 写代码的时候这样写：如果 [l + 1, r - 1]  的元素小于等于 1 个，即 r - l <=  2 ，就不用做判断了

        // 因为只有 1 个字符的情况在最开始做了判断
        // 左边界一定要比右边界小，因此右边界从 1 开始
        for(int r=1;r<len;r++)
        {
            for(int l=0;l<r;l++)
            {
            	// 区间应该慢慢放大
                // 状态转移方程：如果头尾字符相等并且中间也是回文
                // 在头尾字符相等的前提下，如果收缩以后不构成区间（最多只有 1 个元素），直接返回 True 即可
                // 否则要继续看收缩以后的区间的回文性
                // 重点理解 or 的短路性质在这里的作用

                if(s.charAt(l)==s.charAt(r)&&(l+1>=r-1|| dp[l+1][r-1])){
                    dp[l][r]=true;
                    if(r-l+1>longestPalindrome)
                    {
                        longestPalindrome=r-l+1;
                        longestPalindromeStr=s.substring(l,r+1);
                    }
                }
            }
        }
        return longestPalindromeStr;
    }
}
```
### 正则表达式匹配

给你一个字符串 s 和一个字符规律 p，请你来实现一个支持 '.' 和 '* ' 的正则表达式匹配。
```
'.' 匹配任意单个字符
'*' 匹配零个或多个前面的那一个元素
所谓匹配，是要涵盖 整个 字符串 s的，而不是部分字符串。
```
说明:
```
s 可能为空，且只包含从 a-z 的小写字母。
p 可能为空，且只包含从 a-z 的小写字母，以及字符 . 和 *。
示例 1:
```
输入:
```
s = "aa"
p = "a"
输出: false
解释: "a" 无法匹配 "aa" 整个字符串。
```
示例 2:
```
输入:
s = "aa"
p = "a*"
输出: true

解释: 因为 '*' 代表可以匹配零个或多个前面的那一个元素, 在这里前面的元素就是 'a'。因此，字符串 "aa" 可被视为 'a' 重复了一次。
```
示例 3:
```
输入:
s = "ab"
p = ".*"
输出: true
解释: ".*" 表示可匹配零个或多个（'*'）任意字符（'.'）。
```
示例 4:
```
输入:
s = "aab"
p = "c*a*b"
输出: true
解释: 因为 '*' 表示零个或多个，这里 'c' 为 0 个, 'a' 被重复一次。因此可以匹配字符串 "aab"。
```
示例 5:
```
输入:
s = "mississippi"
p = "mis*is*p*."
输出: false

```
链接：https://leetcode-cn.com/problems/regular-expression-matching

算法思路：
方法一：回溯
    当pattern(模式串)中由* 时，需要检查匹配串s 中的不同后缀，以判断它们能否匹配模式串剩余的部分。
    

方法二：DP


    最优子结构：动态规划做，将中间结果保存，通过dp(i,j) 表示text[i:] 和pattern[j:] 是否能匹配。
    我们可以用更短的字符串匹配问题来表示原本的问题。

如果模式串中由星号，则pattern[1]，这种情况下，我们可以直接忽略模式串中这一部分，或者删除匹配串中的第一个字符，前提是它能够匹配模式串当前位置字符，即pattern[0] 。如果两种操作中由任何一种使得剩下的字符串能够匹配，那么初始时，匹配串和模式串就可以被匹配。


参考代码：
Java
```
class Solution {
    public boolean isMatch(String text, String pattern) {
        if (pattern.isEmpty()) return text.isEmpty();
        boolean first_match = (!text.isEmpty() &&
                               (pattern.charAt(0) == text.charAt(0) || pattern.charAt(0) == '.'));

        if (pattern.length() >= 2 && pattern.charAt(1) == '*'){
            return (isMatch(text, pattern.substring(2)) ||
                    (first_match && isMatch(text.substring(1), pattern)));
        } else {
            return first_match && isMatch(text.substring(1), pattern.substring(1));
        }
    }
}

```
### 最长有效括号
给定一个只包含 '(' 和 ')' 的字符串，找出最长的包含有效括号的子串的长度。

示例 1:

输入: "(()"
输出: 2
解释: 最长有效括号子串为 "()"
示例 2:

输入: ")()())"
输出: 4
解释: 最长有效括号子串为 "()()"
链接：https://leetcode-cn.com/problems/longest-valid-parentheses
算法思路：
1. 暴力
   遇到"("，放到栈顶，遇到每个")"，从栈中弹出一个"("，
如果栈顶没有"(",或者遍历完整个子字符串后栈中仍然有元素，则该子字符串是无效的。
    此种方法对每个偶数长度子字符串都进行了判断，并保存目前为止找到的最长的有效子字符串的长度。

参考代码：
```
   public class Solution{
   	public boolean isValid(String s)
   	{
   		Stack<Character> stack=new Stack<Character>();
   		for(int i=0;i<s.length();i++)
   		{
   			if(s.charAt(i)=='(')
   			{
   				stack.push('(');
   			}
   			else if (!stack.empty()&& stack.peek()=='('){
   				stack.pop();
   			}
   			else
   			{
   			   return false;
   			}
   		}
   		return stack.empty();
   	}
     public int longestValidParentheses(String s)
     {
     	int maxlen=0;
     	for(int i=0;i<s.length();i++)
     	{
     		for(int j=i+2;j<=s.length();j+=2)
     		{
     			if(isValid(s.substring(i,j)))
     			{
     				maxlen=Math.max(maxlen,j-i);
     			}
     		}
     	}
     	return maxlen;
     }
   }
```
时间复杂度： O(n^2)
从长度为n的字符串产生所有可能的子字符串需要的时间复杂度为 O(n^2)。
验证一个长度为 n 的子字符串需要的时间为 O(n) 。

空间复杂度： O(n)
子字符串的长度最多会需要一个深度为 n的栈。

2. DP动态规划
算法：
   定义一个dp数组，其中第i个元素表示以下标为i的字符结尾的最长有效字符串的长度。
   将dp数组全部初始化为0，以'('结尾的子字符串对应的dp数组位置上的值必定为0,因此只需要更新')'在dp数组中对应位置的值。


  为求出dp数组，每两个字符将检查一次，如果满足如下条件：
      1. s[i]=')'且s[i-1]='(', 也就是形如"......()"，我们可以推出：
          dp[i]=dp[i-2]+2
       因为结束部分的"()"是一个有效子字符串，并且将之前有效子字符串的长度增加了2
      2. s[i]=')' 且s[i-1]=')',也就是形如".....))",我们可以推出：
      如果s[i-dp[i-1]-1]='(',那么
          dp[i]=dp[i-1]+dp[i-dp[i-1]-2]+2

      因为如果倒数第二个')'是一个有效子字符串的一部分(subs),对于最后一个')'，如果它是一个更长子字符串的一部分，那么它一定有一个对应的'('，它的位置在倒数第二个')'所在的有效子字符串的前面(subs).

      因此，如果子字符串subs的前面恰好是'('，那么就用2加上subs的长度(dp[i-1])去更新dp[i]，除此以外，我们也会把有效子字符串"(,subs,)"之前的有效子字符串的长度也加上，也就是加上dp[i-dp[i-1]-2]。

时间复杂度：O(n)，遍历整个字符串一次，就可以将dp数组求出来。
空间复杂度:O(n),需要一个大小为n的dp数组。
参考代码：
Java
```
public class Solution{
	public int longestValidParentheses(String s)
	{
		int maxans=0;
		int dp[]=new int[s.length()];
		for(int i=1;i<s.length();i++)
		{
			if(s.charAt(i)==')')
			{
				if(s.charAt(i-1)=='(')
				{
					dp[i]=(i>=2?dp[i-2]:0)+2;
				}
				else if(i-dp[i-1]>0&&s.charAt(i-dp[i-1]-1)=='(')
				{
					dp[i]=dp[i-1]+((i-dp[i-1])>=2?dp[i-dp[i-1]-2]:0)+2;
				}
				maxans=Math.max(maxans,dp[i]);
			}
		}
		return maxans;
	}
}


```
### 通配符匹配
给定一个字符串 (s) 和一个字符模式 (p) ，实现一个支持 '?' 和 ' * ' 的通配符匹配。
```
'?' 可以匹配任何单个字符。
'*' 可以匹配任意字符串（包括空字符串）。
两个字符串完全匹配才算匹配成功。
```
说明:
```
s 可能为空，且只包含从 a-z 的小写字母。
p 可能为空，且只包含从 a-z 的小写字母，以及字符 ? 和 *。
```
示例 1:
```
输入:
s = "aa"
p = "a"
输出: false
解释: "a" 无法匹配 "aa" 整个字符串。
```
示例 2:
```
输入:
s = "aa"
p = "*"
输出: true
解释: '*' 可以匹配任意字符串。
```
示例 3:
```
输入:
s = "cb"
p = "?a"
输出: false
解释: '?' 可以匹配 'c', 但第二个 'a' 无法匹配 'b'。
```

示例 4:
```

输入:
s = "adceb"
p = "*a*b"
输出: true
解释: 第一个 '*' 可以匹配空字符串, 第二个 '*' 可以匹配字符串 "dce".
```
示例 5:
```
输入:

s = "acdcb"
p = "a*c?b"
输出: false
```
链接：https://leetcode-cn.com/problems/wildcard-matching
算法思路：
  1. 双指针遍历  时间复杂度:O(mn)
  2. 动态规划DP  
  dp[i][j]表示s到i位置，p到j位置是否匹配。
  初始化：
  1. dp[0][0]:true
  2. 第一行dp[0][j]，即就是s为空，与p匹配，所以只要p开始为 * 才为true
  3. 第一列dp[i][0],全部为False.
  动态方程：
  1. 如果(s[i]==p[j]||p[j]=="?")&&dp[i-1][j-1],有dp[i][j]=true
  2. 如果p[j]==" * "&&(dp[i-1][j]=true||dp[i][j-1]=true) 有dp[i][j]=true
      dp[i-1][j],表示 * 代表的是非空任何字符，例如：abcd,ab*
      dp[i][j-1]，表示 * 代表空字符，例如：ab,ab*


参考代码：
```
class Solution{
	public boolean isMatch(String s,String p)
	{
		boolean[][] dp=new boolean[
		s.length()+1][p.length() +1];
		dp[0][0]=true;
		for(int j=1;j<p.length()+1;j++){
			if(p.charAt(j-1)=='*'){
				dp[0][j]=dp[0][j-1];
			}
		}
		for(int i=1;i<s.length()+1;i++)
		{
			for(int j=1;j<p.length()+1;j++)
			{
				if(s.charAt(i-1)==p.charAt(j-1)||p.charAt(j-1)=='?')
				{
					dp[i][j]=dp[i-1][j-1];
				}
				else if(p.charAt(j-1)=='*')
				{
					dp[i][j]=dp[i][j-1]||dp[i-1][j];
				}
			}
		}
		return dp[s.length()][p.length()];

	}
}


```

###  最大子序和
给定一个整数数组 nums ，找到一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。

示例:

输入: [-2,1,-3,4,-1,2,1,-5,4],
输出: 6
解释: 连续子数组 [4,-1,2,1] 的和最大，为 6。
进阶:
如果你已经实现复杂度为 O(n) 的解法，尝试使用更为精妙的分治法求解。
链接：https://leetcode-cn.com/problems/maximum-subarray

算法思路：
      动态规划算法，对数组遍历，当前最大连续子序列和为sum,结果为ans.
  * 如果 sum>0,则说明sum对结果有增益效果，则sum保留并加上当前遍历数字
  * 如果sum<=0 ，则说明sum对结果无增益效果，需要舍弃，则sum直接更新为当前遍历数字
  * 每次比较sum 和ans的大小，将最大值置为ans ,遍历结束返回结果
  时间复杂度:O(n)
  参考代码：
  Java
  ```
class Solution{
	public int maxSubArray(int[] nums)
	{
		int ans=nums[0];
		int sum=0;
		for(int num:nums)
		{
			if(sum>0)
			{
				sum+=num;
			}
			else{
				sum=num;
			}
			ans=Math.max(ans,sum);
		}
		return ans;
	}
}


```

### 买卖股票的最佳时机
给定一个数组，它的第 i 个元素是一支给定股票第 i 天的价格。

如果你最多只允许完成一笔交易（即买入和卖出一支股票），设计一个算法来计算你所能获取的最大利润。

注意你不能在买入股票前卖出股票。

示例 1:
```
输入: [7,1,5,3,6,4]
输出: 5
解释: 在第 2 天（股票价格 = 1）的时候买入，在第 5 天（股票价格 = 6）的时候卖出，最大利润 = 6-1 = 5 。
     注意利润不能是 7-1 = 6, 因为卖出价格需要大于买入价格。
     ```
示例 2:
```
输入: [7,6,4,3,1]
输出: 0
解释: 在这种情况下, 没有交易完成, 所以最大利润为 0。
```
链接：https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock
算法思路：
1. 暴力法
   找出最大利润，即就是找出给定数组两个数字之间的最大差值，且第二个数字必须大于第一个数字
    对于每组i和j(其中j>i)我们需要找出max(prices[j]-prices[i])
    
参考代码：
```
class Solution {
    public int maxProfit(int[] prices) {
        int maxprofit=0;
        for(int i=0;i<prices.length;i++)
        {
            for(int j=i+1;j<prices.length;j++)
            {
                int profit=prices[j]-prices[i];
                if(profit>maxprofit)
                    maxprofit=profit;
            }
        }
        return maxprofit;
    }
}
```
时间复杂度：O(n^2)
空间复杂度：O(1)

```
public class Solution{
	public int maxProfit(int prices[])
	{
		int minprice=Integer.MAX_VALUE;
		int maxprofit=0;
		for(int i=0;i<prices.length;i++)
		{
			if(prices[i]<minprice)
			minprice=prices[i];
			else
			 if(prices[i]-minprice>maxprofit)
			  maxprofit=prices[i]-minprice;
		}
		return maxprofit;
	}
}


```
时间复杂度：O(n)
空间复杂度：O(1)


### 最短路径和
给定一个包含非负整数的 m x n 网格，请找出一条从左上角到右下角的路径，使得路径上的数字总和为最小。

说明：每次只能向下或者向右移动一步。

示例:

输入:
[
  [1,3,1],
  [1,5,1],
  [4,2,1]
]
输出: 7
解释: 因为路径 1→3→1→1→1 的总和最小。

链接：https://leetcode-cn.com/problems/minimum-path-sum

算法思路：
    找出一条从左上角到右下角的路径，路径上数字之和最小。
1. 暴力算法
    cost(i,j)=grid[i][j]+min(cost(i+1,j),cost(i,j+1))
时间复杂度：O(2^(m+n))，每次移动最多有两种选择
空间复杂度:O(m+n)，递归深度是m+n
2. DP算法 二维动态规划
    新建一个额外的dp数组，与原矩阵大小相同，在这个矩阵中，dp(i,j)表示从坐标(i,j)到右下角的最小路径权值，我们初始化右下角的dp值为对应的原矩阵值，然后填充整个矩阵，对于每个元素考虑移动到右边或者下面，因此获得最小路径，递推公式如下：
    dp(i,j)=grid(i,j)+min(dp(i+1,j),dp(i,j+1))
时间复杂度:O(mn)。遍历整个矩阵恰好一次
空间复杂度:O(mn)。额外的一个同大小矩阵。
3. 一维动态规划
   我们可以用一个一维数组来代替二维数组，dp数组的大小和行大小相同，这是因为对于某个固定状态，只需要考虑下方和右侧的节点。
   首先初始化dp数组最后一个元素是右下角的元素值，然后我们向左更新每个dp(j)为：
    dp(j)=grid(i,j)+min(dp(j),dp(j+1))
    我们对于每一行都重复这个过程，然后向上一行移动，计算完成后dp(0)就是最后的结果。

 时间复杂度 ：O(mn)遍历整个矩阵恰好一次。
 空间复杂度 ：O(n)额外的一维数组，和一行大小相同。
4. 动态规划（不需要额外存储空间）
  与二维DP算法相同，但是不需要用额外的dp数组，而是在原数组上存储，这样就不需要额外的存储空间。递推公式如下：
  grid(i,j)=grid(i,j)+min(grid(i+1,j),grid(i,j+1))

  时间复杂度：
   O(mn)。遍历整个矩阵恰好一次。

  空间复杂度：
  O(1)。不需要额外空间。
参考代码：
```
//暴力
class Solution {
    public int calculate(int [][] grid ,int i,int j)        
    {
        if(i==grid.length||j==grid[0].length)
            return Integer.MAX_VALUE;
        if(i==grid.length-1 && j==grid[0].length-1)
            return grid[i][j];
        return grid[i][j]+Math.min(calculate(grid,i+1,j),calculate(grid,i,j+1));
    }

    public int minPathSum(int[][] grid) {
        return calculate(grid,0,0);
    }
}
//二维DP算法
class Solution {
    public int minPathSum(int[][] grid) {
        int [][]dp=new int[grid.length][grid[0].length];
        for(int i=grid.length-1;i>=0;i--)
        {
            for(int j=grid[0].length-1;j>=0;j--)
            {
                if(i==grid.length-1&&j!=grid[0].length-1)
                    dp[i][j]=grid[i][j]+dp[i][j+1];
                else if(j==grid[0].length-1&&i!=grid.length-1)
                    dp[i][j]=grid[i][j]+dp[i+1][j];
                else if(j!=grid[0].length&&i!=grid.length-1)
                    dp[i][j]=grid[i][j]+Math.min(dp[i+1][j],dp[i][j+1]);
                else 
                    dp[i][j]=grid[i][j];
            }
        }
        return dp[0][0];
    }
}
//一维动态规划
class Solution {
    public int minPathSum(int[][] grid) {
        int []dp=new int[grid[0].length];
        for(int i=grid.length-1;i>=0;i--)
        {
            for(int j=grid[0].length-1;j>=0;j--)
            {
                if(i==grid.length-1&&j!=grid[0].length-1)
                    dp[j]=grid[i][j]+dp[j+1];
                else if(i!=grid.length-1&&j==grid[0].length-1)
                    dp[j]=grid[i][j]+dp[j];
                else if(i!=grid.length-1&&j!=grid[0].length-1)
                    dp[j]=grid[i][j]+Math.min(dp[j],dp[j+1]);
                else
                    dp[j]=grid[i][j];
            }
        }
        return dp[0];
    }
}
//不需要存储空间的DP
class Solution {
    public int minPathSum(int[][] grid) {
        for(int i=grid.length-1;i>=0;i--)
        {
            for(int j=grid[0].length-1;j>=0;j--)
            {
                if(i==grid.length-1&&j!=grid[0].length-1)
                grid[i][j]=grid[i][j]+grid[i][j+1];
                else if(i!=grid.length-1&&j==grid[0].length-1)
                    grid[i][j]=grid[i][j]+grid[i+1][j];
                else if(i!=grid.length-1&&j!=grid[0].length-1)
                    grid[i][j]=grid[i][j]+Math.min(grid[i][j+1],grid[i+1][j]);
                    else 
                        grid[i][j]=grid[i][j];
            }
        }
        return grid[0][0];
    }
}


```

### 扰乱字符串
给定一个字符串 s1，我们可以把它递归地分割成两个非空子字符串，从而将其表示为二叉树。

下图是字符串 s1 = "great" 的一种可能的表示形式。

    great
   /    \
  gr    eat
 / \    /  \
g   r  e   at
           / \
          a   t

在扰乱这个字符串的过程中，我们可以挑选任何一个非叶节点，然后交换它的两个子节点。

例如，如果我们挑选非叶节点 "gr" ，交换它的两个子节点，将会产生扰乱字符串 "rgeat" 。

    rgeat
   /    \
  rg    eat
 / \    /  \
r   g  e   at
           / \
          a   t

我们将 "rgeat” 称作 "great" 的一个扰乱字符串。

同样地，如果我们继续将其节点 "eat" 和 "at" 进行交换，将会产生另一个新的扰乱字符串 "rgtae" 。

    rgtae
   /    \
  rg    tae
 / \    /  \
r   g  ta  e
       / \
      t   a

我们将 "rgtae” 称作 "great" 的一个扰乱字符串。

给出两个长度相等的字符串 s1 和 s2，判断 s2 是否是 s1 的扰乱字符串。

示例 1:

输入: s1 = "great", s2 = "rgeat"
输出: true

示例 2:

输入: s1 = "abcde", s2 = "caebd"
输出: false
链接：https://leetcode-cn.com/problems/scramble-string


算法思路：
1. 递归
  第 1 种情况： S1 切割为两部分，然后进行若干步切割交换，最后判断两个子树分别是否能变成 S2 的两部分。
 第 2 种情况：S1 切割并且交换为两部分，然后进行若干步切割交换，最后判断两个子树是否能变成 S2 的两部分。
2. DP动态规划


参考代码：
```
//递归 6ms
public boolean isScramble(String s1, String s2) {
    if (s1.length() != s2.length()) {
        return false;
    }
    if (s1.equals(s2)) {
        return true;
    }

    //判断两个字符串每个字母出现的次数是否一致
    int[] letters = new int[26];
    for (int i = 0; i < s1.length(); i++) {
        letters[s1.charAt(i) - 'a']++;
        letters[s2.charAt(i) - 'a']--;
    }
    //如果两个字符串的字母出现不一致直接返回 false
    for (int i = 0; i < 26; i++) {
        if (letters[i] != 0) {
            return false;
        }
    } 
    //遍历每个切割位置
    for (int i = 1; i < s1.length(); i++) {
        //对应情况 1 ，判断 S1 的子树能否变为 S2 相应部分
        if (isScramble(s1.substring(0, i), s2.substring(0, i)) && isScramble(s1.substring(i), s2.substring(i))) {
            return true;
        }
        //对应情况 2 ，S1 两个子树先进行了交换，然后判断 S1 的子树能否变为 S2 相应部分
        if (isScramble(s1.substring(i), s2.substring(0, s2.length() - i)) &&
           isScramble(s1.substring(0, i), s2.substring(s2.length() - i)) ) {
            return true;
        }
    }
    return false;
}
//动态规划 34ms
public boolean isScramble(String s1, String s2) {
    if (s1.length() != s2.length()) {
        return false;
    }
    if (s1.equals(s2)) {
        return true;
    }

    int[] letters = new int[26];
    for (int i = 0; i < s1.length(); i++) {
        letters[s1.charAt(i) - 'a']++;
        letters[s2.charAt(i) - 'a']--;
    }
    for (int i = 0; i < 26; i++) {
        if (letters[i] != 0) {
            return false;
        }
    }

    int length = s1.length();
    boolean[][][] dp = new boolean[length + 1][length][length];
  //遍历所有的字符串长度
    for (int len = 1; len <= length; len++) {
        //S1 开始的地方
        for (int i = 0; i + len <= length; i++) {
            //S2 开始的地方
            for (int j = 0; j + len <= length; j++) {
                //长度是 1 无需切割
                if (len == 1) {
                    dp[len][i][j] = s1.charAt(i) == s2.charAt(j);
                } else {
                    //遍历切割后的左半部分长度
                    for (int q = 1; q < len; q++) {
                        dp[len][i][j] = dp[q][i][j] && dp[len - q][i + q][j + q]
                            || dp[q][i][j + len - q] && dp[len - q][i + q][j];
                        //如果当前是 true 就 break，防止被覆盖为 false
                        if (dp[len][i][j]) {
                            break;
                        }
                    }
                }
            }
        }
    }
    return dp[length][0][0];
}


```














## 最小的必要团队
作为项目经理，你规划了一份需求的技能清单 req_skills，并打算从备选人员名单 people 中选出些人组成一个「必要团队」（ 编号为 i 的备选人员 people[i] 含有一份该备选人员掌握的技能列表）。

所谓「必要团队」，就是在这个团队中，对于所需求的技能列表 req_skills 中列出的每项技能，团队中至少有一名成员已经掌握。

我们可以用每个人的编号来表示团队中的成员：例如，团队 team = [0, 1, 3] 表示掌握技能分别为 people[0]，people[1]，和 people[3] 的备选人员。

请你返回 任一 规模最小的必要团队，团队成员用人员编号表示。你可以按任意顺序返回答案，本题保证答案存在。

 

示例 1：

输入：req_skills = ["java","nodejs","reactjs"], people = [["java"],["nodejs"],["nodejs","reactjs"]]
输出：[0,2]
示例 2：

输入：req_skills = ["algorithms","math","java","reactjs","csharp","aws"], people = [["algorithms","math","java"],["algorithms","math","reactjs"],["java","csharp","aws"],["reactjs","csharp"],["csharp","math"],["aws","java"]]
输出：[1,2]
 

提示：
1 <= req_skills.length <= 16
1 <= people.length <= 60
1 <= people[i].length, req_skills[i].length, people[i][j].length <= 16
req_skills 和 people[i] 中的元素分别各不相同
req_skills[i][j], people[i][j][k] 都由小写英文字母组成
本题保证「必要团队」一定存在

链接：https://leetcode-cn.com/problems/smallest-sufficient-team
算法思路：
典型的背包问题，用位图实现，递推公式
dp[x|y]=min(dp[x|y],dp[x]+dp[y])

S表示一个二进制集合，S中第i位是1表示该集合包含标号是i的技能。
令dp[S]表示要获得集合S表示的技能的最小花费，也就是最少需要选多少人。
假设技能个数是n，那么要求的答案就是dp[(1< < n ) - n ]
对于状态转移方程：
假设当前第i个人的技能集合是now，我们就拿当前的技能集合now去更新每一个dp[now|j],0<=j<(1< < n)的值，因为要记录最后所选的答案，所以拿一个team数组维护一下。
时间复杂度O(m * 2^n),m是人的个数，n是技能个数

代码实现：
C++:
```
class Solution
{
	public:
	vector<int> smallestSufficientTeam(vector<string>& req_skills,vector<vector<string>>& people)
	unordered_map<string,int> mp;
	int n=req_skills.size();
	for(int i = 0 ;i < n; ++i) mp[req_skills[i]]=i;
	vector<int> dp(1<<n,-1);
	vector<int> team[1<<n];
	dp[0]=0;
	for(int i=0;i<people.size();++i)
	{
		int now=0;
		for(string s: people[i])
		{
			int x=mp[s];
			now|=(1<<x);
		}
		for(int j=0;j<(1<<n);++j)
		{
			if(dp[j]>=0)
			{
				int x=now|j;
				if(dp[x]==-1||dp[x]>dp[j]+1)
				{
					dp[x]=dp[j]+1;
					team[x]=team[j];
					team[x].push_back(i);
				}
			}
		}
	}
	return team[(1<<n)-1];

}

```
本题是一个 集合覆盖问题，决定性问题 的集合覆盖是 NP完全问题，最佳化问题的集合覆盖是NP困难问题。所以想得到最优解（之一），只能用暴力搜索。好在数据范围并不大，最大状态空间也只有 2^{16}=65,536‬2 种状态，也就是 1616 个人每个人有选和不选两种情况。我们可以用动态规划的方法进行搜索。先将 req_skills 的全集建立一个字典，对每个技能进行编号 0 ~ n-1 。然后建立 dp 数组，长度为 2^n2 
  ，数组元素初始化为 people 的全集，然后对 dp[0] 初始化为空集。然后我们遍历每个人，对于所有状态，计算把这个人加入团队后，整个团队的技能是否增加，如果增加并且人数比拥有相同技能的团队更优化，则更新结果。最终，全集 dp[(1 << n) - 1] 中的 people 集合就是我们要求的结果。
python:
```
class Solution:
    def smallestSufficientTeam(self, req_skills: List[str], people: List[List[str]]) -> List[int]:
        # 为skills建立字典
        n = len(req_skills)
        d = dict()
        for i in range(n):
            d[req_skills[i]] = i
        # 所有状态
        dp = [list(range(len(people))) for _ in range(1 << n)]
        dp[0] = []
        # 遍历所有人
        for i in range(len(people)):
            # 求这个人的技能
            skill = 0
            for s in people[i]:
                skill |= (1 << d[s])
            for k, v in enumerate(dp):
                # 把这个人加入进来以后的团队技能
                new_skills = k | skill
                # 如果团队技能因此而增加 并且增加后的人数比新技能原来的人数少 则更新答案
                if new_skills != k and len(dp[new_skills]) > len(v) + 1:
                    dp[new_skills] = v + [i]
        return dp[(1 << n) - 1]
```