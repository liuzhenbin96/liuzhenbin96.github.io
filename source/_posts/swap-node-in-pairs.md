---
title: 【Leetcode系列】：24.两两交换链表中的节点
date: 2020-04-05 20:59:34
tag:
- Leetcode
categories:
- Leetcode
---
### 题目描述
[24.两两交换链表中的节点](https://leetcode-cn.com/problems/swap-nodes-in-pairs/)

给定一个链表，两两交换其中相邻的节点，并返回交换后的链表。

你不能只是单纯的改变节点内部的值，而是需要实际的进行节点交换。

```
示例：
给定 1->2->3->4, 你应该返回 2->1->4->3.
```

### 题目解析

#### 方法一：递归

**解题思路**

递归的解题思路在于把子问题交给下一层递归函数处理，而本身只聚焦于本层次的问题，当你得到递归函数返回值时，默认为已经得到正确结果，只需要继续处理当前层逻辑即可。

本题需要两两交换链表节点，所以头节点 ***head*** 的下一个节点 ***next*** 必然就是新的头节点 ***newHead*** ，然后通过递归的方式交换 ***next*** 节点后面的链表，得到后续链表交换节点后直接赋值给头节点 ***head*** 的next指针，处理 ***newHead*** 和 ***head*** 的关联，得到交换后的链表。

**代码示例**

Java：
```java
/**
 * Definition for singly-linked list.
 */
public class ListNode {
    int val;
    ListNode next;
    ListNode(int x) {
        val = x; 
    }
}

public ListNode swapPairs(ListNode head) {
    // terminator
    if (head == null || head.next == null) {
        return head;
    }

    // recursion
    ListNode newHead = head.next;
    head.next = swapPairs(newHead.next);

    // process data
    newHead.next = head;
    return newHead;
}
```

**复杂度分析**

时间复杂度：O(n)

空间复杂度：O(n)，在递归时使用额外栈空间。

#### 方法二：迭代

**解题思路**

迭代的方式大家都比较熟悉，从链表头节点开始遍历然后两两交换节点。在交换节点的过程中，需要注意就是链表遍历到什么位置了，两两节点交换完成后是否还能继续向后遍历。

迭代解法中通过 ***prev*** 和 ***curr*** 指针用来记录链表当前遍历位置，并且在两两节点交换完成后 ***prev*** 和 ***curr*** 指针向后移动继续进行节点交换，直到链表末尾。

**代码示例**

Java：
```java
/**
 * Definition for singly-linked list.
 */
public class ListNode {
    int val;
    ListNode next;
    ListNode(int x) {
        val = x; 
    }
}

public ListNode swapPairs(ListNode head) {
    ListNode newHead = new ListNode(0);
    newHead.next = head;
    ListNode prev = newHead, curr = head;
    while(curr != null && curr.next != null) {
        // 记录当前节点的next节点
        ListNode next = curr.next;
        
        // 交换节点
        curr.next = curr.next.next;
        next.next = curr;
        prev.next = next;
        
        // prev，curr指针后移
        prev = curr;
        curr = curr.next;
    }
    return newHead.next;
}
```

**复杂度分析**

时间复杂度：O(n)

空间复杂度：O(1)
