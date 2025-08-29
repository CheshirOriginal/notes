
https://leetcode.com/explore/learn/card/hash-table/187/conclusion-hash-table/1133/
### Условие
Дан массив целых чисел nums и целое число k. Верните k наиболее часто встречающихся элементов. Вы можете возвращать ответ в любом порядке.
### Пример

**Ввод:** nums = [1,1,1,2,2,3], k = 2
**Выход:** [1,2]

**Ввод:** nums = [1], k = 1
**Выход:** [1]
### Решение

```go
func topKFrequent(nums []int, k int) []int {
    res := []int{}
	m := map[int]int{}
	h := &NodeHeap{}

	for _, n := range nums {
		m[n]++
	}
	for v, f := range m {
		heap.Push(h, Node{val: v, frequency: f})
		if h.Len() > k {
			heap.Pop(h)
		}
	}
	for h.Len() > 0 {
		node := heap.Pop(h).(Node)
		res = append(res, node.val)
	}
	return res
}

type NodeHeap []Node

func (h NodeHeap) Len() int           { return len(h) }
func (h NodeHeap) Less(i, j int) bool { return h[i].frequency < h[j].frequency }
func (h NodeHeap) Swap(i, j int)      { h[i], h[j] = h[j], h[i] }

func (h *NodeHeap) Push(x interface{}) {
	*h = append(*h, x.(Node))
}
func (h *NodeHeap) Pop() interface{} {
	n := len(*h)
	res := (*h)[n-1]
	*h = (*h)[:n-1]
	return res
}

type Node struct {
	val       int
	frequency int
}
```

### Принцип 

Сначала с помощью маппы определяется частота появления элементов среза, после чего с помощью минхипы составляется искомый топ k элементов по частоте появления.


