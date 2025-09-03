```go
func TrimRightFunc(s string, f func(rune) bool) string
```

`TrimRightFunc` возвращает фрагмент строки `s`, из которого удалены все конечные кодовые точки Unicode c, удовлетворяющие условию f(c).

```go
package main

import (
	"fmt"
	"strings"
	"unicode"
)

func main() {
	fmt.Print(strings.TrimRightFunc("¡¡¡Hello, Gophers!!!", func(r rune) bool {
		return !unicode.IsLetter(r) && !unicode.IsNumber(r)
	}))
	//¡¡¡Hello, Gophers
}
```