```go
func Fields(s string) []string
```

Функция `Fields` разбивает строку `s` по каждому вхождению одного или нескольких последовательных пробелов, как определено в [unicode.IsSpace](https://pkg.go.dev/unicode#IsSpace) , возвращая фрагмент подстрок `s` или пустой фрагмент, если `s` содержит только пробелы. Каждый элемент возвращаемого фрагмента непустой. В отличие от `Split` , начальные и конечные фрагменты пробелов отбрасываются.

```go
package main

import (
	"fmt"
	"strings"
)

func main() {
	fmt.Printf("Fields are: %q", strings.Fields("  foo bar  baz   "))
	// ["foo" "bar" "baz"]
}
```
