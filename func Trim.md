```go
func Trim(s, cutset string) string
```

`Trim` возвращает фрагмент строки `s`, из которого удалены все начальные и конечные коды Unicode, содержащиеся в `cutset`.

```go
package main

import (
	"fmt"
	"strings"
)

func main() {
	fmt.Print(strings.Trim("¡¡¡Hello, Gophers!!!", "!¡"))
	//Hello, Gophers
}
```