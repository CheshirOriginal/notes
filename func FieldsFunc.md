```go
func FieldsFunc(s string, f func( rune ) bool) []string
```

Функция `FieldsFunc` разбивает строку `s` на части, соответствующие кодовым точкам Unicode c, удовлетворяющим условию f(c), и возвращает массив срезов строки `s`. Если все кодовые точки в `s` удовлетворяют условию f(c) или строка пуста, возвращается пустой срез. Каждый элемент возвращаемого среза непустой. В отличие от `SplitFunc`, начальные и конечные срезы кодовых точек, удовлетворяющих условию f(c), отбрасываются.

`FieldsFunc` не дает никаких гарантий относительно порядка вызова f(c) и предполагает, что f всегда возвращает одно и то же значение для заданного c.

```go
package main

import (
	"fmt"
	"strings"
	"unicode"
)

func main() {
	f := func(c rune) bool {
		return !unicode.IsLetter(c) && !unicode.IsNumber(c)
	}
	fmt.Printf("Fields are: %q",
	strings.FieldsFunc("foo1;bar2,baz3...", f))
	// ["foo1" "bar2" "baz3"]
}
```
