https://leetcode.com/explore/learn/card/linked-list/213/conclusion/1228/
### Условие
Вам даны два непустых связанных списка, представляющих два неотрицательных целых числа. Цифры хранятся в обратном порядке, и каждый из их узлов содержит одну цифру. Сложите два числа и верните сумму в виде связанного списка.

Вы можете предположить, что оба числа не содержат начальных нулей, за исключением самого числа 0.
### Пример

**Ввод:** l1 = [2,4,3], l2 = [5,6,4]
**Выход:** [7,0,8]

**Ввод:** head = l1 = [9,9,9,9,9,9,9], l2 = [9,9,9,9]
**Выход:** [8,9,9,9,0,0,0,1]
### Решение
Мой вариант с использованием уже выделенной памяти:

```go
func addTwoNumbers(l1 *ListNode, l2 *ListNode) *ListNode {
    var res, prev *ListNode = l1, nil
    add := 0
    for l1 != nil && l2 != nil {
        tmp := l1.Val + l2.Val + add
        add = tmp / 10
        l1.Val = tmp % 10
        prev = l1
        l1 = l1.Next
        l2 = l2.Next
    }
    if l2 != nil {
        prev.Next = l2
    }
    for add > 0 {
        if prev.Next == nil {
            prev.Next = &ListNode{}
        }
        tmp := prev.Next.Val + add
        add = tmp / 10
        prev.Next.Val = tmp % 10
        prev = prev.Next
    }
    return res
}
```

Вариант литкода с выделением новой памяти

```go
func addTwoNumbers(l1 *ListNode, l2 *ListNode) *ListNode {
    dummyHead := &ListNode{Val: 0}
    curr := dummyHead
    carry := 0
    for l1 != nil || l2 != nil || carry != 0 {
        x := 0
        if l1 != nil {
            x = l1.Val
        }
        y := 0
        if l2 != nil {
            y = l2.Val
        }
        sum := carry + x + y
        carry = sum / 10
        curr.Next = &ListNode{Val: sum % 10}
        curr = curr.Next
        if l1 != nil {
            l1 = l1.Next
        }
        if l2 != nil {
            l2 = l2.Next
        }
    }
    return dummyHead.Next
}
```