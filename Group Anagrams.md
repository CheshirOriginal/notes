https://leetcode.com/explore/learn/card/hash-table/185/hash_table_design_the_key/1124
### Условие
Дан массив строк strs. Сгруппируйте анаграммы. Вы можете вернуть ответ в любом порядке.
### Пример

**Ввод:** strs = ["eat","tea","tan","ate","nat","bat"]
**Выход:** \[["bat"],["nat","tan"],["ate","eat","tea"]]
### Решение

```go
func groupAnagrams(strs []string) [][]string {
    store := make(map[string][]string)
    res := [][]string{}
    
    for _, str := range strs {
        ctr := make([]int, 26)
        for _, char := range str {
            ctr[char - 'a']++
        }
        tmp := []byte{}
        for i, c := range ctr {
            if c > 0 {
                tmp = append(tmp, byte(i + 'a'))
                tmp = append(tmp, byte(c + '0'))
            }
        }
        store[string(tmp)] = append(store[string(tmp)], str)
    }
    for _, v := range store {
        res = append(res, v)
    }
    return res
}
```