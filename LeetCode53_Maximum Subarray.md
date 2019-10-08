## LeetCode53 Maximum Subarray 最大子序和

### 原题链接

https://leetcode-cn.com/problems/maximum-subarray/

### 题目描述

给定一个整数数组 nums ，找到一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。

示例：

```
输入: [-2,1,-3,4,-1,2,1,-5,4],
输出: 6
解释: 连续子数组 [4,-1,2,1] 的和最大，为 6。
```

进阶：

如果你已经实现复杂度为 O(n) 的解法，尝试使用更为精妙的分治法求解。

## 解决方案

### 方法 1：暴力法

```java
class Solution {
    public int maxSubArray(int[] nums) {
        int max = nums[0];
        for (int i = 0; i < nums.length; i++) {
            int temp = 0;
            for (int j = i; j < nums.length; j++) {
                temp += nums[j];
                if (temp > max) max = temp;
            }

        }
        return max;
    }
}
```

**复杂度分析**

- 时间复杂度：O(n^2^)。双层 for 循环嵌套。
- 空间复杂度：O(1)。

### 方法 2：一般方法

```java
class Solution {
    public int maxSubArray(int[] nums) {
        int max = nums[0];
        int temp = max;
        for (int i = 1; i < nums.length; i++) {
            if (temp < 0) {
                temp = nums[i];
            } else {
                temp += nums[i];
            }
			if (temp > max) max = temp;
        }
        return max;
    }
}
```

**复杂度分析**

- 时间复杂度：O(n)。
- 空间复杂度：O(1)。

### 进阶：分治法

题目中的进阶方法个人感觉分治法并不适合该题，反而容易把人搞晕，故该方法在本题中不给出