## 题目链接

https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array/

## 题目描述

## 解决方案

### 思路



### 双指针法

```java
class Solution {
    public int removeDuplicates(int[] nums) {
        if (nums.length == 0) {
            return 0;
        }
        int index = 0;
        for (int i = 1; i < nums.length; i++) {
            if (nums[index] != nums[i]) {
                nums[++index] = nums[i];
            }
        }
        return index + 1;
    }
}
```

