```go
func CutPrefix(s, prefix string) (after string, found bool)
```

`CutPrefix` возвращает `s` без указанной начальной строки префикса и сообщает, найден ли префикс. Если `s` не начинается с префикса, `CutPrefix` возвращает `s`, `false`. Если префикс — пустая строка, `CutPrefix` возвращает `s`, `true`.

```go
package main

import (
	"fmt"
	"strings"
)

func main() {
	show := func(s, prefix string) {
		after, found := strings.CutPrefix(s, prefix)
		fmt.Printf("CutPrefix(%q, %q) = %q, %v\n", s, prefix, after, found)
	}
	show("Gopher", "Go") //"pher", true
	show("Gopher", "ph") //"Gopher", false
}
```