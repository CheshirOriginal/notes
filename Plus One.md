
https://leetcode.com/explore/learn/card/array-and-string/201/introduction-to-array/1148/
### Условие
Вам дано **большое целое число,** представленное в виде массива целых чисел. `digits`, где каждый `digits[i]`это `ith`Цифра целого числа. Цифры упорядочены слева направо от старшей к младшей. Большое целое число не содержит начальных цифр. `0`'s.

Увеличьте большое целое число на единицу и верните _полученный массив цифр_ .
### Пример

**Ввод:** digits = [1,2,3]
**Выход:** [1,2,4]

**Ввод:** digits = [9]
**Выход:** [1,0]
### Решение

```go
func plusOne(digits []int) []int {
    if len(digits) == 0 {
		return digits
	}

	for i := len(digits) - 1; i >= 0; i-- {
		if digits[i] != 9 {
			digits[i]++
			return digits
		}
		digits[i] = 0
	}

	digits = make([]int, len(digits)+1)
	digits[0] = 1

	return digits
}
```


