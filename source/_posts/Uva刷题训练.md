---
title: Uva、LeetCode刷题训练
tags: 刷题
categories: 算法
abbrlink: 43f7ae2f
date: 2019-07-18 14:45:32
password: 753159
message: Welcome to my blog, enter password to read.  
abstract: Welcome to my blog, enter password to read.  
---
刷题训练
<!--more-->
### Longest Common Prefix

题目链接：https://leetcode.com/problems/longest-common-prefix/

###### 题目描述
```
Write a function to find the longest common prefix string amongst an array of strings.

If there is no common prefix, return an empty string "".

Example 1:
Input: ["flower","flow","flight"]
Output: "fl"

Example 2:
Input: ["dog","racecar","car"]
Output: ""
Explanation: There is no common prefix among the input strings.

Note:
All given inputs are in lowercase letters a-z.
```
__题解：__
* 当字符串数组长度为 0 时则公共前缀为空，直接返回
* 令最长公共前缀 ans 的值为第一个字符串，进行初始化
* 遍历后面的字符串，依次将其与 ans 进行比较，两两找出公共前缀，最终结果即为最长公共前缀
* 如果查找过程中出现了 ans 为空的情况，则公共前缀不存在直接返回
* 时间复杂度：O(s)O(s)，s 为所有字符串的长度之和


C++：
```
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {
        string ans = "";
        if(strs.empty())    return ans; //输入为空，输出空ans
        int arr = strs.size();
        string min = strs[0];
        for(int i = 1; i < arr; ++ i) //找到最短字符串
        {
            if(strs[i].size() < min.size())
                min = strs[i];
        }
        for(int j = 0; j < min.size(); ++ j) //从第一个字符开始对比，若都一样，ans加上该字符，若不一样，返回答案；
        {
            for(int m = 0; m < arr; ++m)
            {
                if(min[j] != strs[m][j])
                    return ans;
            }
            ans = ans + min[j];
        }
        return ans;
    }
};

```
Java:
```
class Solution {
    public String longestCommonPrefix(String[] strs) {
        if(strs.length==0)
            return "";
        String ans=strs[0];
        for(int i=1;i<strs.length;i++)
        {        int j=0;    
            for(;j<ans.length()&&j<strs[i].length();j++)
            { 
                if(ans.charAt(j)!=strs[i].charAt(j))
                    break;
            } 
        ans=ans.substring(0,j);
        if(ans.equals(""))
            return ans;
        }
      return ans;
    }
 
}

```
python:
```
class Solution:
    def longestCommonPrefix(self, strs: List[str]) -> str:
        if strs == []:         # 字符串为空返回''
            return ''
        elif len(strs) == 1:   # 若列表仅有一个单词，返回该单词
            return strs[0]
        else:
            minlen = len(min(strs, key=len))  # 找到最短的单词长度
            s = ''
            for i in range(1, minlen + 1):    # 最多遍历 minlen 次
                # 每次遍历同时从每个单词取相同长度的前缀加入集合，集合中不会有重复单词
                # 列表中每个单词的相同长度前缀相同，因此集合长度为1
                if len({s[:i] for s in strs}) == 1: 
                    # 将s值更新为当前s长度与任意一个单词[:i]的部分中较大的那个值
                    # 为了避免不存在strs[1]，此处默认为第一个
                    s = max(s, strs[0][:i])          
            return s

```
JS:
```
/**
 * @param {string[]} strs
 * @return {string}
 */
var longestCommonPrefix = function(strs) {
    if(strs.length==0)
        return "";
    let ans=strs[0];
    for(let i=1;i<strs.length;i++)
    {  
        let j=0;
      for(;j<ans.length&&j<strs[i].length;j++)
       { 
           if(ans[j]!=strs[i][j])
            break;
       }
    ans=ans.substr(0,j);
    if(ans=="")
        return ans;
    }  
    return ans;
};
```






参考LeetCode前端代码

二分法

