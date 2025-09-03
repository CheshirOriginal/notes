```go
func TrimLeftFunc(s string, f func(rune) bool) string
```

`TrimLeftFunc` возвращает фрагмент строки `s`, из которого удалены все начальные коды Unicode c, удовлетворяющие условию f(c).

```go
package main

import (
	"fmt"
	"strings"
	"unicode"
)

func main() {
	fmt.Print(strings.TrimLeftFunc("¡¡¡Hello, Gophers!!!", func(r rune) bool {
		return !unicode.IsLetter(r) && !unicode.IsNumber(r)
	}))
	//Hello, Gophers!!!
}
```