### Конвертация целых знаковых чисел в строки

При конвертации чисел в строки очень удобно использовать пакет `strconv`, он обладает методом `Itoa`, превращающим числовое значение (int) переменной в строковое (string). Рассмотрим на примере:

```go
package main

import (
	"fmt"
	"strconv"
)

func main() {
	a := strconv.Itoa(2020) // int -> string
	fmt.Printf("%T \n", a) // тип - string
	fmt.Println(a) // 2020
}
```

**Интересное дополнение**, метод `Itoa` это всего-лишь обертка для `FormatInt`: (кусок исходного кода пакета `strconv`)

```go
// Itoa is equivalent to FormatInt(int64(i), 10).
func Itoa(i int) string {
	return FormatInt(int64(i), 10)
}
```

То-есть вызывая метод `Itoa` мы по сути вызываем `FormatInt` который принимает систему счисления в качестве 2 аргумента, но туда сразу передается - десятичная система счисления.

Но никто нам не мешает напрямую вызывать`FormatInt`, полезно если работаем с разными системами счисления:

```go
package main

import (
	"fmt"
	"strconv"
)

func main() {
	// приставка '0x' означает что число в шестнадцатеричной системе счисления
	var a int64 = 0xB // 'B' в шестнадцатеричной это 11 в десятичной системе
	fmt.Println(strconv.FormatInt(a, 10)) // 11
	fmt.Println(strconv.FormatInt(a, 16)) // b
}
```

### Конвертация целых беззнаковых чисел в строку

По аналогии с `FormatInt` есть такой метод как `FormatUint`, пример:

```go
var a uint64 = 10101
res := strconv.FormatUint(a, 10)
fmt.Println(res) // 10101
```

### Конвертация чисел с плавающей точкой в строку 

Для этого есть функция `FormatFloat`:

```go
package main

import (
	"fmt"
	"strconv"
)

func main() {
	var a float64 = 1.0123456789

	// 1 параметр - число для конвертации
	// fmt - форматирование
	// prec - точность (кол-во знаков после запятой)
	// bitSize - 32 или 64 (32 для float32, 64 для float64)
	fmt.Println(strconv.FormatFloat(a, 'f', 2, 64)) // 1.01

	// если мы хотим учесть все цифры после запятой, то можем в prec передать -1
	fmt.Println(strconv.FormatFloat(a, 'f', -1, 64)) // 1.0123456789

	// Возможные форматы fmt:
	// 'f' (-ddd.dddd, no exponent),
	// 'b' (-ddddp±ddd, a binary exponent),
	// 'e' (-d.dddde±dd, a decimal exponent),
	// 'E' (-d.ddddE±dd, a decimal exponent),
	// 'g' ('e' for large exponents, 'f' otherwise),
	// 'G' ('E' for large exponents, 'f' otherwise),
	// 'x' (-0xd.ddddp±ddd, a hexadecimal fraction and binary exponent), or
	// 'X' (-0Xd.ddddP±ddd, a hexadecimal fraction and binary exponent).
	var b float64 = 2222 * 1023 * 245 * 2 * 52
	fmt.Println(strconv.FormatFloat(b, 'e', -1, 64)) // 5.791874088e+10
}
```

### Конвертация bool в string

Тут все просто:

```go
var a = true
res := strconv.FormatBool(a)
fmt.Println(res)     	// true
fmt.Printf("%T", res)   // string
```