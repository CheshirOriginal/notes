
https://leetcode.com/explore/learn/card/array-and-string/201/introduction-to-array/1147
### Условие
Вам предоставляется целочисленный массив `nums` где наибольшее целое число является **уникальным** .

Определите, является ли самый большой элемент в массиве **по крайней мере вдвое** больше любого другого числа в массиве. Если это так, верните _**индекс** самого большого элемента или верните_ `-1` _в противном случае_ .
### Пример

**Ввод:** nums = [3,6,1,0]
**Выход: 1

**Ввод:** nums = [1,2,3,4]
**Выход:** -1
### Решение
```go
func dominantIndex(nums []int) int {
    if len(nums) < 1 || nums == nil {
        return -1
    }
    var max1, max1Idx, max2 = -1, -1, -1
    
    for i, v := range nums {
        if v > max1 {
            max2 = max1
            max1 = v
            max1Idx = i
        } else if v > max2 {
            max2 = v
        }
    }
    
    if max2 * 2 > max1 {
        return -1
    }
    
    return max1Idx
}
```

либо так 

```go
func dominantIndex(nums []int) int {
    if len(nums) < 1 || nums == nil {
        return -1
    }
    var max, maxIdx = -1, -1
    
    for i, v := range nums {
        if v > max {
            max = v
            maxIdx = i
        }
    }
    for _, v := range nums {
        if v * 2 > max && v != max {
            return -1
        }
    }
    
    return maxIdx
}
```