[原题链接](<https://leetcode-cn.com/problems/reverse-integer/>)

## LeetCode7 Reverse Integer 反转整数

### 题目描述

给出一个 32 位的有符号整数，你需要将这个整数中每位上的数字进行反转。

**示例 1:**

```
输入: 123
输出: 321
```

 **示例 2:**

```
输入: -123
输出: -321
```

**示例 3:**

```
输入: 120
输出: 21
```

**注意:**

假设我们的环境只能存储得下 32 位的有符号整数，则其数值范围为 [−231,  231 − 1]。请根据这个假设，如果反转后整数溢出那么就返回 0。

### 初步思路

- 转化为字符串，反转字符串然后解析整数
- 当为负数时，负号会被反转到末端，此时需要截取"-"，并添加至字符串初始段
- 反转后字符串有可能在[INT_MIN,INT_MAX]之外，故解析错误返回0

### 算法

```java
public int reverse(int x) {
    StringBuilder builder = new StringBuilder();
    builder.append(x);
    builder.reverse();
    String value = builder.toString();
    if (value.contains("-")) {
        value = "-" + value.substring(0, value.length() - 1);
    }
    try {
        int result = Integer.parseInt(value);
        return result;
    } catch (Exception e) {
        return 0;
    }
}
```

时间复杂度:O(1)

空间复杂度:O(1)

题目中有如下注意,故应该不能采用。

> 假设我们的环境只能存储得下 32 位的有符号整数

## 标准答案

**方法：弹出和推入数字 & 溢出前进行检查**

逻辑如下:

```java
//pop operation: 取原数的低位
pop = x % 10;
x /= 10;

//push operation: 放入目标数的低位
temp = rev * 10 + pop;
rev = temp;
```

但是，这种方法很危险，因为当`temp = rev * 10 + pop;` 时会可能溢出。幸运的是，事先检查这个语句是否会导致溢出很容易。

1. 如果temp = rev * 10 + pop 导致溢出，那么一定有rev >= INT_MAX/10
2. 如果rev > INT_MAX / 10 , 那么temp = rev * 10 + pop 一定会溢出
3. 如果rev == INT_MAX/ 10， 那么只要pop > (INT_MAX  % 10) 就会溢出

```java
 public int reverse(int x) {
     int rev = 0;
     while (x != 0) {
         int pop = x % 10;
         x /= 10;
         if (rev > Integer.MAX_VALUE/10 || (rev == Integer.MAX_VALUE / 10 && pop > Integer.MAX_VALUE % 10)) return 0;
         if (rev < Integer.MIN_VALUE/10 || (rev == Integer.MIN_VALUE / 10 && pop < Integer.MIN_VALUE % 10)) return 0;
         rev = rev * 10 + pop;
     }
     return rev;
 }
```

### 复杂度分析

时间复杂度：O(log(x)) 
空间复杂度：O(1)

