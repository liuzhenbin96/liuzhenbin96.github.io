---
title: 【Leetcode系列】：23.合并K个排序链表
date: 2020-04-19 20:01:44
tag:
- Leetcode
categories:
- Leetcode
---
### 题目描述
[23.合并K个排序链表](https://leetcode-cn.com/problems/merge-k-sorted-lists/)

合并k个排序链表，返回合并后的排序链表。请分析和描述算法的复杂度。

```
示例：
输入:
[
  1->4->5,
  1->3->4,
  2->6
]
输出: 1->1->2->3->4->4->5->6
```

### 题目解析

#### 方法一：暴力法

**解题思路**

合并K个排序链表，首先我们直接采用暴力法去解决，将链表所有节点的val值放入一个List中，然后将这个List进行排序，根据排序后的List重新构建新链表。

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

class Solution {
    public ListNode mergeKLists(ListNode[] lists) {
        if (lists == null || lists.length == 0) return null;

        // 1. 将链表节点放入一个List中
        List<Integer> arr = new ArrayList<Integer>();
        for (int i = 0; i < lists.length; i++) {
            ListNode cur = lists[i];
            while(cur != null) {
                arr.add(cur.val);
                cur = cur.next;
            }
        }

        // 2. 排序
        Collections.sort(arr);
        
        // 3. 重新构建链表
        ListNode res = new ListNode(0);
        ListNode cur = res;
        for(int i = 0; i < arr.size(); i++) {
            ListNode node = new ListNode(arr.get(i));
            cur.next = node;
            cur = cur.next;
        }
        return res.next;
    }
}
```

**复杂度分析**

时间复杂度：O(N * log(N)), N代表所有链表节点数量。

- 遍历所有值需要花费O(N)时间
- 稳定的排序算法花费N * log(N)时间
- 重新构建链表需要花N * log(N)时间

空间复杂度：O(N)

#### 方法二：堆排序法

**解题思路**

采用堆的方式来进行链表节点的排序，创建一个大小为K的小根堆，首先将K个链表的头指针插入到堆中，然后取出堆顶元素，同时将堆顶元素的下一个节点插入到最小堆中，然后循环该操作直至链表节点全部遍历完成。

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

class Solution {
    public ListNode mergeKLists(ListNode[] lists) {
        if (lists == null || lists.length == 0) return null;

        // 1. 初始化小根堆
        PriorityQueue<ListNode> queue = new PriorityQueue(new Comparator<ListNode>() {
    			public int compare(ListNode o1, ListNode o2) {
    				return (o1.val - o2.val);
    			}
    		});
        for(int i = 0; i < lists.length; i++) {
            if (lists[i] != null) queue.add(lists[i]);
        }

        // 2. 取出堆顶元素，并将堆顶元素的下一节点插入小根堆
        ListNode res = new ListNode(0);
        ListNode cur = res;
        while(!queue.isEmpty()) {
            ListNode top = queue.poll();
            if (top.next != null) {
                queue.add(top.next);
            }
            cur.next = top;
            cur = cur.next;
        }
        return res.next;
    }
}
```

**复杂度分析**

时间复杂度：O(N * log(K))，N代表所有链表节点数量，K代表链表的个数。

空间复杂度：O(K)

#### 方法三：分治法

**解题思路**

我们将K个链表对半划分，先合并前K/2个链表，再合并后K/2个链表，然后将前K/2个链表合并成的链表再与后K/2个链表合并的链表进行合并，得到最终结果。

在处理前K/2个和后K/2个链表时，就跟上述方法思路一致，通过递归的方式不停的将链表进行划分，直到链表无法继续划分为止，将链表返回给递归的上一层进行两两合并。

***分治思路：划分子问题 --> 合并子问题结果【递归实现】***

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

class Solution {
    public ListNode mergeKLists(ListNode[] lists) {
        if (lists == null || lists.length == 0) return null;
        return divide(lists, 0, lists.length-1);
    }

    public ListNode divide(ListNode[] lists, int lo, int hi) {
        if (lo == hi) return lists[lo];
        int mid = lo + (hi - lo) / 2;
        ListNode left = divide(lists, lo, mid);
        ListNode right = divide(lists, mid + 1, hi);
        return merge(left, right);
    }

    public ListNode merge(ListNode l1, ListNode l2) {
		if(l1 == null || l2 == null) {
			return (l1 == null) ? l2 : l1;
		}
		if(l1.val <= l2.val) {
			l1.next = merge(l1.next,l2);
			return l1;
		} else {
			l2.next = merge(l1, l2.next);
			return l2;
		}
	}
}
```

**复杂度分析**

时间复杂度：O(N * log(K))，N代表所有链表节点数量，K代表链表的个数。

空间复杂度：O(K)