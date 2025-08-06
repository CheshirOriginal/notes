https://leetcode.com/explore/learn/card/hash-table/183/combination-with-other-algorithms/1105/
### Условие
Даны два целочисленных массива nums1 и nums2. Верните массив их пересечения. Каждый элемент в результате должен быть уникальным, и результаты можно возвращать в любом порядке.
### Пример

**Ввод:** nums1 = [1,2,2,1], nums2 = [2,2]
**Выход:** [2]
### Решение

```go
func intersection(nums1 []int, nums2 []int) []int {
    set := make(map[int]bool)
    res := []int{}
    
    for _, v := range nums1 {
        set[v] = true
    }
    for _, v := range nums2 {
        if set[v] {
            res = append(res, v)
            delete(set, v)
        }
    }
    return res
}
```