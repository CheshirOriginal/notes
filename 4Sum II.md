https://leetcode.com/explore/learn/card/hash-table/187/conclusion-hash-table/1134
### Условие
Даны четыре целочисленных массива nums1, nums2, nums3 и nums4, все длиной n. Верните количество кортежей (i, j, k, l) таких, что:
- 0 <= i, j, k, l < n
- nums1[i] + nums2[j] + nums3[k] + nums4[l] == 0
### Пример

**Ввод:** nums1 = [1,2], nums2 = [-2,-1], nums3 = [-1,2], nums4 = [0,2]
**Выход:** 2
### Решение

```go
func fourSumCount(nums1 []int, nums2 []int, nums3 []int, nums4 []int) int {
    n := len(nums1)
    set := make(map[int]int, n * n)
    counter := 0
    
    for i := 0; i < n; i++ {
        for j := 0; j < n; j++ {
            tmp := nums1[i] + nums2[j]
            set[tmp]++
        }
    }

    for i := 0; i < n; i++ {
        for j := 0; j < n; j++ {
            if tmp := set[0 - nums3[i] - nums4[j]]; tmp > 0 {
                counter += tmp
            }
        }
    }
    return counter
}
```