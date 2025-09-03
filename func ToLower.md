```go
func ToLower(s string) string
```

`ToLower` возвращает `s` со всеми буквами Unicode, преобразованными в строчные.

```go
package main

import (
	"fmt"
	"strings"
)

func main() {
	fmt.Println(strings.ToLower("Gopher")) //gopher
}
```