## 33. Search in Rotated Sorted Array 搜索旋转排序数组

### 原题链接

https://leetcode-cn.com/problems/search-in-rotated-sorted-array/

### 题目描述

假设按照升序排序的数组在预先未知的某个点上进行了旋转。

( 例如，数组 `[0,1,2,4,5,6,7] `可能变为` [4,5,6,7,0,1,2]` )。

搜索一个给定的目标值，如果数组中存在这个目标值，则返回它的索引，否则返回 -1 。

你可以假设数组中不存在重复的元素。

你的算法时间复杂度必须是 O(log n) 级别。

示例 1:

```
输入: nums = [4,5,6,7,0,1,2], target = 0
输出: 4
```

示例 2:

```
输入: nums = [4,5,6,7,0,1,2], target = 3
输出: -1
```

## 解决方案

### 二分查找

**思路**

由于题目要求时间复杂度为 O(log n)，故实现为**二分法查找**，关于二分法，一般存在 low,high,mid位，来辅助判断。

- 如果 target 在`[mid+1,high]` 序列中，则`low = mid+1`,否则` high = mid`,关键是如何判断 target在`[mid+1,high]`序列中,具体判断如下：
- 当`[0, mid]` 序列是升序: `nums[0] <= nums[mid]`, 当`target > nums[mid] || target < nums[0]` ,则向后规约
- 当`[0, mid]` 序列存在旋转位: `nums[0] > nums[mid]`,当`target < nums[0] && target > nums[mid]`,则向后规约
- 当然其他其他情况就是向前规约了

循环判断，直到排除到只剩最后一个元素时，退出循环，如果该元素和 target 相同，直接返回下标，否则返回 -1。

**实现**

```java
class Solution {
	public int search(int[] nums, int target) {
        int lo = 0;
        int hi = nums.length - 1;

        while (lo < hi) {
            int mid = (lo + hi) / 2;
            // 当[0,mid]有序时,向后规约条件
            if (nums[0] <= nums[mid] && (target > nums[mid] || target < nums[0])) {
                lo = mid + 1;
                // 当[0,mid]发生旋转时，向后规约条件
            } else if (target > nums[mid] && target < nums[0]) {
                lo = mid + 1;
            } else {
                hi = mid;
            }
        }
        return lo == hi && nums[lo] == target ? lo : -1;
    }
}
```

关于思路相同，但更简洁写法可以参考https://leetcode-cn.com/problems/search-in-rotated-sorted-array/solution/ji-jian-solution-by-lukelee/,其中使用了疑惑思想，很精妙，但是相对难理解。

**复杂度分析**

- 时间复杂度：*O*(*log n*) 。二分法查找，每次排除一半元素，故执行次数为 x 存在 `2^x^ = n`, 则 `x = log~2~n` ,故时间复杂度为O(log n)。
- 空间复杂度：*O*(1) 。 没有使用额外的空间。

### 算法比较

| 提交时间 | 提交结果 | 执行用时 | 内存消耗 |
| :------- | :------- | :------- | :------- |
| 二分查找 | 通过     | 1 ms     | 35.5 MB  |