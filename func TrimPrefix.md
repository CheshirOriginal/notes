```go
func TrimPrefix(s, prefix string) string
```

`TrimPrefix` возвращает `s` без указанной начальной строки `prefix`. Если `s` не начинается с префикса, `s` возвращается без изменений.

```go
package main

import (
	"fmt"
	"strings"
)

func main() {
	var s = "¡¡¡Hello, Gophers!!!"
	s = strings.TrimPrefix(s, "¡¡¡Hello, ")
	s = strings.TrimPrefix(s, "¡¡¡Howdy, ")
	fmt.Print(s)
	//Gophers!!!
}
```