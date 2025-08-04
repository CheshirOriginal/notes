https://leetcode.com/explore/learn/card/linked-list/219/classic-problems/1207/
### Условие
Given the `head` of a linked list and an integer `val`, remove all the nodes of the linked list that has `Node.val == val`, and return _the new head_.
### Пример

**Ввод:** head = [1,2,6,3,4,5,6], val = 6
**Выход:** [1,2,3,4,5]

**Ввод:** head = [7,7,7,7], val = 7
**Выход:** []
### Решение

```go
func removeElements(head *ListNode, val int) *ListNode {
    if head == nil {
        return head
    }
    for head != nil && head.Val == val {
        head = head.Next
    }
    if head == nil {
        return head
    }
    prev, curr := head, head.Next
    for curr != nil {
        if curr.Val == val {
            prev.Next = curr.Next
            curr = prev.Next
        } else {
            prev = prev.Next
            curr = curr.Next
        }
    }
    return head
}
```
