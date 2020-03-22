---
title: 【Leetcode系列】：四数之和
date: 2020-03-22 15:15:15
tag:
- Leetcode
categories:
- Leetcode
---
### 题目描述
[18. 四数之和](https://leetcode-cn.com/problems/4sum/)
> 给定一个包含 n 个整数的数组 nums 和一个目标值 target，判断 nums 中是否存在四个元素 a，b，c 和 d ，使得 a + b + c + d 的值与 target 相等？找出所有满足条件且不重复的四元组。
> 注意：答案中不可以包含重复的四元组。
> 示例：
> 给定数组 nums = [1, 0, -1, 0, -2, 2]，和 target = 0
> 满足要求的四元组集合为：[[-1,  0, 0, 1],[-2, -1, 1, 2],[-2,  0, 0, 2]]

### 题目解析

四数之和与上一题三数之和的解题思路是一样的，也能通过暴力遍历和哈希表的方法来进行求解，这两种方法在这里就不过多赘述，可以先从两数之和、三数之和两题开始了解具体的解题思路。

#### 双指针解法

**解题思路**

题目要求不包含重复的四元组，且双指针一般用于有序数组的情况。首先我们将数组进行排序，然后通过两层循环固定两个元素 ***nums[i]和nums[j]***，此时就把问题转化为求三数之和。接着通过双指针l和r遍历数组，加快找到 ***nums[i]+nums[j]+nums[l]+nums[r]=target**的四元组。

**代码示例**

Java:

```java
public List<List<Integer>> fourSum(int[] nums, int target) {
    List<List<Integer>> res = new ArrayList<>();
    if (nums == null || nums.length < 4) {
        return res;
    }
    Arrays.sort(nums);
    int len = nums.length;

    for (int i = 0; i < len - 3; i++) {
        if (i > 0 && nums[i] == nums[i - 1]) {
            continue;
        }
        int min = nums[i] + nums[i + 1] + nums[i + 2] + nums[i + 3];
        if (min > target) {
            break;s
        }
        int max = nums[i] + nums[len - 1] + nums[len - 2] + nums[len - 3];
        if (max < target) {
            continue;
        }

        for (int j = i + 1; j < len - 2; j++) {
            if (j > i + 1 && nums[j] == nums[j - 1]) {
                continue;
            }
            int min2 = nums[i] + nums[j] + nums[j + 1] + nums[j + 2];
            if(min2 > target){
                break;
            }
            int max2 = nums[i] + nums[j] + nums[len - 1] + nums[len - 2];
            if(max2 < target){
                continue;
            }

            int l = j + 1, r = len - 1;
            while (l < r) {
                int sum = nums[i] + nums[j] + nums[l] + nums[r];
                if (sum == target) {
                    res.add(Arrays.asList(nums[i], nums[j], nums[l] ,nums[r]));
                    while (l < r && nums[l] == nums[++l]);
                    while (l < r && nums[r] == nums[--r]);
                } else if (sum < target) {
                    l++;
                } else {
                    r--;
                }
            }
        }
    }
    return res;
}

```

**复杂度分析**

时间复杂度：O(n^3)

空间复杂度：O(1)