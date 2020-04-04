---
title: 【Leetcode系列】：141.循环链表
date: 2020-04-04 19:50:57
tag:
- Leetcode
categories:
- Leetcode
---
### 题目描述
[141.环形链表](https://leetcode-cn.com/problems/linked-list-cycle/)
> 给定一个链表，判断链表中是否有环。  
> 为了表示给定链表中的环，我们使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 pos 是 -1，则在该链表中没有环。

```
示例 1：
输入：head = [3,2,0,-4], pos = 1  
输出：true
解释：链表中有一个环，其尾部连接到第二个节点。

示例 2：
输入：head = [1,2], pos = 0
输出：true
解释：链表中有一个环，其尾部连接到第一个节点。
```

### 题目解析

#### 方法一：哈希表

**解题思路**

首先可以想到的方法就是遍历链表并将遍历的链表Node节点存入哈希表中，通过哈希表是判断Node是否已经存在，若已经存在则说明链表存在环，否则无环。

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

public boolean hasCycle(ListNode head) {
    Set<ListNode> visited = new HashSet<ListNode>();
    ListNode curr = head;
    while (curr != null) {
        if (visited.contains(curr)) {
            return true;
        } 
        visited.add(curr);
        curr = curr.next;
    }
    return false;
}
```

**复杂度分析**

时间复杂度：O(n)

空间复杂度：O(n)，通过额外空间存储遍历节点

#### 方法二：双指针

**解题思路**

假设链表存在环，若两个遍历速率不一样的指针同时遍历整个链表，这两个指针最终会相遇。所以我们可以采用 ***快慢指针*** 的方式将空间复杂度优化到O(1)。慢指针slow每次遍历一个节点，快指针fast每次遍历两个节点，直到指针走到链表末尾或者两个指针相遇。

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

public boolean hasCycle(ListNode head) {
    ListNode slow = head, fast = head;
    while(fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
        if (slow == fast) {
            return true;
        }
    }
    return false;
}
```

**复杂度分析**

时间复杂度：O(n)

空间复杂度：O(1)
