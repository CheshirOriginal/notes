https://leetcode.com/explore/learn/card/hash-table/184/comparison-with-other-data-structures/1177/
### Условие
Даны два массива строк list1 и list2, найдите общие строки с наименьшей суммой индексов.

Общая строка — это строка, которая встречается как в списке list1, так и в списке list2.

Общая строка с наименьшей суммой индексов — это общая строка, такая что если она появляется в list1[i] и list2[j], то i + j должно быть минимальным значением среди всех других общих строк.

Верните все общие строки с наименьшей суммой индексов. Верните ответ в любом порядке.
### Пример

**Ввод:** list1 = ["happy","sad","good"], list2 = ["sad","happy","good"]
**Выход:** ["sad","happy"]
### Решение

```go
func findRestaurant(list1 []string, list2 []string) []string {
    store := make(map[string]int)
    res := []string{}
    
    for i, v := range list1 {
        store[v] = i
    }
    min := math.MaxInt
    for i, v := range list2 {
        if _, ok := store[v]; ok {
            store[v] +=  i
            if tmp := store[v]; tmp < min {
                min = tmp
                res = []string{}
                res = append(res, v)
            } else if tmp == min {
                res = append(res, v)
            }
        }
    }
    return res
}
```