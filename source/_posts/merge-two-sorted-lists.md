---
title: 【Leetcode系列】：21.合并两个有序链表
date: 2020-04-12 17:18:09
tag:
- Leetcode
categories:
- Leetcode
---
### 题目描述
[21.合并两个有序链表](https://leetcode-cn.com/problems/merge-two-sorted-lists/)

将两个升序链表合并为一个新的升序链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。

```
示例：
输入：1->2->4, 1->3->4
输出：1->1->2->3->4->4
```

### 题目解析

#### 方法一：递归

**解题思路**

我们直接采用递归的方法，首先判断其中一个链表是否为空，若为空则无需进行递归比较直接返回结果。否则比较链表 ***l1*** 和链表 ***l2*** 的头元素哪个更小，将较小元素作为头节点，然后递归比较较小元素后的链表与另一链表，直至两链表元素都经过比较。


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

public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
    // 1. terminator 递归终止条件
    if (l1 == null) return l2;
    if (l2 == null) return l1;
    
    // 2. process data  处理当前层逻辑
    if (l1.val < l2.val) {
        // 3. drill down 递归到下一层
        l1.next = mergeTwoLists(l1.next, l2);
        return l1;
    } else {
        l2.next = mergeTwoLists(l2.next, l1);
        return l2;
    }
}
```

**复杂度分析**

时间复杂度：O(m+n)

空间复杂度：O(m+n)

#### 方法二：迭代

**解题思路**

我们采用迭代的方式来解决该问题，首先我们需要新建一个哨兵元素 ***newHead*** 用来当做合并后的链表头节点。然后遍历比较链表 ***l1*** 和链表 ***l2*** 元素，将元素较小者赋值到合并后的链表上，直至完成两链表元素之间的比较。

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

public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
    // 1. 新建哨兵节点
    ListNode newHead = new ListNode(-1);
    
    // 2. 迭代比较两链表元素
    ListNode curr = newHead;
    while(l1 != null && l2 != null) {
        if (l1.val < l2.val) {
            curr.next = l1;
            l1 = l1.next;
        } else {
            curr.next = l2;
            l2 = l2.next;
        }
        curr = curr.next;
    }
    
    // 3. 处理未比较元素
    curr.next = l1 == null ? l2 : l1;
    return newHead.next;
}
```

**复杂度分析**

时间复杂度：O(m+n)

空间复杂度：O(1)