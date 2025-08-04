https://leetcode.com/explore/interview/card/top-interview-questions-easy/93/linked-list/772/
### Условие
Given the `head` of a singly linked list, return `true` _if it is a_ _palindrome_ _or_ `false` _otherwise_.
### Пример

**Ввод:** head = [1,2,2,1]
**Выход:** true

**Ввод:** head = [1,2]
**Выход:** false
### Решение

```go
func isPalindrome(head *ListNode) bool {
    var slow, fast, prev, next *ListNode = head, head, nil, nil
    
    for fast != nil && fast.Next != nil {
        slow = slow.Next
        fast = fast.Next.Next
    }
    prev = slow
    slow = slow.Next
    prev.Next = nil
    
    for slow != nil {
        next = slow.Next
        slow.Next = prev
        prev = slow
        slow = next
    }
    slow, fast = prev, head
    
    for slow != nil {
        if slow.Val != fast.Val {
            return false
        }
        slow, fast = slow.Next, fast.Next
    }
    return true
}
```
### Принцип 
Находим половину списка, после чего переворачиваем вторую половину. Далее проходим по обоим половинам, сравнивая соответствующие элементы, если они не равны, значит это не палиндром.