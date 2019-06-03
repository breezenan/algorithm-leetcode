[原题链接](https://leetcode-cn.com/problems/add-two-numbers/)

## LeetCode2. Add Two Numbers 两数相加  

### 题目描述

给出两个 **非空** 的链表用来表示两个非负的整数。其中，它们各自的位数是按照 **逆序** 的方式存储的，并且它们的每个节点只能存储 **一位** 数字。

如果，我们将这两个数相加起来，则会返回一个新的链表来表示它们的和。

您可以假设除了数字 0 之外，这两个数都不会以 0 开头。

**输出**

```
输入：(2 -> 4 -> 3) + (5 -> 6 -> 4)
输出：7 -> 0 -> 8
原因：342 + 465 = 807
```

### 思路

遍历两个链表，从左到右对应位置的节点值相加并作为新的链表对应位置的节点值。

注意点:

- 某个位置值相加大于10时存在进位
- 两个列表可能不等长

### 算法

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
    ListNode p = l1;
    ListNode q = l2;
    ListNode dummyHead = new ListNode(0); // 用于作为结果的新链表头结点，其为哑结点，简化后续赋值
    ListNode temp = dummyHead; 
    int carry = 0; // 进位
    while (p != null || q != null || carry != 0) {
        int x = p == null ? 0 : p.val;
        int y = q == null ? 0 : q.val;
        int sum = x + y + carry; // 对应位置的和
        carry = sum / 10; // 对应位置算出的进位，应应用于下个位置的求和
        temp.next = new ListNode(sum % 10);
        temp = temp.next;
        if (p != null) p = p.next;
        if (q != null) q = q.next;
    }
    return dummyHead.next;
}
```

### 复杂度分析

- 时间复杂度:  假设l1和l2的长度分别为m和n,则时间复杂度为O(max(m,n))
- 空间复杂度:  O(max(m,n)),while循环最大执行次数为max(m,n)+1,也就意味着开辟这么多次ListNode的空间，从而确定空间复杂度为线性阶，也就是O(n),此题就是O(max(m,n))

### 自测

```java
public class LeetCode2_AddTwoNumbers {
    public static void main(String[] args) {
        /*
        测试用例
        1. [1,2] [3,4] ,[4,6]
        2. [1] [3,6] ,[4,6]
        3. [2,5,1] [3,5,1]  ,[5,0,3]
        4. [9,9] [9] ,[8,0,1]
         */
        ListNode l1 = new ListNode(9);
        l1.next = new ListNode(9);
        ListNode l2 = new ListNode(9);
        ListNode result = new Solution().addTwoNumbers(l1, l2);
        print(result);
    }

    private static void print(ListNode node) {
        ListNode temp = node;
        StringBuilder builder = new StringBuilder();
        builder.append("[");
        while (temp != null) {
            builder.append(temp.val);
            temp = temp.next;
            if (temp != null) {
                builder.append(",");
            }
        }
        builder.append("]");
        System.out.println(builder.toString());
    }
}

class ListNode {
    int val;
    ListNode next;

    ListNode(int x) {
        val = x;
    }
}

class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode p = l1;
        ListNode q = l2;
        ListNode dummyHead = new ListNode(0);
        ListNode temp = dummyHead;
        int carry = 0;
        while (p != null || q != null || carry != 0) {
            int x = p == null ? 0 : p.val;
            int y = q == null ? 0 : q.val;
            int sum = x + y + carry;
            carry = sum / 10;
            temp.next = new ListNode(sum % 10);
            temp = temp.next;
            if (p != null) p = p.next;
            if (q != null) q = q.next;
        }
        return dummyHead.next;
    }

}
```

