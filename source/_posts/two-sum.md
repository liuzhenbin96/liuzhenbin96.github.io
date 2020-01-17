---
title: 【Leetcode系列】：两数之和
tag:
- Leetcode
categories:
- Leetcode
date: 2020-01-16 17:30:00
---
### 题目描述
[1. 两数之和](https://leetcode-cn.com/problems/two-sum/)
> 给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那 两个 整数，并返回他们的数组下标。  
> 你可以假设每种输入只会对应一个答案。但是，你不能重复利用这个数组中同样的元素。  
> 示例:  
> 给定 nums = [2, 7, 11, 15],   target = 9  
> 因为 nums[0] + nums[1] = 2 + 7 = 9  
> 所以返回 [0, 1]

### 题目解析

#### 方法一：暴力法

**解题思路**

首先我们可以通过两层循环的方式来求解，外层循环用于固定元素***nums[i]*** ，内存循环从下标***j=i+1***开始遍历至数组结尾，比较两数之和***nums[i] + nums[j]***与target值是否相等，若相等则直接返回结果***[i,j]***。  

**代码示例**

Java:

``` java
public int[] twoSum(int[] nums, int target) {
  if (nums == null || nums.length == 0) {
    return new int[]{};
  }
  for (int i = 0; i < nums.length; i++) {
    for (int j = i + 1; i < nums.length; j++) {
      if (nums[i] + nums[j] == target) {
        return new int[]{i,j};
      }
    }
  }
  return new int[]{};
}
```

Python3:

```python
def twoSum(self, nums: List[int], target: int) -> List[int]:
  nums_len = len(nums)
  for i in range(nums_len):
    for j in range(i + 1, nums_len):
      if nums[i] == target - nums[j]:
        return [i, j]
```

Go:

```go
func twoSum(nums []int, target int) []int {
  for i := 0; i < len(nums); i++ {
    for j := i + 1; j < len(nums); j++ {
      if nums[i] + nums[j] == target {
        return []int{i,j}
      }
    }
  }
  return []int{}
}
```
**复杂度分析**

时间复杂度：O(n^2)

空间复杂度：O(1)

#### 方法二：两遍哈希表遍历

**解题思路**

暴力法解决该问题采用两层循环方式，时间复杂度较高为O(n^2)，我们可采用***空间换时间***的方式降低算法的时间复杂度。哈希表的查询时间复杂度为O(1)，我们可以将数组元素作为key，元素下标作为value存入哈希表中。遍历数组元素***nums[i]***，计算差值***target - nums[i]***，判断差值是否在哈希表中且下标值不为i，若存在则将元素下标返回。

**代码示例**

Java:

```java
public int[] twoSum(int[] nums, int target) {
  Map<Integer, Integer> map = new HashMap<>();
  for (int i = 0; i < nums.length; i++) {
    map.put(nums[i], i);
  }
  for (int i = 0; i < nums.length; i++) {
    int diff = target - nums[i];
    if (map.containsKey(diff) && map.get(diff) != i) {
      return new int[]{i, map.get(diff)};
    }
  }
  return new int[]{};
}
```

Python3:

```python
def twoSum(self, nums: List[int], target: int) -> List[int]:
  nums_map = {}
  for key, value in enumerate(nums):
    nums_map[value] = key
  for key, value in enumerate(nums):
    left = target - value
    if nums_map.get(left, False) and key != nums_map[left]:
      return [key, nums_map[left]]
```

Go:

```go
func twoSum(nums []int, target int) []int {
  numsMap := make(map[int]int)
  for i, num := range nums {
    numsMap[num] = i
  }
  for i, num := range nums {
    if j, exist := numsMap[target - num]; exist && i != j {
      return []int{i, j}
    }
  }
  return []int{}
}
```
**复杂度分析**

时间复杂度：O(n)

空间复杂度：O(n)

#### 方法三：一遍哈希表遍历

**解题思路**

题目中提到***不能重复利用这个数组中同样的元素***，我们可以考虑只采用一次遍历的方式求解。在遍历数组元素***nums[i]***的过程中我们可以判断差值***target-nums[i]***是否存在于哈希表中，若不存在则将***nums[i]***的值作为key，下标作为value存入哈希表中，否则直接返回结果。

**代码示例**

Java:

```java
public int[] twoSum(int[] nums, int target) {
  Map<Integer, Integer> map = new HashMap<>();
  for (int i = 0; i < nums.length; i++) {
    int diff = target - nums[i];
    if (map.containsKey(diff)) {
      return new int[]{map.get(diff), i};
    }
    map.put(nums[i], i);
  }
  return new int[]{};
}
```

Python3:

```python
def twoSum(self, nums: List[int], target: int) -> List[int]:
  nums_map = {}
  for key, value in enumerate(nums):
    left = target - value
    if left in nums_map:
      return [nums_map[left], key]
    nums_map[value] = key
```

Go:

```go
func twoSum(nums []int, target int) []int {
  numsMap := make(map[int]int)
  for i, num := range nums {
    if j, exist := numsMap[target - num]; exist {
      return []int{j, i}
    }
    numsMap[num] = i
  }
  return []int{}
}
```
**复杂度分析**

时间复杂度：O(n)

空间复杂度：O(n)
