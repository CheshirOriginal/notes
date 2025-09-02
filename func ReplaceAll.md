```go
func ReplaceAll(s, old, new string) string
```

ReplaceAll возвращает копию строки s, в которой все неперекрывающиеся вхождения old заменены на new. Если old пусто, сопоставление выполняется в начале строки и после каждой последовательности UTF-8, что позволяет выполнить до k+1 замен для строки из k рун.

```go
package main

import (
	"fmt"
	"strings"
)

func main() {
	fmt.Println(strings.ReplaceAll("oink oink oink", "oink", "moo"))
	//moo moo moo
}
```