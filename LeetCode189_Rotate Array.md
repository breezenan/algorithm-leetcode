## 原题链接

https://leetcode-cn.com/problems/rotate-array/

## 题目描述

## 解决方案

### 方法一：暴力法

```java
class Solution {
    public void rotate(int[] nums, int k) {
        for (int i = 0; i < k; i++) {
            int value = nums[nums.length - 1];
            for (int j = nums.length - 1; j > 0; j--) {
                nums[j] = nums[j - 1];
            }
            nums[0] = value;
        }
    }
}
```

