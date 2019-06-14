[原题链接](https://leetcode-cn.com/problems/two-sum/)

## LeetCode1. Two Sum 两数之和

### 题目

给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那 两个 整数，并返回他们的数组下标。

你可以假设每种输入只会对应一个答案。但是，你不能重复利用这个数组中同样的元素。

```
示例:

给定 nums = [2, 7, 11, 15], target = 9

因为 nums[0] + nums[1] = 2 + 7 = 9
所以返回 [0, 1]
```

### 算法1：暴力法

暴力法就是将两个不同元素组合是否等于target

```java
public int[] twoSum(int[] nums, int target) {
    for (int i = 0; i < nums.length - 1; i++) {
        int next = target - nums[i];
        for (int j = i + 1; j < nums.length; j++) {
            if (nums[j] == next) return new int[]{i, j};
        }
    }
    return null;
}
```

**复杂度分析**

- 时间复杂度*O*(n^2^)
- 空间复杂度*O*(1)

### 算法2：两遍哈希表

为了对运行时间复杂度进行优化，我们需要一种更有效的方法来检查数组中是否存在目标元素。如果存在，我们需要找出它的索引。保持数组中的每个元素与其索引相互对应的最好方法是什么？哈希表。

通过以空间换取速度的方式，我们可以将查找时间从 O(n) 降低到 O(1)。哈希表正是为此目的而构建的，它支持以 近似 恒定的时间进行快速查找。我用“近似”来描述，是因为一旦出现冲突，查找用时可能会退化到 O(n)。但只要你仔细地挑选哈希函数，在哈希表中进行查找的用时应当被摊销为O(1)。

```java
public int[] twoSum(int[] nums, int target) {
    Map<Integer, Integer> map = new HashMap();
    // 存取所有值到哈希表
    for (int i = 0; i < nums.length; i++) {
        map.put(nums[i], i);
    }
	// 遍历元素作为第一个元素
    for (int i = 0; i < nums.length - 1; i++) {
        int next = target - nums[i];
        // 查找目标元素
        if (map.containsKey(next) && map.get(next) != i) {
            return new int[]{i, map.get(next)};
        }
    }
    return null;
}
```

**复杂度分析**

- 时间复杂度：O(n)， 我们把包含有 n 个元素的列表遍历两次。由于哈希表将查找目标元素时间缩短到 O(1) ，所以时间复杂度为 O(n)。

- 空间复杂度：O(n)， 所需的额外空间取决于哈希表中存储的元素数量，该表中存储了 n 个元素。

### 算法3：一遍哈希表

元素迭代时可以同时插入元素到哈希表，可以想象我们将遍历元素作为结果的第二个元素，而第一个元素只需要在第二个元素之前查找即可，可以一定程度减少查找时间。

```java
public int[] twoSum(int[] nums, int target) {
    Map<Integer, Integer> map = new HashMap();
    for (int i = 0; i < nums.length; i++) {
        int next = target - nums[i];
        if (map.containsKey(next) && map.get(next) != i) {
            return new int[]{map.get(next), i};
        }
        map.put(nums[i], i);
    }
    return null;
}
```

**复杂度分析**

- 时间复杂度：O(n)

- 空间复杂度：O(n)