* 找到字符串数组中长度最短字符串
* longest common prefix 长度范围 0 ~ minLength
* 运用binary search
参考代码:
```
/*
 * @lc app=leetcode id=14 lang=javascript
 *
 * [14] Longest Common Prefix
 */

function isCommonPrefix(strs, middle) {
  const prefix = strs[0].substring(0, middle);
  for (let i = 1; i < strs.length; i++) {
    if (!strs[i].startsWith(prefix)) return false;
  }

  return true;
}



/**
 * @param {string[]} strs
 * @return {string}
 */


var longestCommonPrefix = function(strs) {
  // trie 解法
  // 时间复杂度O(m) 空间复杂度O(m * n)

  // tag: 二分法
  // 时间复杂度 O(n*logm)  空间复杂度O(1)
  if (strs.length === 0) return "";
  if (strs.length === 1) return strs[0];

  let minLen = Number.MAX_VALUE;

  for (let i = 0; i < strs.length; i++) {
    minLen = Math.min(minLen, strs[i].length);
  }

  let low = 0;
  let high = minLen;

  while (low <= high) {
    const middle = (low + high) >> 1;
    if (isCommonPrefix(strs, middle)) low = middle + 1;
    else high = middle - 1;
  }

  return strs[0].substring(0, (low + high) >> 1);
};
```


### Implement Trie

以LeetCode另一道题Implement Trie的解法作为本题的参考思路, 具体代码可以自行补充完善

