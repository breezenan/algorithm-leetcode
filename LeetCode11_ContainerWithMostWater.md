[官方原题链接](<https://leetcode-cn.com/problems/container-with-most-water/solution/sheng-zui-duo-shui-de-rong-qi-by-leetcode/>)

## LeetCode11. Container With Most Water 盛最多水的容器

### 题目描述

给定 n 个非负整数 a1，a2，...，an，每个数代表坐标中的一个点 (i, ai) 。在坐标内画 n 条垂直线，垂直线 i 的两个端点分别为 (i, ai) 和 (i, 0)。找出其中的两条线，使得它们与 x 轴共同构成的容器可以容纳最多的水。

**说明：**你不能倾斜容器，且 n 的值至少为 2。

![](https://aliyun-lc-upload.oss-cn-hangzhou.aliyuncs.com/aliyun-lc-upload/uploads/2018/07/25/question_11.jpg)

图中垂直线代表输入数组 [1,8,6,2,5,4,8,3,7]。在此情况下，容器能够容纳水（表示为蓝色部分）的最大值为 49。

**示例:**

```
输入: [1,8,6,2,5,4,8,3,7]
输出: 49
```

## 解决方案

**算法**

该种比较简单，遍历所有组合计算面积比较即可。

### 方法一：暴力法

```java
class Solution {
    public int maxArea(int[] height) {
        int maxArea = 0;
        for (int i = 0; i < height.length; i++) {
            for (int j = i + 1; j < height.length; j++) {
                int area = Math.min(height[i], height[j]) * (j - i);
                if (area > maxArea) {
                    maxArea = area;
                }
            }
        }
        return maxArea;
    }
}
```

**复杂度分析**

- 时间复杂度：O(n^2^)
- 空间复杂度：O(1)

### 方法二：双指针法

**算法**

![](https://pic.leetcode-cn.com/Figures/11_Container_WaterSlide1.PNG)

结合图来说明逻辑

使用下标`i`指向左侧，下标`j`指向右侧，当`i`和`j`的宽度最大时，面积总是以高度低着为准，此时如果当低着与其他任何线段再组合时总是小于当前的宽度的面积，故唯一让面积增大的可能就是通过高着作为基准去改变宽度,为了面积最大，宽度依然要保持最大去寻找,直到i和j相遇就可以查找到最大者。

**图中使用双指针步骤：**

1. 第1次：i=0,j=8 宽度为8，面积为8*1 = 8,     由于高度低着`i`与其他任何线段组合都无法增大面积，故基准需要以j=8为准改变宽度去查找是否有面积更大着，此时使i++改变宽度

2. 第2次：i=1,j=8 宽度为7，面积为7*7 = 49，由于高度低着`j`与其他任何线段组合都无法增大面积，故基准需要以i=1为准改变宽度去查找是否有面积更大着，此时使j--改变宽度

3. 第3次：i=1,j=7 宽度为6，面积为6*3 = 18，由于高度低着`j`与其他任何线段组合都依然无法增大面积，故基准需要以i=1继续为准改变宽度去查找是否有面积更大着，此时j--改变宽度

4. 第4次：i=1,j=6 宽度为5，面积最大为40，为了继续寻找面积最大，由于i和j的高度均为8，故可以继续以i=1为基准

5. ...

按照此方法依次类推，直到i和j相遇停止，所有面积可能会更大的情况都遍历完了，然后就可以得到面积最大着。

```java
public int maxArea(int[] height) {
    int maxArea = 0;
    int i = 0;
    int j = height.length - 1;
    while (i < j) {
        int area = Math.min(height[i], height[j]) * (j - i);
        if (area > maxArea) {
            maxArea = area;
        }
        if (height[i] > height[j]) {
            j--;
        } else {
            i++;
        }
    }
    return maxArea;
}
```

**复杂度分析**

- 时间复杂度：*O*(n)，一次扫描。
- 空间复杂度：*O*(1)，使用恒定的空间。