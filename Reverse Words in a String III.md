
https://leetcode.com/explore/learn/card/array-and-string/204/conclusion/1165
### Условие
Дана строка `s`, меняют порядок символов в каждом слове предложения на обратный, сохраняя при этом пробелы и начальный порядок слов.
### Пример

**Ввод:** s = "Let's take LeetCode contest"
**Выход:** "s'teL ekat edoCteeL tsetnoc"
### Решение

```go
func reverseWords(s string) string {
    if len(s) <= 1 {
        return s
    }
    
    l := -1
    res := []byte(s)
    
    for i := 0; i < len(res); i++ {
		if res[i] != ' ' && l == -1 {
			l = i
			continue
		}
		if res[i] == ' ' {
			reverse(res, l, i-1)
			l = -1
		}
	}

	if l != -1 {
		reverse(res, l, len(res)-1)
	}
    
    return string(res)
}


func reverse(s []byte, l, r int) {
    for l <= r {
        s[l], s[r] = s[r], s[l]
        l++
        r--
    }
}
```

### Принцип 

возможно стоит доработать


