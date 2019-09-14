## 415. Add Strings  字符串相加

### 原题链接

https://leetcode-cn.com/problems/add-strings/

### 题目描述

给定两个字符串形式的非负整数 num1 和num2 ，计算它们的和。

**注意：**

1. num1 和num2 的长度都小于 5100.
2. num1 和num2 都只包含数字 0-9.
3. num1 和num2 都不包含任何前导零。
4. **你不能使用任何內建 BigInteger 库， 也不能直接将输入的字符串转换为整数形式。**

## 解决方案

### 双指针法

**思路:**

需要对两个字符串逐位相加，需要考虑**进位**情况。

- 使用` i`,` j` 两个指针分别指向 `num1`,` num2` 的尾部，模拟人工加法；
- **计算当前位：**`(n1 + n2 + carry) % 10`,可能存在进位，故相加时需要添加前一次的进位；
- **计算进位：**`(n1 + n2 + carry)/ 10` 即是进位
- **索引溢出处理：**遍历运算，当指针`i`或`j`超出数字首位后，可以将对应位 n1 或 n2 置为 0，方便运算
- **循环退出条件：**除了指针`i`和`j`均已超出数字首部后，我们还需要判断进位是否存在，只有均不满足时才退出循环

**实现**

```java
class Solution {
    public String addStrings(String num1, String num2) {
        StringBuilder builder = new StringBuilder();
        int carry = 0;
        for (int i = num1.length() - 1, j = num2.length() - 1;
             i >= 0 || j >= 0 || carry != 0;
             i--, j--) {
            int x = i < 0 ? 0 : num1.charAt(i) - '0';
            int y = j < 0 ? 0 : num2.charAt(j) - '0';
            int sum = (x + y + carry) % 10;
            builder.append(sum);
            carry = (x + y + carry) / 10;
        }
        return builder.reverse().toString();
    }
}
```

**复杂度分析**

- 时间复杂度：*O*(max(M,N))。M，N分别为两个字符串长度。
- 空间复杂度：*O*(1)。