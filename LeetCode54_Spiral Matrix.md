## LeetCode54 Spiral Matrix 螺旋矩阵

### 原题链接

https://leetcode-cn.com/problems/spiral-matrix/

### 题目描述

给定一个包含 m x n 个元素的矩阵（m 行, n 列），请按照顺时针螺旋顺序，返回矩阵中的所有元素。

示例 1:

```
输入:
[
 [ 1, 2, 3 ],
 [ 4, 5, 6 ],
 [ 7, 8, 9 ]
]
输出: [1,2,3,6,9,8,7,4,5]
```

示例 2:

```
输入:
[
  [1, 2, 3, 4],
  [5, 6, 7, 8],
  [9,10,11,12]
]
输出: [1,2,3,4,8,12,11,10,9,5,6,7]
```



## 解决方案

### 方法 1：按层模拟

**思路**

实现根据螺旋矩阵特性完成，其定义如下：

**螺旋矩阵** 就是从左上角开始，由外层到内层顺时针旋转添加元素，直到遍历到最内层，也就是所有元素添加完成构成的带有顺序的元素集合。

要实现螺旋矩阵就只需要顺时针由外层到内层添加元素即可，而当前层可以看做由`top、right、bottom、left `边元素构成。

用` r1、r2、c1、c2` 分别表示当前层的行起始终止下标，列起始终止下标。

- 添加 top 边，r 不变，c 在[c1, c2] 区间变化
- 添加 right 边，c 不变为c2，r 在 [r1+1, r2] 区间变化
- 添加 bottom 边，r 不变为r2，c 在 [c2, c1] 区间变化
- 添加 left 边，c 不变为c1，r 在[r2, r1+1] 区间变化

一层添加完后，r1++，r2--，c1++，c2-- 开始遍历下层，直到所有层添加完成。

**注意：一层存在 4 条边的条件为(r1 < r2) 并且 (c1 < c2)**

**实现**

```java
public List<Integer> spiralOrder(int[][] matrix) {
    List<Integer> ret = new ArrayList<>();
    if (matrix.length == 0 || matrix[0].length == 0) {
        return ret;
    }
    int r1 = 0, r2 = matrix.length - 1;
    int c1 = 0, c2 = matrix[0].length - 1;
    while (r1 <= r2 && c1 <= c2) {
        // top
        for (int c = c1; c <= c2; c++) {
            ret.add(matrix[r1][c]);
        }
        // right
        for (int r = r1 + 1; r <= r2; r++) {
            ret.add(matrix[r][c2]);
        }
        // has 4 sides
        if (r1 < r2 && c1 < c2) {
            // bottom
            for (int c = c2 - 1; c >= c1; c--) {
                ret.add(matrix[r2][c]);
            }
            // left
            for (int r = r2 - 1; r >= r1 + 1; r--) {
                ret.add(matrix[r][c1]);
            }
        }

        r1++;
        r2--;
        c1++;
        c2--;
    }
    return ret;
}
```

**复杂度分析**

- 时间复杂度： *O*(N)，其中 N 是输入矩阵所有元素的个数。因为我们将矩阵中的每个元素都添加进结果中。
- 空间复杂度： *O*(N)，需要存储矩阵中所有元素。

### 方法 2：模拟旋转

绘制螺旋轨迹路径，我们发现当**路径超出界限**或者**进入之前访问过的单元格**时，会顺时针旋转方向。就按照这个思维去实现算法。

假设数组有 **R** 行 **C** 列，`seen[r][c]` 表示第 `r` 行第 `c` 列的单元格之前已经被访问过了。当前所在位置为` (r, c)`，前进方向是 `di`。我们希望访问所有 `R x C` 个单元格。

```java
public List<Integer> spiralOrder(int[][] matrix) {
    List<Integer> ans = new ArrayList<>();
    if (matrix.length == 0 || matrix[0].length == 0) {
        return ans;
    }

    final int R = matrix.length, C = matrix[0].length;
    // 记录数组的给定位置是否已经遍历
    boolean[][] seen = new boolean[R][C];
    // 当前位置
    int r = 0, c = 0;
    
    // 顺时针方向，dr 为r的走向，dc 为c的走向
    // 下面数组表示方向为: 右->下->左->上
    int[] dr = {0, 1, 0, -1};
    int[] dc = {1, 0, -1, 0};
    int di = 0;

    for (int i = 0; i < R * C; i++) {
        ans.add(matrix[r][c]);
        System.out.println(r + "," + c);
        seen[r][c] = true;
        // 候选位置
        int cr = r + dr[di];
        int cc = c + dc[di];
        // 候选位置可用(没有超出界限，且没有访问过)
        if (cr >= 0 && cr < R && cc >= 0 && cc < C && !seen[cr][cc]) {
            r = cr;
            c = cc;
        } else {
            // 旋转
            di = (di + 1) % 4;
            r += dr[di];
            c += dc[di];
        }

    }

    return ans;
}
```

**复杂度分析**

- 时间复杂度： *O*(N)，其中 N 是输入矩阵所有元素的个数。因为我们将矩阵中的每个元素都添加进答案里。
- 空间复杂度： *O*(N)，需要两个矩阵 seen 和 ans 存储所需信息。

**算法比较**

| 算法     | 提交结果 | 执行用时 | 内存消耗 | 语言 |
| :------- | :------- | :------- | :------- | :--- |
| 按层模拟 | 通过     | 0 ms     | 34.4 MB  | Java |
| 模拟旋转 | 通过     | 0 ms     | 34.3 MB  | java |

