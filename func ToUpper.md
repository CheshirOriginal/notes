```go
func ToUpper(s string) string
```

`ToUpper` возвращает `s` со всеми буквами Unicode, преобразованными в верхний регистр.

```go
package main

import (
	"fmt"
	"strings"
)

func main() {
	fmt.Println(strings.ToUpper("Gopher")) //GOPHER
}
```