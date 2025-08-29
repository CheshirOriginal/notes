Пакет **heap** обеспечивает операции кучи (heap) для любого типа, который реализует heap.Interface. Куча - это дерево со свойством того, что каждый узел является узлом с минимальным значением в своем поддереве.
#### Тип Interface

```go
type Interface interface {
    sort.Interface
    Push(x interface{}) // добавить x как элемент Len()
    Pop() interface{}   // удалить и вернуть элемент Len() - 1.
}
```

==Тип Interface описывает требования к типу с использованием подпрограмм в этом пакете.== Любой тип, который реализует это, может использоваться в качестве min-кучи со следующими инвариантами (устанавливаемыми после вызова Init или если данные пусты или отсортированы).
#### Функция Fix

```go
func Fix(h Interface, i int)
```

Fix восстанавливает порядок кучи после того, как элемент с индексом i изменил свое значение. Изменение значения элемента по индексу i с последующим вызовом Fix эквивалентно, но менее затратно, чем вызов Remove(h, i), за которым следует Push нового значения. Сложность O(log n), где n = h.Len().
==т.е. можно изменить значение элемента в куче, после чего вызвать метод Fix с индексом измененного элемента и "нормализовать" кучу.==
#### Функция Init

```go
func Init(h Interface)
```

Init устанавливает инварианты кучи, необходимые для других процедур в этом пакете. Init является идемпотентом по отношению к инвариантам кучи и может вызываться всякий раз, когда инварианты кучи могли быть признаны недействительными. Сложность O(n), где n = h.Len().
==т.е. данный метод выстраивает структуру кучи (по подобию бинарного дерева), чтобы другие методы правильно работали.==
#### Функция Pop

```go
func Pop(h Interface) interface{}
```

Pop удаляет и возвращает минимальный элемент (согласно Less) из кучи. Сложность O(log n), где n = h.Len(). Pop эквивалентен Remove(h, 0).

#### Функция Push

```go
func Push(h Interface, x interface{})
```

Push заталкивает элемент х на кучу. Сложность O(log n), где n = h.Len().

#### Функция Remove

```go
func Remove(h Interface, i int) interface{}
```

Remove удаляет и возвращает элемент с индексом i из кучи. Сложность O(log n), где n = h.Len().
#### Пример целочисленной кучи

Этот пример вставляет несколько int в IntHeap, проверяет минимум и удаляет их в порядке приоритета.

```go
// Этот пример демонстрирует целочисленную кучу, 
// построенную с использованием heap.Interface.
package main

import (
    "container/heap"
    "fmt"
)

// IntHeap это минимальная куча целых чисел.
type IntHeap []int

// Len, Less, Swap для реализации интерфейса sort.Interface
func (h IntHeap) Len() int           { return len(h) }
func (h IntHeap) Less(i, j int) bool { return h[i] < h[j] }
func (h IntHeap) Swap(i, j int)      { h[i], h[j] = h[j], h[i] }

func (h *IntHeap) Push(x interface{}) {
    // Push и Pop используют приемники указателей, 
    // потому что они изменяют длину среза,
    // не только его содержимое.
    *h = append(*h, x.(int))
}

func (h *IntHeap) Pop() interface{} {
    old := *h
    n := len(old)
    x := old[n-1]
    *h = old[0 : n-1]
    return x
}

// Этот пример вставляет несколько int в IntHeap, 
// проверяет минимум,
// и удаляет их в порядке приоритета.
func main() {
    h := &IntHeap{2, 1, 5}
    heap.Init(h)
    heap.Push(h, 3)
    fmt.Printf("minimum: %d\n", (*h)[0])
    for h.Len() > 0 {
        fmt.Printf("%d ", heap.Pop(h))
    }
}
```

Вывод:

```
minimum: 1
1 2 3 5 
```

#### Пример создания приоритетной очереди

В этом примере создается PriorityQueue с некоторыми элементами, добавляется и обрабатывается элемент, а затем удаляются элементы в порядке приоритета.

```go
// Этот пример демонстрирует приоритетную очередь, 
// построенную с использованием интерфейса heap.
package main

import (
    "container/heap"
    "fmt"
)

// Item - это то, чем мы управляем в приоритетной очереди.
type Item struct {
    value    string // Значение элемента; произвольное.
    priority int    // Приоритет элемента в очереди.
    // Индекс необходим для обновления 
    // и поддерживается методами heap.Interface.
    index int // Индекс элемента в куче.
}

// PriorityQueue реализует heap.Interface и содержит Items.
type PriorityQueue []*Item

func (pq PriorityQueue) Len() int { return len(pq) }

func (pq PriorityQueue) Less(i, j int) bool {
    // Мы хотим, чтобы Pop давал нам самый высокий, 
    // а не самый низкий приоритет, 
    // поэтому здесь мы используем оператор больше.
    return pq[i].priority > pq[j].priority
}

func (pq PriorityQueue) Swap(i, j int) {
    pq[i], pq[j] = pq[j], pq[i]
    pq[i].index = i
    pq[j].index = j
}

func (pq *PriorityQueue) Push(x interface{}) {
    n := len(*pq)
    item := x.(*Item)
    item.index = n
    *pq = append(*pq, item)
}

func (pq *PriorityQueue) Pop() interface{} {
    old := *pq
    n := len(old)
    item := old[n-1]
    old[n-1] = nil  // избежать утечки памяти
    item.index = -1 // для безопасности
    *pq = old[0 : n-1]
    return item
}

// update изменяет приоритет и значение Item в очереди.
func (pq *PriorityQueue) update(item *Item, value string, priority int) {
    item.value = value
    item.priority = priority
    heap.Fix(pq, item.index)
}

// Этот пример создает PriorityQueue с некоторыми элементами, 
// добавляет и управляет элементом,
// а затем удаляет элементы в порядке приоритета.
func main() {
    // Некоторые элементы и их приоритеты.
    items := map[string]int{
        "banana": 3, "apple": 2, "pear": 4,
    }

    // Создаем очередь с приоритетами, 
    // помещаем в нее элементы и
    // устанавливаем приоритетные инварианты очереди (кучи).
    pq := make(PriorityQueue, len(items))
    i := 0
    for value, priority := range items {
        pq[i] = &Item{
            value:    value,
            priority: priority,
            index:    i,
        }
        i++
    }
    heap.Init(&pq)

    // Вставить новый элемент, 
    // а затем изменить его приоритет.
    item := &Item{
        value:    "orange",
        priority: 1,
    }
    heap.Push(&pq, item)
    pq.update(item, item.value, 5)

    // Вынимаем предметы; 
    // они прибывают в порядке убывания приоритета.
    for pq.Len() > 0 {
        item := heap.Pop(&pq).(*Item)
        fmt.Printf("%.2d:%s ", item.priority, item.value)
    }
}
```

Вывод:

```
05:orange 04:pear 03:banana 02:apple 
```