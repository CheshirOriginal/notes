```go
func TrimRight(s, cutset string) string
```

`TrimRight` возвращает фрагмент строки `s`, из которого удалены все конечные кодовые точки Unicode, содержащиеся в `cutset`.

```go
package main

import (
	"fmt"
	"strings"
)

func main() {
	fmt.Print(strings.TrimRight("¡¡¡Hello, Gophers!!!", "!¡"))
	//¡¡¡Hello, Gophers
}
```