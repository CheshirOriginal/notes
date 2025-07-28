
https://leetcode.com/explore/learn/card/array-and-string/205/array-two-pointer-technique/1299
### Условие
Дан массив положительных целых чисел `nums`и положительное целое число `target`, возвращает _минимальную **длину** подмассива_ _,_ _сумма которого больше или равна_ `target`. Если такого подмассива нет, вернуть `0` вместо.
### Пример

**Ввод:** target = 7, nums = [2,3,1,2,4,3]
**Выход:** 2

**Ввод:** target = 11, nums = [1,1,1,1,1,1,1,1]
**Выход:** 0
### Решение

```go
func minSubArrayLen(target int, nums []int) int {
    var (
		l, sum int
		minSize int = math.MaxInt32
	)
    for r := 0; r < len(nums); r++ {
        sum += nums[r]
        for sum >= target {
            minSize = min(minSize, r - l + 1)
            sum -= nums[l]
            l++
        }
    }
    
    if minSize == math.MaxInt32 {
        return 0
    }
	return minSize
}
```

### Принцип 

[[Скользящее окно]]



