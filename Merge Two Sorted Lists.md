https://leetcode.com/explore/interview/card/top-interview-questions-easy/93/linked-list/771/
### Условие
Вам даны заголовки двух отсортированных связанных списков. `list1` и `list2`.

Объедините два списка в один **отсортированный** . Список должен быть получен путём объединения узлов первых двух списков.

Возвращает _заголовок объединенного связанного списка_ .
### Пример

**Ввод:** list1 = [1,2,4], list2 = [1,3,4]
**Выход:** [1,1,2,3,4,4]

**Ввод:** list1 = [], list2 = []
**Выход:** []
### Решение

```go
func mergeTwoLists(list1 *ListNode, list2 *ListNode) *ListNode {
    if list1 == nil {
        return list2
    }
    if list2 == nil {
        return list1
    }
    
    var resHead, p *ListNode
    
    if list1.Val <= list2.Val {
        resHead = list1
        p = resHead
        list1 = list1.Next
    } else {
        resHead = list2
        p = resHead
        list2 = list2.Next
    }
    
    for list1 != nil && list2 != nil {
        if list1.Val <= list2.Val {
            p.Next = list1
            p = p.Next
            list1 = list1.Next
        } else {
            p.Next = list2
            p = p.Next
            list2 = list2.Next
        }
    }
    
    if list1 != nil {
        p.Next = list1
    } else {
        p.Next = list2
    }
    
    return resHead
}
```
