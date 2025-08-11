https://leetcode.com/explore/learn/card/hash-table/187/conclusion-hash-table/1135/
### Условие
Для строки s найдите длину самой длинной подстроки без повторяющихся символов.
### Пример

**Ввод:** s = "abcabcbb"
**Выход:** 3
### Решение
Скользящее окно с отображением:

```go
func lengthOfLongestSubstring(s string) int {
    if len(s) == 0 {
        return 0
    }
    var(
        max, l int
        duplicate bool
    )
    set := make(map[byte]bool)
    
    for r := 0; r < len(s); r++ {
        if set[s[r]] {
            duplicate = true
        } else {
            set[s[r]] = true
        }
        for duplicate {
            if s[l] == s[r] {
                duplicate = false
            } else {
                set[s[l]] = false
            }
            l++
        }
        if tmp := r - l + 1; tmp > max {
            max = tmp
        }
    }
    return max
}
```

Оптимизация с массивом вместо отображения:

```go
func lengthOfLongestSubstring(s string) int {
    if len(s) == 0 {
        return 0
    }
    var(
        max, l int
        duplicate bool
        set [128]bool
    )
    
    for r := 0; r < len(s); r++ {
        if set[s[r]] {
            duplicate = true
        } else {
            set[s[r]] = true
        }
        for duplicate {
            if s[l] == s[r] {
                duplicate = false
            } else {
                set[s[l]] = false
            }
            l++
        }
        if tmp := r - l + 1; tmp > max {
            max = tmp
        }
    }
    return max
}
```