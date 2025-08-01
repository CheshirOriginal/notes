
https://leetcode.com/explore/interview/card/top-interview-questions-easy/127/strings/880/
### Условие
Дано знаковое 32-битное целое число `x`, возвращаться `x`_с перевернутыми цифрами_ . Если перевернуть `x`приводит к тому, что значение выходит за пределы диапазона 32-битных целых чисел со знаком `[-231, 231 - 1]`, затем вернуться `0`.
### Пример

**Ввод:** x = 123
**Выход:** 321

**Ввод:** x = -123
**Выход:** -321
### Решение

```go
func reverse(x int) int {
	res := 0

    for x != 0 {
        pop := x % 10
        x /= 10
        if (res > math.MaxInt32 / 10) || (res == math.MaxInt32 / 10 && pop > 7) {
            return 0
        }
        if (res < math.MinInt32 / 10) || (res == math.MinInt32 / 10 && pop < -8) {
            return 0
        }
        res = res * 10 + pop
    }
	return res
}
```


