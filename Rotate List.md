https://leetcode.com/explore/learn/card/linked-list/213/conclusion/1225
### Условие
Дан заголовок связного списка. Повернуть список вправо на k позиций.
### Пример

![[Pasted image 20250805185916.png]]
**Ввод:** head = [0,1,2], k = 4
**Выход:** [2,0,1]
### Решение

```go
func rotateRight(head *ListNode, k int) *ListNode {
    if head == nil {
        return head
    }
    c := 1
    tail, res := head, head
    for tail.Next != nil { //определяем длину списка
        c++
        tail = tail.Next
    }
    k %= c // сколько реально надо перестановок, если k > длины
    if k == 0 { // если k кратно длинне, значит ничего менять не надо
        return res
    }
    tail.Next = head //временно зацикливаем
    for i := 1; i < c - k; i++ { // находим новый первый элемент
        head = head.Next
    }
    res = head.Next // новый первый элемент
    head.Next = nil // разрываем цикл
    
    return res
}
```