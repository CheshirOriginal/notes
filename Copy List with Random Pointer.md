https://leetcode.com/explore/learn/card/linked-list/213/conclusion/1229/
### Условие
Задан связанный список длины n таким образом, что каждый узел содержит дополнительный случайный указатель, который может указывать на любой узел в списке или на null.

Создайте полную копию списка. Глубокая копия должна состоять ровно из n совершенно новых узлов, где каждому новому узлу присвоено значение, равное значению соответствующего исходного узла. Как следующий, так и случайный указатель новых узлов должны указывать на новые узлы в скопированном списке таким образом, чтобы указатели в исходном и скопированном списках представляли одно и то же состояние списка. Ни один из указателей в новом списке не должен указывать на узлы исходного списка.

Например, если в исходном списке есть два узла X и Y, где X.random --> Y, то для соответствующих двух узлов x и y в скопированном списке x.random --> y.

Верните заголовок скопированного связанного списка.

Связанный список представлен на входе/выходе как список из n узлов. Каждый узел представлен парой [val, random_index], где:

- val: целое число, представляющее Node.val
- random_index: индекс узла (диапазон от 0 до n-1), на который указывает случайный указатель, или null, если он не указывает ни на один узел.

Вашему коду будет присвоен только заголовок исходного связанного списка.
### Пример

![[Pasted image 20250806131456.png]]
**Ввод:** head = \[[7,null],[13,0],[11,4],[10,2],[1,0]]
**Выход:** \[[7,null],[13,0],[11,4],[10,2],[1,0]]
### Решение
С использованием хэш-таблицы:
```go
func copyRandomList(head *Node) *Node {
    if head ==  nil {
        return head
    }
    store := make(map[*Node]*Node)
    res := &Node{Val: head.Val}
    p := res
    store[head] = res
    for head != nil {
        if tmp := store[head.Next]; tmp != nil {
            p.Next = tmp
        } else if head.Next != nil {
            p.Next = &Node{Val: head.Next.Val}
            store[head.Next] = p.Next
        }
        if head.Random == nil {
            p.Random = nil
        } else if tmp := store[head.Random]; tmp != nil {
            p.Random = tmp
        } else {
            p.Random = &Node{Val: head.Random.Val}
            store[head.Random] = p.Random
        }
        head = head.Next
        p = p.Next
    }
    return res
}
```

Проходимся по исходному списку и создаем копию каждого элемента, если копия этого элемента уже есть, просто устанавливаем связь. 

Решение без дополнительной памяти:
```go
func copyRandomList(head *Node) *Node {
    if head ==  nil {
        return head
    }
    // вставляем после каждого элемента входного списка его копию
    p := head
    for p != nil {
        next := p.Next
        p.Next = &Node{Val: p.Val}
        p.Next.Next = next
        p = p.Next.Next
    }
    //устанавливаем для всех копий соответствующие значения для поля
    //Random
    p = head
    for p != nil {
        var tmp *Node
        if p.Random == nil {
            tmp = nil
        } else {
            tmp = p.Random.Next
        }
        p.Next.Random = tmp
        p = p.Next.Next
    }
    //разделяем два списка: изначальный и копию
    res := head.Next
    p = res
    for p.Next != nil {
        next := p.Next
        //
        p.Next = p.Next.Next
        p = p.Next
        //
        head.Next = next
        head = head.Next
    }
    head.Next = nil
    return res
}
```