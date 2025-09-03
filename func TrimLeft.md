```go
func TrimLeft(s, cutset string) string
```

`TrimLeft` возвращает фрагмент строки `s`, из которого удалены все начальные коды Unicode, содержащиеся в `cutset`.

```go

package main

import (
	"fmt"
	"strings"
)

func main() {
	fmt.Print(strings.TrimLeft("¡¡¡Hello, Gophers!!!", "!¡"))
	//Hello, Gophers!!!!
}
```