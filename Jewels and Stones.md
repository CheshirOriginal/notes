https://leetcode.com/explore/learn/card/hash-table/187/conclusion-hash-table/1136
### Условие
Вам даны нити с драгоценностями, представляющие типы камней, которые являются драгоценностями, и нити с камнями, представляющими имеющиеся у вас камни. Каждый символ в словах «камни» соответствует типу имеющегося у вас камня. Вам нужно узнать, сколько из имеющихся у вас камней также являются драгоценностями.

Буквы чувствительны к регистру, поэтому «а» считается другим типом камня, чем «А».
### Пример

**Ввод:** jewels = "aA", stones = "aAAbbbb"
**Выход:** 3
### Решение

```go
func numJewelsInStones(jewels string, stones string) int {
    var (
        set [26*2+7]bool
        // +7, т.к. между большими и маленькими буквами 
        //еще 7 символов
        counter int
    )
    for i := 0; i < len(jewels); i++ {
        idx := int(jewels[i] - 'A')
        set[idx] = true
    }
    for i := 0; i < len(stones); i++ {
        if idx := int(stones[i] - 'A'); set[idx] {
            counter++
        }
    }
    return counter
}
```