https://leetcode.com/problems/permutation-in-string/description/
### Условие

Даны две строки `s1`и `s2`, вернуть, `true`если `s2`содержит перестановку `s1`или  `false` в ином случае.

Другими словами, вернуть, `true`если одна из `s1`перестановок является подстрокой `s2`.
### Пример
**Ввод:** s1 = "ab", s2 = "eidbaooo"
**Выход:** true

**Ввод:** s1 = "ab", s2 = "eidboaoo"
**Выход:** false
### Решение
Максимально оптимизированный вариант под условия литкода
```go
// анаграма определеяется путем сравнения массивов с частотой символов
func checkInclusion(s1 string, s2 string) bool {
	n := len(s1)
	freqS1, freqTmp := make([]int, 26), make([]int, 26)

	for i := 0; i < len(s1); i++ {
		freqS1[s1[i]-'a']++
	}
	for i, j := 0, 0; i < len(s2); i++ {
		freqTmp[s2[i]-'a']++
		if i-j >= n {
			freqTmp[s2[j]-'a']--
			j++
		}
		if slices.Equal(freqTmp, freqS1) {
			return true
		}
	}
	return false
}
```

Более универсальный вариант под строки с любыми символами
```go
// анаграма определеяется путем сравнения отсортированных срезов рун
func checkInclusion(s1 string, s2 string) bool {
    str1, str2 := []rune(s2), []rune(s1)
	n := len(str2)

	sort.Slice(str2, func(i, j int) bool {
		return str2[i] < str2[j]
	})

	tmp := make([]rune, n)
	for i := n; i <= len(str1); i++ {
		copy(tmp, str1[i-n:i])
		sort.Slice(tmp, func(i, j int) bool {
			return tmp[i] < tmp[j]
		})
		if slices.Equal(str2, tmp) {
			return true
		}
	}
	return false
}
```