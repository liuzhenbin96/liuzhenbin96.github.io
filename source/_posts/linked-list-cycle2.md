---
title: 【Leetcode系列】：142.循环链表II
date: 2020-04-04 19:55:23
tag:
- Leetcode
categories:
- Leetcode
---
### 题目描述
[142.环形链表 II](https://leetcode-cn.com/problems/linked-list-cycle-ii/)
> 给定一个链表，返回链表开始入环的第一个节点。如果链表无环，则返回 null。  
> 为了表示给定链表中的环，我们使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 pos 是 -1，则在该链表中没有环。  
> 说明：不允许修改给定的链表。

```
示例 1：
输入：head = [3,2,0,-4], pos = 1
输出：tail connects to node index 1
解释：链表中有一个环，其尾部连接到第二个节点。

示例 2：
输入：head = [1,2], pos = 0
输出：tail connects to node index 0
解释：链表中有一个环，其尾部连接到第一个节点。
```

### 题目解析

#### 方法一：哈希表

**解题思路**

首先可以想到的方法就是遍历链表并将遍历的链表Node节点存入哈希表中，若链表有环则返回第一个出现的重复的Node节点。

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

public ListNode detectCycle(ListNode head) {
    Set<ListNode> visited = new HashSet<ListNode>();
    ListNode curr = head;
    while (curr != null) {
        if (visited.contains(curr)) {
            return curr;
        }
        visited.add(curr);
        curr = curr.next;
    }

    return null;
}
```

**复杂度分析**

时间复杂度：O(n)

空间复杂度：O(n)，通过额外空间存储遍历节点

#### 方法二：双指针

**解题思路**

在上一题环形链表中已经提过若链表有环的情况，快慢指针会在某个节点相遇。

此时链表头节点到环起始点的距离为x，环起始点到快慢指针相遇节点的距离为y，然后相遇节点再继续走z的距离回到环起始点。

由于快指针的速度时慢指针的两倍，所以快指针追上慢指针必然比慢指针多走一圈。此时慢指针走的路程s = x + y，快指针走的路程f = x + y + z + y，且 f = 2s，所有可以得到 x = z，也就是说头节点到环起始点的距离等于指针相遇节点到环起始点距离。所以当快慢指针相遇时，指定其中一个指针回到头节点，并保持相等的速度继续遍历直到两个指针再次相遇则为链表环的起始点了。

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

public ListNode detectCycle(ListNode head) {
    ListNode slow = head, fast = head;
    while (true) {
        if (fast == null || fast.next == null) {
            return null;
        }
        slow = slow.next;
        fast = fast.next.next;
        if (slow == fast) break;
    }
    fast = head;
    while (slow != fast) {
        slow = slow.next;
        fast = fast.next;
    }
    return fast;
}
```

**复杂度分析**

时间复杂度：O(n)

空间复杂度：O(1)