* 建立 Trie
* 遍历到有一个children有超过一个子元素为止
Trie实现参考代码:
Python:
```
class Trie:
    def __init__(self):
        self.dic={}
    def insert(self,strr):
        a=self.dic
        for i in strr:
            if not i in a:
                a[i]={}
            a=a[i]
        a["end"]=True
    def search(self,strr):
        a=self.dic
        for i in strr:
            if not i in a:
                return False
            a=a[i]
        if "end" in a:
            return True
        else:
            return False
    def startsWith(self,strr):
        a=self.dic
        for i in strr:
            if not i in a:
                return False
            a=a[i]
        return True
```
C++:
```
class Trie {
public:
    /** Initialize your data structure here. */
    Trie() {
        
    }
    
    /** Inserts a word into the trie. */
    void insert(string word) {
        
    }
    
    /** Returns if the word is in the trie. */
    bool search(string word) {
        
    }
    
    /** Returns if there is any word in the trie that starts with the given prefix. */
    bool startsWith(string prefix) {
        
    }
};

/**
 * Your Trie object will be instantiated and called as such:
 * Trie* obj = new Trie();
 * obj->insert(word);
 * bool param_2 = obj->search(word);
 * bool param_3 = obj->startsWith(prefix);
 */
```
Java：
```
class Trie {
    public boolean isWord;
    public char word;
    public Trie[] tries = new Trie[26];

    /** Initialize your data structure here. */
    public Trie() {
        this.isWord =false;
        this.word=' ';
         
    }
    
    /** Inserts a word into the trie. */
    public void insert(String word) {
        char [] array = word.toCharArray();
        Trie node=this;
        for(int i=0;i<array.length;i++){
            if(node.tries[array[i]-'a']==null){
                node.tries[array[i]-'a']=new Trie();     
            }
            node=node.tries[array[i]-'a'];
            node.word=array[i];
            if(i==array.length-1){
                node.isWord=true;
            }    
        }
        
    }
    
    /** Returns if the word is in the trie. */
    public boolean search(String word) {
        char [] array =word.toCharArray();
        Trie node =this;
            for(int i=0;i<array.length;i++){
                if(node.tries[array[i]-'a']!=null){
                    node =node.tries[array[i]-'a'];
                    if(node.word ==array[i])
                        continue;
                    else
                        return false;
                }else
                    return false;
            
            }     
            return node.isWord==true?true:false;
     
        
        
        
    }
    
    /** Returns if there is any word in the trie that starts with the given prefix. */
    public boolean startsWith(String prefix) {
        char [] array =prefix.toCharArray();
        Trie node =this;
            for(int i=0;i<array.length;i++){
                if(node.tries[array[i]-'a']!=null){
                    node =node.tries[array[i]-'a'];
                    if(node.word ==array[i])
                        continue;
                    else
                        return false;
                }
                else
                    return false;

            }
            return true;
    }
}

/**
 * Your Trie object will be instantiated and called as such:
 * Trie obj = new Trie();
 * obj.insert(word);
 * boolean param_2 = obj.search(word);
 * boolean param_3 = obj.startsWith(prefix);
 */
```
JS：
```
/*
中文翻译：
实现一个 Trie (前缀树)，包含 insert, search, 和 startsWith 这三个操作。

示例:

Trie trie = new Trie();

trie.insert("apple");
trie.search("apple");   // 返回 true
trie.search("app");     // 返回 false
trie.startsWith("app"); // 返回 true
trie.insert("app");   
trie.search("app");     // 返回 true
说明:

你可以假设所有的输入都是由小写字母 a-z 构成的。
保证所有输入均为非空字符串。
链接：https://leetcode-cn.com/problems/implement-trie-prefix-tree

 * @lc app=leetcode id=208 lang=javascript
 *
 * [208] Implement Trie (Prefix Tree)
 *
 * https://leetcode.com/problems/implement-trie-prefix-tree/description/
 *
 * algorithms
 * Medium (36.93%)
 * Total Accepted:    172K
 * Total Submissions: 455.5K
 * Testcase Example:  '["Trie","insert","search","search","startsWith","insert","search"]\n[[],["apple"],["apple"],["app"],["app"],["app"],["app"]]'
 *
 * Implement a trie with insert, search, and startsWith methods.
 *
 * Example:
 *
 *
 * Trie trie = new Trie();
 *
 * trie.insert("apple");
 * trie.search("apple");   // returns true
 * trie.search("app");     // returns false
 * trie.startsWith("app"); // returns true
 * trie.insert("app");
 * trie.search("app");     // returns true
 *
 *
 * Note:
 *
 *
 * You may assume that all inputs are consist of lowercase letters a-z.
 * All inputs are guaranteed to be non-empty strings.
 *
 *
 */




function TrieNode(val) {
  this.val = val;
  this.children = [];
  this.isWord = false;
}

function computeIndex(c) {
  return c.charCodeAt(0) - "a".charCodeAt(0);
}
/**
 * Initialize your data structure here.
 */
var Trie = function() {
  this.root = new TrieNode(null);
};

/**
 * Inserts a word into the trie.
 * @param {string} word
 * @return {void}
 */
Trie.prototype.insert = function(word) {
  let ws = this.root;
  for (let i = 0; i < word.length; i++) {
    const c = word[i];
    const current = computeIndex(c);
    if (!ws.children[current]) {
      ws.children[current] = new TrieNode(c);
    }
    ws = ws.children[current];
  }
  ws.isWord = true;
};

/**
 * Returns if the word is in the trie.
 * @param {string} word
 * @return {boolean}
 */
Trie.prototype.search = function(word) {
  let ws = this.root;
  for (let i = 0; i < word.length; i++) {
    const c = word[i];
    const current = computeIndex(c);
    if (!ws.children[current]) return false;
    ws = ws.children[current];
  }
  return ws.isWord;
};

/**
 * Returns if there is any word in the trie that starts with the given prefix.
 * @param {string} prefix
 * @return {boolean}
 */
Trie.prototype.startsWith = function(prefix) {
  let ws = this.root;
  for (let i = 0; i < prefix.length; i++) {
    const c = prefix[i];
    const current = computeIndex(c);
    if (!ws.children[current]) return false;
    ws = ws.children[current];
  }
  return true;
};

/**
 * Your Trie object will be instantiated and called as such:
 * var obj = new Trie()
 * obj.insert(word)
 * var param_2 = obj.search(word)
 * var param_3 = obj.startsWith(prefix)
 */

```

### friend-circles
班上有 N 名学生。其中有些人是朋友，有些则不是。他们的友谊具有是传递性。如果已知 A 是 B 的朋友，B 是 C 的朋友，那么我们可以认为 A 也是 C 的朋友。所谓的朋友圈，是指所有朋友的集合。

