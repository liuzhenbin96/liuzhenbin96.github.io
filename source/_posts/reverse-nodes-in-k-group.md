---
title: 【Leetcode系列】：25.K个一组翻转链表
date: 2020-04-06 16:39:28
tag:
- Leetcode
categories:
- Leetcode
---
### 题目描述
[25.K个一组翻转链表](https://leetcode-cn.com/problems/reverse-nodes-in-k-group/)

给你一个链表，每 k 个节点一组进行翻转，请你返回翻转后的链表。

k是一个正整数，它的值小于或等于链表的长度。

如果节点总数不是k的整数倍，那么请将最后剩余的节点保持原有顺序。

说明：你的算法只能使用常数的额外空间。
你不能只是单纯的改变节点内部的值，而是需要实际进行节点交换。

```
示例：
给你这个链表：1->2->3->4->5
当k= 2 时，应当返回: 2->1->4->3->5
当k= 3 时，应当返回: 3->2->1->4->5
```

### 题目解析

#### 方法一：迭代法

**解题思路**

这个在上一题两两交换链表节点的基础增加了难度，需要K个一组进行翻转。首先我们可以将链表分为 ***“已翻转”、“待翻转”、“未翻转”*** 三部分。在每次进行翻转前，通过K值确定翻转链表的范围。

在上面一题已经提到进行链表翻转时需要注意链表当前遍历节点、其前驱节点和后继节点，需要额外的指针进行标识，防止链表翻转后无法继续向后遍历。在这里我们 ***prev*** 指针代表待翻转链表的前驱节点， ***end*** 指针代表待翻转链表的末尾， ***next*** 指针代表待翻转链表的后继节点。

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

public ListNode reverseKGroup(ListNode head, int k) {
    if (head == null || head.next == null || k == 0) {
        return head;
    }
    
    // 哨兵节点
    ListNode dummy = new ListNode(0);
    dummy.next = head;
    
    // 待翻转链表的前驱节点
    ListNode prev = dummy;
    // 待翻转链表的结束位置
    ListNode end = dummy;
    
    while(end.next != null) {
        for (int i = 0; i < k && end != null; i++) {
            end = end.next;
        }
        if (end == null) break;

        // 待翻转链表的起始位置
        ListNode start = prev.next;
        // 待翻转链表的后继节点
        ListNode next = end.next;

        // 将待翻转链表的next指针置为null，然后翻转链表
        end.next = null;
        prev.next = reverse(start);
        start.next = next;
        
        // 前驱指针和结束指针移动
        prev = start;
        end = prev;
    }
    return dummy.next;
}

// 链表反转
private ListNode reverse(ListNode head) {
    ListNode pre = null;
    ListNode curr = head;
    while (curr != null) {
        ListNode next = curr.next;
        curr.next = pre;
        pre = curr;
        curr = next;
    }
    return pre;
}
```

**复杂度分析**

时间复杂度：O(n*k)

空间复杂度：O(1)

#### 方法二：递归法

**解题思路**

递归的思路主要在于将子问题传递到下一层递归函数处理，递归函数返回的即正确结果，当前层只需要处理挡墙层逻辑即可。

当前层只需要处理K个链表元素的翻转，并调用递归函数处理后面未翻转的链表，递归函数处理完未翻转链表后返回结果，当前层拿到结果后与自身翻转结束后的链表进行拼接，最后得到完整的翻转结果。

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

public ListNode reverseKGroup(ListNode head, int k) {
    // terminator
    if (head == null || head.next == null || k == 0) {
        return head;
    }
    
    ListNode start = head , end = head;
    for (int i = 0; i < k; i++) {
        if (end == null) return head;
        end = end.next;
    }
    
    // process data: reverse linked list
    ListNode newHead = reverse(start, end);
    
    // recursion
    start.next = reverseKGroup(end, k);
    return newHead;
}

private ListNode reverse(ListNode start, ListNode end) {
    ListNode pre = null, curr = start, next = start;
    while (curr != end) {
        next = curr.next;
        curr.next = pre;
        pre = curr;
        curr = next;
    }
    return pre;
}
```

**复杂度分析**

时间复杂度：O(n*k)

空间复杂度：O(n)