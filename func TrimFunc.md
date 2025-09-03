```go
func TrimFunc(s string, f func(rune) bool) string
```

`TrimFunc` возвращает фрагмент строки `s`, из которого удалены все начальные и конечные коды Unicode c, удовлетворяющие условию f(c).

```go
package main

import (
	"fmt"
	"strings"
	"unicode"
)

func main() {
	fmt.Print(strings.TrimFunc("¡¡¡Hello, Gophers!!!", func(r rune) bool {
		return !unicode.IsLetter(r) && !unicode.IsNumber(r)
	}))
	//Hello, Gophers
}
```