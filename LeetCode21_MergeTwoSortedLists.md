## LeetCode21. Merge Two Sorted Lists 合并两个有序链表

### 题目

[原题链接](https://leetcode-cn.com/problems/merge-two-sorted-lists/)

将两个有序链表合并为一个新的有序链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。 

示例：

```
输入：1->2->4, 1->3->4
输出：1->1->2->3->4->4
```

## 题解

### 方法 1：递归

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        if(l1 == null) {
            return l2;
        }
        if(l2 == null) {
            return l1;
        }

        if(l1.val < l2.val) {
            l1.next = mergeTwoLists(l1.next, l2);
            return l1;
        } else {
            l2.next = mergeTwoLists(l1, l2.next);
            return l2;
        }
    }
}
```



### 方法 2：迭代

```java
class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        // 哑结点，该引用不会改变，其作为返回node的头结点
        ListNode prehead = new ListNode(-1);

        ListNode prev = prehead;
        while (l1 != null && l2 != null) {
            if (l1.val <= l2.val) {
                prev.next = l1;
                l1 = l1.next;
            } else {
                prev.next = l2;
                l2 = l2.next;
            }
            prev = prev.next;
        }

        // 退出循环时l1或者l2中某一个可能不能null，剩余结点都是有序的，故只需与目标链接连接即可
        prev.next = l1 == null ? l2 : l1;

        return prehead.next;
    }
}
```

## 草稿

### 迭代法

1. 创建哑结点dummyHead，dummyHead.next是我们最终需要的链表
2. 创建新节点，将两个给定链表l1,l2中节点较小的值作为创建的新节点的值并赋值给目标链表,对于两个链表中较小值的节点需要指向下个节点，下次循环继续比较，直到有一个链表为空时退出
3. 一个链表为空，则另外一个链表可直接追加在目标链表中

```java
class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        ListNode dummyHead = new ListNode(0);
        ListNode temp = dummyHead;
        while (l1 != null && l2 != null) {
            int value;
            if (l1.val < l2.val) {
                value = l1.val;
                l1 = l1.next;
            } else {
                value = l2.val;
                l2 = l2.next;
            }
            temp.next = new ListNode(value);
            temp = temp.next;
        }
        temp.next = l1 == null ? l2 : l1;
        return dummyHead.next;
    }
}
```

执行结果：

```
执行用时：1ms,在所有 Java 提交中击败了99.79%的用户
内存消耗： 36.3 MB,在所有 Java 提交中击败了87.31%的用户
```