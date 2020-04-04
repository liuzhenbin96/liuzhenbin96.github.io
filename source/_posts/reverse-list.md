---
title: 【Leetcode系列】：206.反转链表
date: 2020-03-29 14:43:37
tag:
- Leetcode
categories:
- Leetcode
---

### 题目描述

[206.反转链表](https://leetcode-cn.com/problems/reverse-linked-list/)

> 反转一个单链表。
>
> **示例:**
>
> ```
> 输入: 1->2->3->4->5->NULL
> 输出: 5->4->3->2->1->NULL
> ```

### 题目解析

#### 方法一：迭代反转

**解题思路**

从题意分析，反转链表对于链表上每个节点来说，就是将当前节点 ***curr*** 的 ***next*** 指针指向其前继节点 ***prev*** 直到链表末尾，所以遍历链表时需要一个指针来标识当前节点 ***curr*** 的前继节点 ***prev*** ，也需要一个指针来标识当前节点 ***curr*** 的后继节点 ***next*** ，防止当前节点反转后无法继续遍历。

**代码示例**

Java：

```java
/*
* Definition for singly-linked list.
*/
public class ListNode {
  int val;
  ListNode next;
  ListNode(int x) { val = x; }
}

public ListNode reverseList(ListNode head) {
  ListNode prev = null, curr = head, next = head;
  while(curr != null) {
    next = curr.next;
    curr.next = prev;
    prev = curr;
    curr = next;
  }
  return prev;
}
```

**复杂度分析**

时间复杂度：O(n)，遍历整个单链表

空间复杂度：O(1)，没有额外开辟空间

#### 方法二：递归反转

**解题思路**

递归解法的思路就是假设头节点 ***head*** 后面的链表都已经完成反转（通过递归完成），现在要处理的就是当前节点 ***head*** 和它的下一个节点 ***next*** 的反转 ***head.next.next = head*** 。

**代码示例**

Java

```java
/*
* Definition for singly-linked list.
*/
public class ListNode {
  int val;
  ListNode next;
  ListNode(int x) { val = x; }
}

public ListNode reverseList(ListNode head) {
  // termination condition
  if (head == null || head.next == null) {
    return head;
  }
  // recursion
  ListNode newHead = reverseList(head.next);
  // process current data
  head.next.next = head;
  head.next = null;
  return newHead;
}
```

**复杂度分析**

时间复杂度：O(n)，遍历整个单链表

空间复杂度：O(n)