
https://leetcode.com/explore/interview/card/top-interview-questions-easy/127/strings/881/
### Условие
Given a string `s`, find the **first** non-repeating character in it and return its index. If it **does not** exist, return `-1`.
### Пример

**Ввод:** s = "leetcode"
**Выход:** 0

**Ввод:** s = "loveleetcode"
**Выход:** 2
### Решение

```go
func firstUniqChar(s string) int {
    var counters [26]int
    
    for i := 0; i < len(s); i++ {
        counters[s[i] - 'a']++
    }
    for i := 0; i < len(s); i++ {
        if counters[s[i] - 'a'] == 1 {
            return i
        }
    }
    return -1
}
```


