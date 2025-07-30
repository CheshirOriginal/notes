
https://leetcode.com/explore/interview/card/top-interview-questions-easy/127/strings/882
### Условие
Даны две строки `s` и `t`, возвращаться `true` если `t`является анаграммой ​ `s`, и `false` в противном случае.
### Пример

**Ввод:** s = "anagram", t = "nagaram"
**Выход:** true

**Ввод:** s = "rat", t = "car"
**Выход:** false
### Решение

```go
func isAnagram(s string, t string) bool {
    if len(s) != len(t) {
        return false
    }
    var counters [26]int
    
    for i := 0; i < len(s); i++ {
        counters[s[i] - 'a']++
        counters[t[i] - 'a']--
    }
    for i := 0; i < 26; i++ {
        if counters[i] != 0 {
            return false
        }
    }
    
    return true
}
```


