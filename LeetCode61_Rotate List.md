## LeetCode61. Rotate List 旋转链表

### 原题链接

https://leetcode-cn.com/problems/rotate-list/

### 题目描述

给定一个链表，旋转链表，将链表每个节点向右移动 k 个位置，其中 k 是非负数。

示例 1:

```
输入: 1->2->3->4->5->NULL, k = 2
输出: 4->5->1->2->3->NULL
解释:
向右旋转 1 步: 5->1->2->3->4->NULL
向右旋转 2 步: 4->5->1->2->3->NULL
```

示例 2:

```
输入: 0->1->2->NULL, k = 4
输出: 2->0->1->NULL
解释:
向右旋转 1 步: 2->0->1->NULL
向右旋转 2 步: 1->2->0->NULL
向右旋转 3 步: 0->1->2->NULL
向右旋转 4 步: 2->0->1->NULL
```

## 解决方案

### 方法一：闭环确定首尾

**思路**

链表中的点已经相连，一次旋转操作意味着：

- 先将链表闭合成环
- 找到相应的位置断开这个环，确定新的链表头和链表尾

```java
class Solution {
    public ListNode rotateRight(ListNode head, int k) {
        if (head == null || head.next == null) {
            return head;
        }
        // 闭环，并计算链表长度
        int n;
        ListNode temp = head;
        for (n = 1; temp.next != null; n++) {
            temp = temp.next;
        }
        temp.next = head;

        // 确定首尾
        ListNode newTail = head;
        for (int i = 0; i < n - k % n - 1; i++) {
            newTail = newTail.next;
        }
        ListNode newHead = newTail.next;
        newTail.next = null;

        return newHead;
    }
}
```

**复杂度分析**

- 时间复杂度：*O*(N)，其中 N 是链表中的元素个数。
- 空间复杂度：*O*(1)，因为只需要常数的空间。

### 方法比较

| 算法           | 提交结果 | 执行用时 | 内存消耗 | 语言 |
| :------------- | :------- | :------- | :------- | :--- |
| 闭环后确定首尾 | 通过     | 1 ms     | 34.4 MB  | Java |

