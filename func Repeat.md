```go
func Repeat(s string, count int) string
```

Повтор возвращает новую строку, состоящую из `count` копий строки `s`.
Он выдает панику, если `count` отрицателен или если результат (len(s) * count) переполняется.

```go
package main

import (
	"fmt"
	"strings"
)

func main() {
	fmt.Println("ba" + strings.Repeat("na", 2))
}
//banana
```