给定一个 N * N 的矩阵 M，表示班级中学生之间的朋友关系。如果M[i][j] = 1，表示已知第 i 个和 j 个学生互为朋友关系，否则为不知道。你必须输出所有学生中的已知的朋友圈总数。

示例 1:

输入: 
[[1,1,0],
 [1,1,0],
 [0,0,1]]
输出: 2 
说明：已知学生0和学生1互为朋友，他们在一个朋友圈。
第2个学生自己在一个朋友圈。所以返回2。
示例 2:

输入: 
[[1,1,0],
 [1,1,1],
 [0,1,1]]
输出: 1
说明：已知学生0和学生1互为朋友，学生1和学生2互为朋友，所以学生0和学生2也是朋友，所以他们三个在一个朋友圈，返回1。
注意：

N 在[1,200]的范围内。
对于所有学生，有M[i][i] = 1。
如果有M[i][j] = 1，则有M[j][i] = 1。
链接：https://leetcode-cn.com/problems/friend-circles

```
   class Solution{
    private int[] parent;

    // 并查集
    public int findCircleNum(int[][] M) {
        int N = M.length;
        parent = new int[N];
        // 默认初始化-1自己为根
        Arrays.fill(parent, -1);

        for (int i = 0; i < N; i++) {
            for (int j = i + 1; j < N; j++) {
                if (M[i][j] == 1) {
                    union(i, j);
                }
            }
        }
        // 查找所有的根(值为-1)的个数即为集合的个数
        int cnt = 0;
        for (int id : parent) {
            if (id == -1)
                cnt++;
        }
        return cnt;
    }

    private void union(int x, int y) {
        int xp = findRoot(x);
        int yp = findRoot(y);
        if (xp != yp)  // 如果已经在同一个集合中则不必合并
            parent[yp] = xp;
    }

    private int findRoot(int id) {
        if (parent[id] == -1)
            return id;
        parent[id] = findRoot(parent[id]); // 路径压缩
        return parent[id];
    }
}
```
C++
```
class Solution {
public:
    int father[210];
    //查找祖先节点，当节点记录的祖先是自己，则表示查找到祖先了
    int findFather(int x)
    {
        while(x!=father[x])
        {
            x = father[x];
        }
        return x;
    }
    //合并节点：设置共同祖先
    void Union(int a,int b)
    {
        int fa = findFather(a);
        int fb = findFather(b);
        if(fa!=fb)
        {
            father[fa] = fb;
        }
    }
    //最开始的时候，每个节点时分散的，都是自己的祖先
    void init()
    {
        for(int i=0;i<210;i++)
        {
            father[i] = i;
        }
    }
    //主函数
    int findCircleNum(vector<vector<int>>& M) {
        init();
        //对N个学生两两做判断
        for(int i=0;i<M.size();i++)
        {
            for(int j=i+1;j<M.size();j++)
            {
                if(M[i][j]==1)
                {
                    Union(i,j);
                }
            }
        }
        //一次遍历找到所有祖先节点，即为朋友圈的个数
        int res = 0;
        for(int i=0;i<M.size();i++)
        {
            if(i==father[i])
            {
                res++;
            }
        }
        return res;
    }
};

```
#### 跳跃游戏
给定一个非负整数数组，你最初位于数组的第一个位置。

数组中的每个元素代表你在该位置可以跳跃的最大长度。

你的目标是使用最少的跳跃次数到达数组的最后一个位置。

示例:

输入: [2,3,1,1,4]
输出: 2
解释: 跳到最后一个位置的最小跳跃数是 2。
     从下标为 0 跳到下标为 1 的位置，跳 1 步，然后跳 3 步到达数组的最后一个位置。
说明:

假设你总是可以到达数组的最后一个位置。
链接：https://leetcode-cn.com/problems/jump-game-ii

