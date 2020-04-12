---
title: 【Leetcode系列】：88.合并两个有序数组
date: 2020-04-12 17:17:40
tag:
- Leetcode
categories:
- Leetcode
---
### 题目描述
[88.合并两个有序数组](https://leetcode-cn.com/problems/merge-sorted-array/)

给你两个有序整数数组 nums1 和 nums2，请你将 nums2 合并到 nums1 中，使 nums1 成为一个有序数组。

说明:
初始化 nums1 和 nums2 的元素数量分别为 m 和 n 。
你可以假设 nums1 有足够的空间（空间大小大于或等于 m + n）来保存 nums2 中的元素。

```
示例：
输入:
nums1 = [1,2,3,0,0,0], m = 3
nums2 = [2,5,6],       n = 3
输出:[1,2,2,3,5,6]
```

### 题目解析

#### 方法一：暴力法

**解题思路**

暴力解法就是直接把两个数组合并到一个数组中，完成合并后再进行排序。这种方式没有利用数组有序的特性，所以时间复杂度较差，时间复杂度为O((m+n)log(m+n))。

**代码示例**

Java：
```java
public void merge(int[] nums1, int m, int[] nums2, int n) {
    // 1. 合并数组
    System.arraycopy(nums2, 0, nums1, m, n);
    // 2. 数组排序
    Arrays.sort(nums1);
}
```

**复杂度分析**

时间复杂度：O((m+n)log(m+n))

空间复杂度：O(1)

#### 方法二：双指针(从前往后)

**解题思路**

两个数组都是有序数组，我们可以采用双指针的方式分别遍历 ***nums1*** 和 ***nums2*** 这两个数组。使用指针 ***p1*** 作为数组 ***nums1*** 的开头，指针 ***p2*** 作为数组 ***nums2*** 的开头，每次将较小值放入输出数组中去。由于 ***nums1*** 是用于输出的数组，所有先要把 ***nums1***中的元素拷贝一份出来，此时需要O(m)的空间复杂度。

**代码示例**

Java：
```java
public void merge(int[] nums1, int m, int[] nums2, int n) {
    // 1. 拷贝nums1数组
    int[] temp = new int[m];
    System.arraycopy(nums1, 0, temp, 0, m);
    
    // 2. 双指针从前往后遍历
    int resIdx = 0, p1 = 0, p2 = 0;
    while(p1 < m && p2 < n) {
        nums1[resIdx++] = temp[p1] < nums2[p2] ? temp[p1++] : nums2[p2++];
    }
    
    // 3. 拷贝未比较元素
    if (p1 < m) {
        System.arraycopy(temp, p1, nums1, p1 + p2, m + n - p1 - p2);
    }
    
    if (p2 < n) {
        System.arraycopy(nums2, p2, nums1, p1 + p2, m + n - p1 - p2);
    }
}
```

**复杂度分析**

时间复杂度：O(m+n)

空间复杂度：O(m)

#### 方法二：双指针(从后往前)

**解题思路**

题目中提到nums1中有足够大的空间存储 ***nums1*** 和 ***nums2*** 中的元素，所以 ***nums1*** 数组下标 ***m-1*** 后面都是空闲空间。我们可以使用双指针从后往前的将 ***nums1*** 和 ***nums2*** 中的较大值放入 ***nums1*** 数组中。

**代码示例**

Java：
```java
public void merge(int[] nums1, int m, int[] nums2, int n) {
    // 1. 双指针从后往前遍历
    int resIdx = m + n - 1, p1 = m - 1, p2 = n - 1;
    while(i >= 0 && j >= 0) {
        nums1[resIdx--] = nums1[p1] > nums2[p2] ? nums1[p1--] : nums2[p2--];
    }
    
    // 2. 拷贝未比较元素
    while(p2 >= 0) {
        nums1[resIdx--] = nums2[p2--];
    }
}
```

**复杂度分析**

时间复杂度：O(m+n)

空间复杂度：O(1)