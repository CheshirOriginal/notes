```go
func Replace(s, old, new string, n int) string
```

Функция `Replace` возвращает копию строки `s`, в которой первые `n` неперекрывающихся вхождений `old` заменены на `new`. Если `old` пусто, сопоставление выполняется в начале строки и после каждой последовательности UTF-8, что позволяет выполнить до k+1 замен для строки из k рун. Если n < 0, количество замен не ограничено.

```go
package main

import (
	"fmt"
	"strings"
)

func main() {
	fmt.Println(strings.Replace("oink oink oink", "k", "ky", 2))
	//oinky oinky oink
	fmt.Println(strings.Replace("oink oink oink", "oink", "moo", -1))
	//moo moo moo
}
```