Java代码：
```
顺序模拟：时间复杂度O(n),
class Solution {
    public int jump(int[] nums) {
          int end=0;
          int MaxPosition=0;
          int steps=0;
          for(int i=0;i<nums.length-1;i++)
          {
              MaxPosition=Math.max(MaxPosition,nums[i]+i);
              
              if(end==i)
              {
                  end=MaxPosition;
                  steps++;
              }                
                
          } 
        return steps;
    }
}


//https://leetcode-cn.com/problems/jump-game-ii/solution/20190717jump-game-iizui-chang-gui-de-yi-chong-si-l/
class Solution {
    //用这个函数去寻找我的下一个位置
    public static int search(int i,int j,int[] num)//i为我所在的位置，j为我所能走的距离。
    {
        int t  = 0;
        if(i + j <  num.length - 1)
        //到不了的话，就用t记录下能到达的最远位置，然后一个个去比较，看哪个合适。
        {
            t = i + j;
        }
        else//如果能够达到最后一个位置，或者再往后，那么想都不用想，直接返回最后一个位置。
        {
            return num.length - 1;
        }
        
        //从我所在位置的下一个位置开始走
        for(int k = i + 1; k <= i + j && k <= num.length - 1; k++)
        {
//一般我们更想走得更远，会选最远的那个位置，除非中间有一个位置，能够使我走得比最远还要远。
//这样的位置必须满足两个条件。1、使我走得比最远还远。2、在可能存在多个这样的点中，它是最强大的那一个。
            if(k + num[k] >= i + j && k + num[k]>=num[t] + t)
            {
                t = k;
            }
        }
        
        return t;//简而言之，我们每一次选择距离与潜在距离最远的那一个。
    }
    
    public static int jump(int[] nums) {

        int i = 0,j = -1,cut = 0;//与跳跃游戏I相比，新增的一个计数器cut.
        while(i<nums.length-1 && i!=j)//两个停止情况:走到最后一个位置了。遇到走不过去的“0”了。
        {
            j = i;
            i = search(i,nums[i],nums);//走到下一个位置
            cut++;
        }
        
        return cut;//与跳跃游戏I相比，此处返回的不是true or false.
    }
}





```

### 螺旋矩阵
给定一个包含 m x n 个元素的矩阵（m 行, n 列），请按照顺时针螺旋顺序，返回矩阵中的所有元素。

示例 1:

输入:
[
 [ 1, 2, 3 ],
 [ 4, 5, 6 ],
 [ 7, 8, 9 ]
]
输出: [1,2,3,6,9,8,7,4,5]
示例 2:

输入:
[
  [1, 2, 3, 4],
  [5, 6, 7, 8],
  [9,10,11,12]
]
输出: [1,2,3,4,8,12,11,10,9,5,6,7]
链接：https://leetcode-cn.com/problems/spiral-matrix
* 算法思路：
  最外层所有元素按照顺时针顺序输出，其次是次外层，类推。

  从左上方开始以顺时针的顺序遍历所有元素，假设当前层左上角坐标是(r1,c1),右下角坐标是(r2,c2)
  首先，遍历上方的所有元素(r1,c);
  然后，遍历右侧的所有元素(r,c2);
  然后，遍历下方的所有元素(r2,c);
  最后，遍历左侧的所有元素(r,c1);

  判断条件：```while(r1<r2&&c1<c2)```
  即：这一层有四条边，然后开始遍历

Java代码：
```
class Solution{
    public List<Integer> spiralOrder(int[][] matrix)
    {
        List ans=new ArrayList();
        //List内存消耗低于ArrayList
        if(matrix.length==0)
        return ans;
        int r1=0,r2=matrix.length-1;//行
        int c1=0,c2=matrix[0].length-1;//列
        while(r1<=r2&&c1<=c2)
        {
               for(int c=c1;c<=c2;c++) ans.add(matrix[r1][c]);
               //r1+1--->r2行
               for(int r=r1+1;r<=r2;r++) ans.add(matrix[r][c2]);
               if(r1<r2&&c1<c2){
                //c2-1--->c1
                for(int c=c2-1;c>c1;c--) ans.add(matrix[r2][c]);
                for(int r=r2;r>r1;r--) ans.add(matrix[r][c1]);
               }
               r1++;
               r2--;
               c1++;
               c2--;
        }
        return ans;

    }
}
```
代码疑问：ArrayList会装箱成Object来存储，拆箱会把object再转换回来，因此比较慢。
ArrayList与LinkedList比较：


