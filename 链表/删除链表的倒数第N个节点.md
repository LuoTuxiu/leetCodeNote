##  删除链表的倒数第N个节点

给定一个链表，删除链表的倒数第 n 个节点，并且返回链表的头结点。

示例：

```

给定一个链表: 1->2->3->4->5, 和 n = 2.

当删除了倒数第二个节点后，链表变为 1->2->3->5.
```

说明：

给定的 n 保证是有效的。

进阶：

你能尝试使用一趟扫描实现吗？


我的第一个思路： (用时84 ms)

> 用快慢指针，相隔n - 1 就可以在到达底部时，取得倒数第N个的数据


```
/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */
/**
 * @param {ListNode} head
 * @param {number} n
 * @return {ListNode}
 */
var removeNthFromEnd = function(head, n) {
    let firstNode =  head;
    let lastNode = head;
    let count = n - 1;
    if(count === 0 && head.next === null) {
        return null
    }
    while(count-- && lastNode) {
        lastNode = lastNode.next;
    }
    let lastNodeBefore = firstNode;
    if(lastNode.next) {
         while(lastNode && lastNode.next) {
            lastNodeBefore = firstNode;
            firstNode = firstNode.next;
            lastNode = lastNode.next;
        }
        // 开始删除操作
        lastNodeBefore.next = firstNode.next;
        return head;
    } else {
        return head.next;
    }

};
```

