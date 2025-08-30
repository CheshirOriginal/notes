```go
func CutSuffix(s, suffix string) (before string, found bool)
```

`CutSuffix` возвращает строку `s` без указанной строки суффикса и сообщает, найден ли суффикс. Если строка `s` не заканчивается суффиксом, `CutSuffix` возвращает `s`, `false`. Если суффикс — пустая строка, `CutSuffix` возвращает `s`,`true`.

```go
package main

import (
	"fmt"
	"strings"
)

func main() {
	show := func(s, suffix string) {
		before, found := strings.CutSuffix(s, suffix)
		fmt.Printf("CutSuffix(%q, %q) = %q, %v\n", s, suffix, before, found)
	}
	show("Gopher", "Go") //"Gopher", false
	show("Gopher", "er") //"Goph", true
}
```