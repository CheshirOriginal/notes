https://leetcode.com/problems/summary-ranges/description/

### Условие
Вам дан отсортированный **уникальный** целочисленный массив `nums`.

Диапазон **​** `[a,b]` это множество всех целых чисел от `a` до `b` (включительно).

Верните наименьший отсортированный список диапазонов, которые покрывают все числа в массиве. То есть, каждый элемент `nums` охватывается ровно одним из диапазонов, и не существует целого числа `x` такой что `x` находится в одном из диапазонов, но не в `nums`.

Каждый диапазон `[a,b]` в списке должно быть выведено как:

- `"a->b"` если `a != b`
- `"a"` если `a == b`
### Пример

Ввод: числа = [0,1,2,4,5,7]
Вывод:  ["0->2","4->5","7"]
### Решение

```go
func summaryRanges(nums []int) []string {  
    if len(nums) == 0 {  
       return []string{}  
    }  
    a, b := nums[0], nums[0]  
    var result []string  
    for i := 1; i < len(nums); i++ {  
       if nums[i]-b == 1 {  
          b = nums[i]  
       } else {  
          if a != b {  
             result = append(result, fmt.Sprintf("%d->%d", a, b))  
          } else {  
             result = append(result, fmt.Sprintf("%d", a))  
          }  
          a = nums[i]  
          b = nums[i]  
       }  
    }  
    if a != b {  
       result = append(result, fmt.Sprintf("%d->%d", a, b))  
    } else {  
       result = append(result, fmt.Sprintf("%d", a))  
    }  
    return result  
}
```