### 螺旋矩阵二
给定一个正整数 n，生成一个包含 1 到 n2 所有元素，且元素按顺时针顺序螺旋排列的正方形矩阵。

示例:

输入: 3
输出:
[
 [ 1, 2, 3 ],
 [ 8, 9, 4 ],
 [ 7, 6, 5 ]
]

链接：https://leetcode-cn.com/problems/spiral-matrix-ii
* 算法思路：
   生成一个n * n的空矩阵mat,随后模拟整个向内环绕的填入过程。
   1. 定义当前左右上下边界，l,r,t,b,初始值num=1,迭代终止值tar=n * n
   2. 当num<=tar时，按l->r,t->b,r->l,b->t,的顺序填入顺序循环，每次填入后执行num+=1:得到下一个需要填入的数字.更新边界：例如从左到右填完后，上边界t+=1，相当于上边界向内缩1。
   3. 使用num<=tar作为迭代条件，而不选用l< r || t< b作为迭代条件的原因是为了解决当n为奇数时，矩阵中心数字无法在迭代过程中被填充的问题。
   4. 最终返回mat.
Java代码：
```
class Solution{
    public int[][] generateMatrix(int n)
    {
        in l=0,r=n-1,t=0,b=n-1;
        int[][] mat=new int[n][n];
        int num=1,tar=n*n;
        while(num<=tar)
        {
            for(int i=l;i<=r;i++) mat[t][i]=num++;
            t++;
            for(int i=t;i<=b;i++) mat[i][r]=num++;
            r--;
            for(int i=r;i>=l;i--) mat[b][i]=num++;
            b--;
            for(int i=b;i>=t;i--) mat[i][l]=num++;
            l++;
        }
        return mat;
    }
}
```
Python代码：
```
//代码执行出错
class Solution:
     def generateMatrix(self,n:int)->[[int]]:
         l,r,t,b=0,n-1,0,n-1
         mat=[[0 for _ in range(n)] for _ in range(n)]
         num,tar=1,n*n
         while num<=tar:
             for i in range(l,r+1):
                 mat[t][i]=num
                 num+=1
             t+=1
             for i in range(t,b+1):
                 mat[i][r]=num
                 num+=1
             r-=1
             for i in range(r,l-1,-1):
                 mat[b][i]=num
                 num+=1
             b-=1
             for i in range(b,t-1,-1):
                 mat[i][1]=num
                 num+=1
             l+=1
         return mat
```

### 爬楼梯
假设你正在爬楼梯。需要 n 阶你才能到达楼顶。

每次你可以爬 1 或 2 个台阶。你有多少种不同的方法可以爬到楼顶呢？

注意：给定 n 是一个正整数。

示例 1：

输入： 2
输出： 2
解释： 有两种方法可以爬到楼顶。
1.  1 阶 + 1 阶
2.  2 阶
示例 2：

输入： 3
输出： 3
解释： 有三种方法可以爬到楼顶。
1.  1 阶 + 1 阶 + 1 阶
2.  1 阶 + 2 阶
3.  2 阶 + 1 阶
链接：https://leetcode-cn.com/problems/climbing-stairs
* 算法思路：

   1. 暴力法climestairs(i,n)=climbstairs(i+1,n)+climbstairs(i+2,n)
     i 定义为当前阶数，n定义为目标阶数
   2. 记忆化递归：

Java代码：
```
//1.暴力法，时间复杂度达到O(2^n)
public class Solution{
    public int climbStairs(int n)
    {
        climb_Stairs(0,n);
    }
    public int climb_Stairs(int i,int n)
    {
        if(i>n)
        {
            return 0;
        }
        if(i==n)
        {
            return 1;
        }
        return climb_Stairs(i+1,n)+climb_Stairs(i+2,n);
    }
}



```
### 字母异位词分组
给定一个字符串数组，将字母异位词组合在一起。字母异位词指字母相同，但排列不同的字符串。

