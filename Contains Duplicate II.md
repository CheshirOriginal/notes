https://leetcode.com/explore/learn/card/hash-table/184/comparison-with-other-data-structures/1121/
### Условие
Учитывая целочисленный массив nums и целое число k, вернуть true, если в массиве есть два различных индекса i и j, таких, что nums[i] == nums[j] и abs(i - j) <= k.
### Пример

**Ввод:** nums = [1,2,3,1], k = 3
**Выход:** true

**Ввод:** nums = [1,0,1,1], k = 1
**Выход:** true
### Решение

```go
func containsNearbyDuplicate(nums []int, k int) bool {
    store := make(map[int]int)
    for i, n := range nums {
        if v, ok := store[n]; ok && abs(v - i) <= k {
            return true
        }
        store[n] = i
    }
    return false
}

func abs(n int) int {
    if n >= 0 {
        return n
    }
    return -1 * n
}
```