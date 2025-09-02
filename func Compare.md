```go
func Compare(a, b string) int
```

Функция Compare возвращает целое число, сравнивая две строки лексикографически. Результат будет равен 0, если a == b, -1, если a < b, и +1, если a > b.

Используйте Compare, когда вам нужно выполнить трёхфакторное сравнение. Обычно нагляднее и всегда быстрее использовать встроенные операторы сравнения строк \==, <, > и т. д.

```go
package main

import (
	"fmt"
	"strings"
)

func main() {
	fmt.Println(strings.Compare("a", "b")) //-1
	fmt.Println(strings.Compare("a", "a")) //0
	fmt.Println(strings.Compare("b", "a")) //1
}
```