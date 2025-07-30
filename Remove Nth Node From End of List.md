
https://leetcode.com/explore/learn/card/linked-list/214/two-pointer-technique/1296/
### Условие
Дан заголовок односвязного списка. Удалите n-ый элемент с конца списка и верните заголовок.
### Решение

```go
func removeNthFromEnd(head *ListNode, n int) *ListNode {
    if head == nil || n <= 0 {
        return head
    }
    slow, fast := head, head
    
    for i := 0; i < n; i++ {
        fast = fast.Next
    }
    if fast == nil {
        return head.Next
    }
    for fast.Next != nil {
        fast = fast.Next
        slow = slow.Next
    }
    slow.Next = slow.Next.Next
    
    return head
}
```

### Принцип 

Перемещаем быстрый указатель на n записей вперед, если он стал равняться nil, значит удалить необходимо первый элемент списка. Иначе начинаем идти быстрым и медленным указателями с равным шагом, пока быстрый не достигнет конца, это будет означать, что следующую за медленным указателем запись необходимо удалить, удаляем и возвращаем начальный указатель.

