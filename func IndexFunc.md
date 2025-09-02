```go
func IndexFunc(s string, f func(rune) bool) int
```

`IndexFunc` возвращает индекс в `s` первой кодовой точки `Unicode`, удовлетворяющей условию `f(c)`, или -1, если ни одно из них не удовлетворяет условию.

```go
package main

import (
	"fmt"
	"strings"
	"unicode"
)

func main() {
	f := func(c rune) bool {
		return unicode.Is(unicode.Han, c)
	}
	fmt.Println(strings.IndexFunc("Hello, 世界", f)) //7
	fmt.Println(strings.IndexFunc("Hello, world", f)) //-1
}
```