[原题链接](https://leetcode-cn.com/problems/3sum-closest/)

# LeetCode16. 3 Sum Closest 最接近的３数之和

给定一个包括 n 个整数的数组 nums 和 一个目标值 target。找出 nums 中的三个整数，使得它们的和与 target 最接近。返回这三个数的和。假定每组输入只存在唯一答案。

```
例如，给定数组 nums = [-1，2，1，-4], 和 target = 1.

与 target 最接近的三个数的和为 2. (-1 + 2 + 1 = 2).
```

## 思路

本题目因为要计算三个数，如果靠暴力枚举的话时间复杂度会到 O(n^3^)，需要降低时间复杂度

- 数组进行一次排序,从而可以通过双指针的移动减少一层遍历
- 在数组 nums 中，进行遍历，每遍历一个值利用其下标i，形成一个固定值 nums[i]
- 使用低位指针`lw = i + 1`,高位指针为`hi = len-1`指向其他两个数
- 通过三数之和`sum = nums[i] + nums[lw] + nums[hi]`，判断sum到target的距离，如果更接近则更新结果ans
- 同时判断 sum 与 target 的大小关系，因为数组有序，如果 sum > target 则 end--，如果 sum < target 则 start++，如果 sum == target 则说明距离为 0 直接返回结果

## 算法：排序＋双指针

```java
class Solution {
    public int threeSumClosest(int[] nums, int target) {
        Arrays.sort(nums);
        int ans = nums[0] + nums[1] + nums[2];　// 记录当前最接近target的三数之和
        for (int i = 0; i < nums.length - 2; i++) {
            //　跳过第一个数是相同的数
            if (i != 0 && nums[i] == nums[i - 1]) continue;

            int lw = i + 1, hi = nums.length - 1;
            while (lw < hi) {
                int sum = nums[i] + nums[lw] + nums[hi];
                if (Math.abs(target - sum) < Math.abs(target - ans)) {
                    ans = sum;
                }
                if (sum < target) {
                    lw++;
                } else if (sum > target) {
                    hi--;
                } else {
                    return sum;
                }
            }
        }
        return ans;
    }
}
```

执行结果：

```
执行用时 :15 ms, 在所有 Java 提交中击败了68.29%的用户
内存消耗 :36.5 MB, 在所有 Java 提交中击败了84.07%的用户
```

**复杂度分析**

- 时间复杂度：O(n^2^),整个遍历过程中对值都是顺序遍历，且有两层循环，故时间复杂度为O(n^2^),之前的排序算法复杂度为O(nlogn),故算法最终时间复杂度依然为O(n^2^)

- 空间复杂度：O(1), 整个算法没有额外空间的消耗，故空间复杂度为O(1)