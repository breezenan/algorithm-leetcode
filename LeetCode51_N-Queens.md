#### 51. N-Queens

题目链接

https://leetcode-cn.com/problems/n-queens/

题目描述

*n* 皇后问题研究的是如何将 *n* 个皇后放置在 *n*×*n* 的棋盘上，并且使皇后彼此之间不能相互攻击。

![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/10/12/8-queens.png)

上图为 8 皇后问题的一种解法。

给定一个整数 n，返回所有不同的 n 皇后问题的解决方案。

每一种解法包含一个明确的 n 皇后问题的棋子放置方案，该方案中 'Q' 和 '.' 分别代表了皇后和空位。

```
输入: 4
输出: [
 [".Q..",  // 解法 1
  "...Q",
  "Q...",
  "..Q."],

 ["..Q.",  // 解法 2
  "Q...",
  "...Q",
  ".Q.."]
]
解释: 4 皇后问题存在两个不同的解法。
```

解决办法

**思路**



**实现**

```java
class Solution {
    /**
     * 所有解
     */
    List<List<String>> output = new ArrayList<>();
    /**
     * 单条解
     */
    List<String> solution = new ArrayList<>();
    int n;

    public List<List<String>> solveNQueens(int n) {
        this.n = n;
        backtrack(0);
        return output;
    }

    private void backtrack(int row) {
        for (int i = 0; i < n; i++) {
            StringBuilder builder = new StringBuilder();
            for (int j = 0; j < n; j++) {
                if (j == i) {
                    builder.append("Q");
                } else {
                    builder.append(".");
                }
            }
            // 是否满足彼此互不攻击条件
            if (isNotUnderAttack(row, i)) {
                solution.add(builder.toString());
            } else {
                continue;
            }

            if (row == n - 1) {
                // 行为最后一行，且已找到皇后位置，则添加解
                output.add(new ArrayList<String>(solution));
            } else {
                backtrack(row + 1);
            }
            solution.remove(solution.size() - 1);
        }
    }

    /**
     * @param row 皇后行坐标
     * @param col 皇后列坐标
     * @return 目标皇后与前面所有皇后比较，如果行相同，或列相同，或所在直线的斜率为1或-1返回false，否则true
     */
    private boolean isNotUnderAttack(int row, int col) {
        if (solution.size() == 0) {
            return true;
        }

        for (int i = 0; i < solution.size(); i++) {
            int j = solution.get(i).indexOf("Q");
            if (row == i || col == j || (row - i) / (float) (col - j) == 1 || (row - i) / (float) (col - j) == -1) {
                return false;
            }
        }
        return true;
    }
}
```



```java
class Solution {
  int rows[];
  // "hill" diagonals
  int hills[];
  // "dale" diagonals
  int dales[];
  int n;
  // output
  List<List<String>> output = new ArrayList();
  // queens positions
  int queens[];

  public boolean isNotUnderAttack(int row, int col) {
    int res = rows[col] + hills[row - col + 2 * n] + dales[row + col];
    return (res == 0) ? true : false;
  }

  public void placeQueen(int row, int col) {
    queens[row] = col;
    rows[col] = 1;
    hills[row - col + 2 * n] = 1;  // "hill" diagonals
    dales[row + col] = 1;   //"dale" diagonals
  }

  public void removeQueen(int row, int col) {
    queens[row] = 0;
    rows[col] = 0;
    hills[row - col + 2 * n] = 0;
    dales[row + col] = 0;
  }

  public void addSolution() {
    List<String> solution = new ArrayList<String>();
    for (int i = 0; i < n; ++i) {
      int col = queens[i];
      StringBuilder sb = new StringBuilder();
      for(int j = 0; j < col; ++j) sb.append(".");
      sb.append("Q");
      for(int j = 0; j < n - col - 1; ++j) sb.append(".");
      solution.add(sb.toString());
    }
    output.add(solution);
  }

  public void backtrack(int row) {
    for (int col = 0; col < n; col++) {
      if (isNotUnderAttack(row, col)) {
        placeQueen(row, col);
        // if n queens are already placed
        if (row + 1 == n) addSolution();
          // if not proceed to place the rest
        else backtrack(row + 1);
        // backtrack
        removeQueen(row, col);
      }
    }
  }

  public List<List<String>> solveNQueens(int n) {
    this.n = n;
    rows = new int[n];
    hills = new int[4 * n - 1];
    dales = new int[2 * n - 1];
    queens = new int[n];

    backtrack(0);
    return output;
  }
}
```

