[原题链接](https://leetcode-cn.com/problems/longest-common-prefix/)

## LeetCode14. Longest Common Prefix 最长公共前缀

### 题目

编写一个函数来查找字符串数组中的最长公共前缀。

如果不存在公共前缀，返回空字符串 ""。

**示例 1:**

```
输入: ["flower","flow","flight"]
输出: "fl"
```

**示例 2:**

```
输入: ["dog","racecar","car"]
输出: ""
解释: 输入不存在公共前缀。
```

**说明:**

所有输入只包含小写字母 a-z 。



### 算法1：水平扫描法

**思路**

假设字符串数组为S1,S2,...Sn,先拿到LCP(S1,S2),再拿到LCP(LCP(S1,S2),S3),依次类推，直到将字符串扫描完即可拿到最终的LCP。



![](https://pic.leetcode-cn.com/b647cab7c3d2bd157cecae10917e0b9b671756b92c9cfcefec1a2bdae299c11c-file_1555694071243)

**代码**

```java
public String longestCommonPrefix(String[] strs) {
    if (strs == null || strs.length == 0) return "";
    String prefix = strs[0];
    for (int i = 1; i < strs.length; i++) {
        while (strs[i].indexOf(prefix) != 0) {
            prefix = prefix.substring(0, prefix.length() - 1);
            if (prefix.isEmpty()) return "";
        }
    }
    return prefix;
}
```

该方法主要是通过indexOf方法返回0判断是否包含公共字符串，一开始没有想到该方法，主要还是对其不熟悉，下面给出indexOf方法的使用

```java
System.out.println("hello".indexOf("helloy")); // -1
System.out.println("hello".indexOf("")); // 0;
System.out.println("hello".indexOf("hell")); // 0
System.out.println("hell".indexOf("hello")); // -1
// 可见只要前者字符串包含给定字符串，返回的是给定字符串在左侧字符串的起始下标
```

**复杂度分析**

- 时间复杂度*O*(S),S是所有字符串字符的总和	

  最坏的情况下，n 个字符串都是相同的。算法会将 S1 与其他字符串 S2 ...  Sn都做一次比较。这样就会进行 S 次字符比较，其中 S 是输入数据中所有字符数量。


- 空间复杂度：*O*(1)，我们只需要使用常数级别的额外空间

> 个人看法：n个字符串相同，应该时间复杂度为O(n),因为一旦相同，while条件只会执行一次就退出，相当于只有一层for循环。唯一缺点就是如果一个字符串特别短，该方法还会对每个字符串进行比较。

### 算法2：水平扫描法

**思路**

想象数组的末尾有一个非常短的字符串，使用上面方法依旧会进行 S 次比较。优化这类情况的一种方法就是水平扫描。我们从前往后枚举字符串的每一列，先比较每个字符串相同列上的字符（即不同字符串相同下标的字符）然后再进行对下一列的比较，是逐列扫描。

**代码**

```java
public String longestCommonPrefix(String[] strs) {
    if (strs == null || strs.length == 0) return "";
    for (int i = 0; i < strs[0].length(); i++) {
        char c = strs[0].charAt(i);
        for (int j = 1; j < strs.length; j++) {
            if (i == strs[j].length() || strs[j].charAt(i) != c) {
                return strs[0].substring(0, i);
            }
        }
    }
    return strs[0];
}
```

**复杂度分析**

- 时间复杂度：*O*(S)，S 是所有字符串中字符数量的总和。

  最坏情况下，输入数据为 n 个长度为 m 的相同字符串，算法会进行S=m∗n 次比较。可以看到最坏情况下，本算法的效率与算法一相同。但是最好的情况下，算法只需要进行 n*minLen次比较，其中 minLen是数组中最短字符串的长度。

- 空间复杂度：*O*(1)，我们只需要使用常数级别的额外空间。

### 算法3：分治

**思路**

1. 分，我们将问题分为求左侧和右侧字符串数组的LCP，直到分为找两个字符串的LCP
2. 治，计算左右两个字符串的LCP并返回,直到返回到初始调用层，就返回了最终的LCP

![](https://pic.leetcode-cn.com/8bb79902c99719a923d835b9265b2dea6f20fe7f067f313cddcf9dd2a8124c94-file_1555694229984)

```java
public String longestCommonPrefix(String[] strs) {
    if (strs == null || strs.length == 0) return "";
    return longestCommonPrefix(strs, 0, strs.length - 1);
}

private String longestCommonPrefix(String[] strs, int l, int r) {
    if (l == r)
        return strs[r]; //单个字符串的公共字符串肯定是自己,此时返回

    int mid = (r + l) / 2;
    String lcpLeft = longestCommonPrefix(strs, l, mid);
    String lcpRight = longestCommonPrefix(strs, mid + 1, r);
    return mergeCommonPrefix(lcpLeft, lcpRight);
}

/**
 * @param left 左lcp
 * @param right 右lcp
 * @return 左lcp和右lcp的lcp
 */
private String mergeCommonPrefix(String left, String right) {
    int min = Math.min(left.length(), right.length());
    for (int i = 0; i < min; i++) {
        if (left.charAt(i) != right.charAt(i))
            return left.substring(0, i);

    }
    return left.substring(0, min);
}
```

**复杂度分析**

最坏情况下，我们有 n 个长度为 m的相同字符串。

- 时间复杂度：O(S)，S 是所有字符串中字符数量的总和，S=m∗n。

  时间复杂度的递推式为 $T(n)=2\cdot T(\frac{n}{2})+O(m)$， 化简后可知其就是 O(S)。最好情况下，算法会进行 minLen$\cdot $n次比较，其中 minLen 是数组中最短字符串的长度。

- 空间复杂度：O($m \cdot log(n)$)，内存开支主要是递归过程中使用的栈空间所消耗的。 一共会进行 log(n)次递归，每次需要 m 的空间存储返回结果，所以空间复杂度为 O($m\cdot log(n)$。



其他算法还有二分查找法、前缀树，这块暂时没有实现，后续补充。

### 本人原实现

```java
public String longestCommonPrefix(String[] strs) {
    StringBuilder common = new StringBuilder();
    int minLen = Integer.MAX_VALUE;
    // 找到最小字符串长度
    for (int i = 0; i < strs.length; i++) {
        minLen = Math.min(strs[i].length(), minLen);
    }
    boolean flag = false;
    for (int j = 0; j < minLen && !flag; j++) {
        int index = 0;
        while (index < strs.length - 1) {
            // 前一个字符串字符和后一个字符串同位置字符比较
            if (strs[index].charAt(j) != strs[index + 1].charAt(j)) {
                flag = true;
                break;
            }
            index++;
        }
        // 找到一个相同字符，添加到StringBuilder中
        if (index == strs.length - 1) {
            common.append(strs[0].charAt(j));
        }
    }

    return common.toString();
}
```

思想和算法2相同，但实现起来没有其简洁。