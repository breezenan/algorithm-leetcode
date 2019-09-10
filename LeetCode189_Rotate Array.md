##  189. Rotate Array 旋转数组

### 原题链接

https://leetcode-cn.com/problems/rotate-array/

### 题目描述

给定一个数组，将数组中的元素向右移动 *k* 个位置，其中 *k* 是非负数。

示例 1:

```
输入: [1,2,3,4,5,6,7] 和 k = 3
输出: [5,6,7,1,2,3,4]
解释:
向右旋转 1 步: [7,1,2,3,4,5,6]
向右旋转 2 步: [6,7,1,2,3,4,5]
向右旋转 3 步: [5,6,7,1,2,3,4]
```

示例 2:

```
输入: [-1,-100,3,99] 和 k = 2
输出: [3,99,-1,-100]
解释: 
向右旋转 1 步: [99,-1,-100,3]
向右旋转 2 步: [3,99,-1,-100]
```

**说明:**

- 尽可能想出更多的解决方案，至少有三种不同的方法可以解决这个问题。
- 要求使用空间复杂度为 O(1) 的 **原地** 算法。

## 解决方案

### 方法 1：暴力法

最简单的方法是旋转 k 次，每次将数组旋转 1 个元素。

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

**复杂度分析**

- 时间复杂度：*O*(n\*k) 。旋转 1 次为 O(n), 旋转 k 次 共 O(n*k)
- 空间复杂度：O(1) 。没有额外空间被使用。

### 方法 2：使用反转

**思路**

将数组旋转 k 次，则数组尾部 k 个元素会被移到头部，其余元素为则构成了数组尾部，通过将数组反转即可实现上述要求，但为了元素顺序保持不变，需要将数组头部 k 个元素和尾部 n-k 个元素分别进行反转即可恢复元素顺序。k 实际又可能大于 数组长度 n，故运算时 `k = k % n`

演示：n 假设为 7 ，k 为3 ：

```
原始数组                  : 1 2 3 4 5 6 7
反转所有数字后             : 7 6 5 4 3 2 1
反转前 k 个数字后          : 5 6 7 4 3 2 1
反转后 n-k 个数字后        : 5 6 7 1 2 3 4 --> 结果
```

**实现**

```java
class Solution {
    public void rotate(int[] nums, int k) {
        k = k % nums.length;
        reverse(nums, 0, nums.length - 1);
        reverse(nums, 0, k - 1);
        reverse(nums, k, nums.length - 1);
    }

    private void reverse(int[] nums, int start, int end) {
        while (start < end) {
            int temp = nums[start];
            nums[start] = nums[end];
            nums[end] = temp;
            start++;
            end--;
        }
    }
} 

```

**复杂度分析**

- 时间复杂度：*O*(*n*) 。 *n* 个元素被反转了总共 3 次。
- 空间复杂度：*O*(1) 。 没有使用额外的空间。

### 算法比较

| 算法   | 提交结果 | 执行用时 | 内存消耗 | 语言 |
| :----- | :------- | :------- | :------- | :--- |
| 暴力法 | 通过     | 144 ms   | 39.3 MB  | Java |
| 反转法 | 通过     | 1 ms     | 39.5 MB  | Java |