示例:

输入: ["eat", "tea", "tan", "ate", "nat", "bat"],
输出:
[
  ["ate","eat","tea"],
  ["nat","tan"],
  ["bat"]
]
说明：

所有输入均为小写字母。
不考虑答案输出的顺序。
链接：https://leetcode-cn.com/problems/group-anagrams


* 算法思路：
1. 排序数组分类
   当且仅当它们的排序字符串相等时，两个字符串是字母异位词

   维护一个ans：{string->List},其中每个键K是一个排序字符串，每个值是初始输入的字符串列表，排序后等于K。

   Java中，将键存储为字符串，

Java代码：
```
    class Solution{
        public List<List<String>> groupAnagrams(String[] strs)
        {
            if(strs.length==0) return new ArrayList();
            Map<String,List> ans=new HashMap<String,List>();
            for(String s: strs){
                char[] ca=s.toCharArray();
                Arrays.sort(ca);
                String key=String.valueOf(ca);
                if(!ans.containsKey(key)) ans.put(key,new ArrayList());
                ans.get(key).add(s);
            }
            return new ArrayList(ans.values());
        }
    }
```
时间复杂度：O(NKlogK) N为strs的长度，K为strs中字符串的最大长度

2. 按计数分类
   当且仅当它们的字符计数（每个字符的出现次数）相同时，两个字符串是字母异位词。

   将每个字符串s转换成字符数count,由26个非负整数组成，表示a，b，c的数量相等，使用这些计数作为哈希映射的基础。

   Java中，将字符数count的散列化表示是一个用 * * # * * 字符分隔的字符串。例如：abbccc将表示为#1#2#3#0#0#0...#0，其中共有26个条目。
  
  java代码：
```
class Solution {
    public List<List<String>> groupAnagrams(String[] strs){
        if(strs.length==0) return new ArrayList();
        Map<String,List> ans=new HashMap(String,List)();
        int[] count=new int[26];
        for(String s:strs){
            Arrays.fill(count,0);
            for(char c: s.toCharArray()) count[c-'a']++;
            StringBuilder sb=new StringBuilder("");
            for(int i=0;i<26;i++)
            {
                sb.append('#');
                sb.append(count[i]);
            }
            String key=sb.toString();
            if(!ans.containsKey(key)) ans.put(key,new ArrayList());
            ans.get(key).add(s);
        }
        return new ArrayList(ans.values());
    }
}


```
时间复杂度： O(NK)
3. 对于每个字符串，比较它们的每个字符出现的个数是否相等，相等就将它们放在一个List中，作为一个类别。最外层写一个for循环然后一一比较就可以，或者用一个等大的bool数组来记录当前字符串是否已经加入List，比较两个字符串的字符出现的次数用一个HashMap。
Java代码：
```

```

### Uva10391
复合词
```



```


###  二叉树的最大深度
给定一个二叉树，找出其最大深度。

二叉树的深度为根节点到最远叶子节点的最长路径上的节点数。

说明: 叶子节点是指没有子节点的节点。

示例：
给定二叉树 [3,9,20,null,null,15,7]，

    3
   / \
  9  20
    /  \
   15   7
返回它的最大深度 3 。
链接：https://leetcode-cn.com/problems/maximum-depth-of-binary-tree
算法思路：
1. DFS求解，递归
Java代码：
```
//**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */

class Solution {
    public int maxDepth(TreeNode root) {
        if(root==null)
        {
            return 0;
        }else
        {
            int left_height=maxDepth(root.left);
            int right_height=maxDepth(root.right);
            return Math.max(left_height,right_height)+1;
        }
    }
}

```
2. 迭代
  使用DFS策略访问每个结点，同时在每次访问时更新最大深度。
  从包含根节点且相应深度为1的栈开始，然后迭代，将当前节点弹出栈并推入子节点，每一步都会更新深度。
