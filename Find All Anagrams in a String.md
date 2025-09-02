
https://leetcode.com/problems/find-all-anagrams-in-a-string/description/
### Условие
Даны две строки s и p. Вернуть массив всех начальных индексов строк p в строке s. Вы можете возвращать ответ в любом порядке.
### Пример

**Ввод:** s = "cbaebabacd", p = "abc"
**Выход:** [0,6]

**Ввод:** s = "abab", p = "ab"
**Выход:** [0,1,2]
### Решение

```go
func findAnagrams(s string, p string) []int {  
    if len(s) < len(p) {  
       return nil  
    }  
    currentFrequency, frequencyInP := make([]int, 26), make([]int, 26)  
    res := []int{}  
  
    for i := 0; i < len(p); i++ {  
       frequencyInP[p[i]-'a']++  
    }  
    window := len(p)  
    for i := 0; i < window; i++ {  
       currentFrequency[s[i]-'a']++  
    }  
    if slices.Equal(frequencyInP, currentFrequency) {  
       res = append(res, 0)  
    }  
    for i := window; i < len(s); i++ {  
       currentFrequency[s[i]-'a']++  
       currentFrequency[s[i-window]-'a']--  
       if slices.Equal(frequencyInP, currentFrequency) {  
          res = append(res, i-window+1)  
       }  
    }  
    return res  
}
```

### Принцип 

Создаем массив, хранящий частоту символов в строке p и такой же массив, который будет хранить частоту символов в подстроке строки s. Далее проходимся скользящим окном фиксированной длинны (длинна строки p) по строке s и сравниваем частоту символов в подстроке с частотой символов в строке p если массивы идентичны, то анаграмм найден, записываем его начальный индекс в срез ответа. 

Примечание: сравнение срезов фиксированной длины это константная операция, поэтому общая сложность алгоритма O(n) по времени и O(1) по пространству (все структуры данных фиксированной длинны)


