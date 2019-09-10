两两合并

```java
class Solution {
    public ListNode mergeKLists(ListNode[] lists) {
        if (lists.length == 0) {
            return null;
        }
        int start = 0;
        return mergeLists(lists, start);
    }

    private ListNode mergeLists(ListNode[] lists, int start) {
        if (start == lists.length - 1) {
            return lists[start];
        }
        ListNode node = mergeLists(lists, start + 1);
        return mergeTwoLists(lists[start], node);
    }

    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        if (l1 == null) {
            return l2;
        }
        if (l2 == null) {
            return l1;
        }

        if (l1.val < l2.val) {
            l1.next = mergeTwoLists(l1.next, l2);
            return l1;
        } else {
            l2.next = mergeTwoLists(l1, l2.next);
            return l2;
        }
    }
}
```

```
执行用时 :528 ms, 在所有 Java 提交中击败了6.32%的用户
内存消耗 :52.9 MB, 在所有 Java 提交中击败了21.12%的用户
```

分治算法

```java
public ListNode mergeKLists(ListNode[] lists) {
    if (lists.length == 0) {
        return null;
    }

    return mergeLists(lists, 0, lists.length - 1);
}

private ListNode mergeLists(ListNode[] lists, int start, int end) {
    if (start == end) {
        return lists[start];
    }
    int mid = (start + end) / 2;
    ListNode leftNode = mergeLists(lists, start, mid);
    ListNode rightNode = mergeLists(lists, mid + 1, end);
    return mergeTwoLists(leftNode, rightNode);
}

public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
    if (l1 == null) {
        return l2;
    }
    if (l2 == null) {
        return l1;
    }

    if (l1.val < l2.val) {
        l1.next = mergeTwoLists(l1.next, l2);
        return l1;
    } else {
        l2.next = mergeTwoLists(l1, l2.next);
        return l2;
    }
}
```

```
执行用时 :4 ms, 在所有 Java 提交中击败了98.82%的用户
内存消耗 :40.4 MB, 在所有 Java 提交中击败了84.36%的用户
```

优先级队列

```java
class Solution {
        public ListNode mergeKLists(ListNode[] lists) {
            if (lists.length == 0) {
                return null;
            }
            PriorityQueue<ListNode> priorityQueue = new PriorityQueue<>(new Comparator<ListNode>() {
                @Override
                public int compare(ListNode listNode, ListNode t1) {
                    if (listNode.val > t1.val) {
                        return 1;
                    } else if(listNode.val == t1.val){
                        return 0;
                    } else {
                    	return -1;
                    }
                }
            });
            for (ListNode list : lists) {
                if (list != null) {
                    priorityQueue.add(list);     
                }
            }
            ListNode dummyNode = new ListNode(0);
            ListNode temp = dummyNode;
            while (!priorityQueue.isEmpty()) {
                temp.next = priorityQueue.poll();
                temp = temp.next;
                if (temp.next != null) {
                    priorityQueue.add(temp.next);
                }
            }
            return dummyNode.next;

        }

```

```
Line 18: java.lang.IllegalArgumentException: Comparison method violates its general contract!
```

compare中需要添加相等时的判断