```
import javafx.util.Pair;
import java.lang.Math;
class Solution{
    public int maxDepth(TreeNode root)
    {
        Queue<Pair<TreeNode,Integer>> stack=new LinkedList<>();
        if(root !=null)
        {
            stack.add(new Pair(root,1));
        }
        int depth=0;
        while(!stack.isEmpty())
        {
            Pair<TreeNode,Integer> current=stack.poll();
            root =current.getKey();
            int current_depth=current.getValue();
            if(root!=null)
            {
                depth=Math.max(depth,current_depth);
                stack.add(new Pair(root.left,current_depth+1));
                stack.add(new Pair(root.right,current_depth+1));
            }
        }
        return depth;
    }
}
```
时间复杂度：O(N) 
空间复杂度：O(N)

### 今夕何夕hdu6112
Time Limit: 2000/1000 MS (Java/Others)    Memory Limit: 32768/32768 K (Java/Others)
Total Submission(s): 1819    Accepted Submission(s): 653
Problem Description
今天是2017年8月6日，农历闰六月十五。

小度独自凭栏，望着一轮圆月，发出了“今夕何夕，见此良人”的寂寞感慨。

为了排遣郁结，它决定思考一个数学问题：接下来最近的哪一年里的同一个日子，和今天的星期数一样？比如今天是8月6日，星期日。下一个也是星期日的8月6日发生在2023年。

小贴士：在公历中，能被4整除但不能被100整除，或能被400整除的年份即为闰年。
 

Input
第一行为T，表示输入数据组数。

每组数据包含一个日期，格式为YYYY-MM-DD。

1 ≤ T ≤ 10000

YYYY ≥ 2017

日期一定是个合法的日期

 

Output
对每组数据输出答案年份，题目保证答案不会超过四位数。
 

Sample Input

3 2017-08-06 2017-08-07 2018-01-01
 

Sample Output

2023 2023 2024
```

#include<cstdio>
#include<iostream>
#include<cmath>
using namespace std;
int month[13]={0,31,28,31,30,31,30,31,31,30,31,30,31};
int day,mon,year;
int nday,nyear,nmon;
int cnt;
int main()
{
    int t;
    scanf("%d",&t);
    while(t--)
    {
        scanf("%d-%d-%d",&year,&mon,&day);
           int cnt=0;
            nday=day;nyear=year;nmon=mon;
            while(1)
            {
                if(nday==day&&nmon==mon&&nyear>year&&cnt%7==0) break;
                if(nyear%4==0&&nyear%100!=0||nyear%400==0)
                {
                    month[2]=29;
                }
                else
                {
                    month[2]=28;
                } 
                nday++;
                cnt++;
                if(nday>month[nmon])
                {
                    nday%=month[nmon];  
                    nmon+=1;
                    if(nmon>12)
                    {
                        nyear+=1;
                        nmon%=12;
                    } 
                }
            }
            printf("%d\n",nyear);
        }   
} 

```
```
#include <iostream>
#include <stdio.h>
#include <string.h>
#include <vector>
#include <algorithm>
#include <math.h>
#include <queue>
#define LL long long
 
using namespace std;
 
int y,m,d;
 
bool pd(int x)
{
    if((x%4==0&&x%100!=0)||x%400==0) return true;
    return false;
}
 
int cal(int y,int m,int d)
{
    if(m<3)
    {
        m+=12;
        --y;
    }
    int W=(d+1+2*m+3*(m+1)/5+y+(y>>2)-y/100+y/400)%7;
    return W==0?7:W;
}
 
bool judge(int x)
{
    if(pd(y)&&!pd(x)&&(m==2&&d==29))
    {
        return true;
    }
    return false;
}
 
int main()
{
    int t;
    scanf("%d",&t);
    while(t--)
    {
        scanf("%d-%d-%d",&y,&m,&d);
        int pos=cal(y,m,d);
        int kk;
        for(int i=y+1;;++i)
        {
            kk=cal(i,m,d);
            if(judge(i)) continue;
            if(kk==pos)
            {
                printf("%d\n",i);
                break;
            }
        }
    }
    return 0;
}
```