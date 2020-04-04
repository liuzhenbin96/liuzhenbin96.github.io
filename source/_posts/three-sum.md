---
title: 【Leetcode系列】：15.三数之和
date: 2020-03-22 13:43:25
tag:
- Leetcode
categories:
- Leetcode
---
### 题目描述
[15. 三数之和](https://leetcode-cn.com/problems/3sum/)
> 给你一个包含 n 个整数的数组 nums，判断 nums 中是否存在三个元素 a，b，c ，使得 a + b + c = 0 ？请你找出所有满足条件且不重复的三元组。
> 注意：答案中不可以包含重复的三元组。
> 示例:  
> 给定数组 nums = [-1, 0, 1, 2, -1, -4]，
> 满足要求的三元组集合为：
> [[-1, 0, 1],[-1, -1, 2]]

### 题目解析

1. 要求a + b + c = 0 ==> a + b = -c
2. 数组无序且存在重复元素，但是结果中不可以包含重复的三元组
3. 可能没有满足条件的情况，返回空数组

#### 方法一：暴力法

**解题思路**

采用三层循环的方式，最外层循环固定一个元素 ***nums[i]*** ，第二层循环从元素 ***nums[i]下一个元素 j = i + 1，nums[j]*** 开始遍历，第三层循环则从元素 ***nums[j]下一个元素 k = j + 1，nums[k]*** 开始遍历，然后判断三个元素相加是否满足以下条件：***nums[i] + nums[j] + nums[k] == 0***

**代码示例**

Java:

```java
public List<List<Integer>> threeSum(int[] nums) {
    if (nums == null || nums.length <= 2) {
        return new ArrayList<List<Integer>>();
    }
    Arrays.sort(nums);
    Set<List<Integer>> result = new LinkedHashSet<>();
    for (int i = 0; i < nums.length; i++) {
        for (int j = i + 1; j < nums.length; j++) {
            for (int k = j + 1; k < nums.length; k++) {
                if (nums[i] + nums[j] + nums[k] == 0) {
                    List<Integer> value = Arrays.asList(nums[i], nums[j], nums[k]);
                    result.add(value);
                }
            }
        }
    }
    return new ArrayList<>(result);
}
```

**复杂度分析**

时间复杂度：O(n^3)

空间复杂度：O(1)

#### 方法二：哈希表

**解题思路**

题目要求为a + b + c = 0可以推出 ***a + b = -c*** ，将三数之和转化为两数求和的问题。外层循环固定某个元素 ***nums[i]*** ，然后再剩下的元素中找到两个元素相加等于-nums[i]的情况。

**代码示例**

Java:

```java
public List<List<Integer>> threeSum(int[] nums) {
    if (nums == null || nums.length <= 2) {
        return new ArrayList<List<Integer>>();
    }

    Set<List<Integer>> result = new LinkedHashSet<>();
    for (int i = 0; i < nums.length - 2; i++) {
        int target = -nums[i];
        Map<Integer, Integer> hashMap = new HashMap<>(nums.length - i);
        for (int j = i + 1; j < nums.length; j++) {
            int v = target - nums[j];
            Integer exist = hashMap.get(v);
            if (exist != null) {
                List<Integer> list = Arrays.asList(nums[i], exist, nums[j]);
                list.sort(Comparator.naturalOrder());
                result.add(list);
            } else {
                hashMap.put(nums[j], nums[j]);
            }
        }
    }
    return new ArrayList<>(result);
}    
```

**复杂度分析**

时间复杂度：O(n^2)

空间复杂度：O(n)

#### 方法三：双指针法

**解题思路**

有方法二中可以得知 ***a + b = -c*** ，在外层循环中固定元素 ***nums[i]*** ，然后再剩余元素中找到两元素相加和为-nums[i]的情况。此时若将数组元素有序，则可通过双指针的方式进行元素遍历，加快元素查找。  
数组有序，若nums[i]>0，则后续数组元素必大于零，不可能满足条件；否则使用双指针l=i+1和r=len-1遍历数组剩余元素；  
若 ***nums[l]+nums[r]+nums[i] > 0*** 则高位指针r向中间移动，否则低位指针l向中间移动；  
若 ***nums[l]+nums[r]+nums[i] = 0*** 则通过判断l、r指针与其前/后元素是否相同加速指针移动。  

**代码示例**

Java:

```java
public List<List<Integer>> threeSum(int[] nums) {
    List<List<Integer>> res = new ArrayList<>();
    if (nums == null || nums.length <= 2) {
        return res;
    }
    Arrays.sort(nums);
    for (int i = 0; i < nums.length - 2; i++) {
        if (nums[i] > 0) {
            break;
        }
        if (i > 0 && nums[i] == nums[i - 1]) {
            continue;
        }
        int l = i + 1, r = nums.length - 1;
        while (l < r) {
            int sum = nums[i] + nums[l] + nums[r];
            if ( sum == 0) {
                res.add(Arrays.asList(nums[i],nums[l],nums[r]));
                while (l<r && nums[l] == nums[++l]);
                while (l<r && nums[r] == nums[--r]);
            } else if (sum < 0) {
                l++;
            } else if (sum > 0) {
                r--;
            }
        }
    }
    return res;
}
```

**复杂度分析**

时间复杂度：O(n^2)

空间复杂度：O(1)