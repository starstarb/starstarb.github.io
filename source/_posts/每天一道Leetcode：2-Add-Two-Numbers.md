---
title: 每天一道Leetcode：2. Add Two Numbers
copyright: true
tags: leetcode
categories: leetcode
abbrlink: 77f7a080
date: 2019-09-23 23:16:29
---
Add Two Numbers
<!--more-->
You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

**Example:**

Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)
Output: 7 -> 0 -> 8
Explanation: 342 + 465 = 807.

****Thought Process****


* 标签：链表
* 将两个链表看成是相同长度的进行遍历，如果一个链表较短则在前面补 000，比如 987 + 23 = 987 + 023 = 1010
* 每一位计算的同时需要考虑上一位的进位问题，而当前位计算结束后同样需要更新进位值
* 如果两个链表全部遍历完毕后，进位值为 111，则在新链表最前方添加节点 111
* 小技巧：对于链表问题，返回结果为头结点时，通常需要先初始化一个预先指针 pre，该指针的下一个节点指向真正的头结点head。使用预先指针的目的在于链表初始化时无可用节点值，而且链表构造过程需要指针移动，进而会导致头指针丢失，无法返回结果。


**为什么返回pre.next?**
因为cur会移动所以留了个pre，这是个常用的技巧，这样才能找到头的位置，所以pre就没动过，就是为了最后返回结果使用，想想看要是没有pre的话，这个链表就无法使用了，因为头找不到了

要把pre和cur理解成指针，也就是他们实际上存的是地址。

假设有一个链表 1->2，其中值为1的地方地址为0x01，值为2的地方地址为0x30

这时候在1前面加个0，也就是0->1->2，值为0的地方地址为0x14

让pre指向0，本质pre存的值时0x14

再让cur = pre，这时候cur也存的是0x14

cur移动，cur = cur.next，这时候cur存的是0x01，也就是值为1的地址，而pre一直都没动，所以它会一直指向0

Java Code
```
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
             ListNode pre=new ListNode(0);
             ListNode cur=pre;
             int carry=0;
             while(l1!=null||l2!=null){
                    int x=l1==null?0:l1.val;
                    int y=l2==null?0:l2.val;
                    int sum=x+y+carry;
                    
                    carry=sum/10;
                    sum=sum%10;
                    cur.next=new ListNode(sum);
                    
                    cur=cur.next;
                    if(l1!=null)
                      l1=l1.next;
                    if(l2!=null)
                      l2=l2.next;                               
                    }
                    if(carry==1){
                       cur.next=new ListNode(carry);
                    }
                    return pre.next;
    }
}

```