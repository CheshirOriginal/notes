```go
func TrimSuffix(s, suffix string) string
```

`TrimSuffix` возвращает `s` без указанной конечной строки суффикса. Если `s`не заканчивается суффиксом, `s` возвращается без изменений.

```go
package main

import (
	"fmt"
	"strings"
)

func main() {
	var s = "¡¡¡Hello, Gophers!!!"
	s = strings.TrimSuffix(s, ", Gophers!!!")
	s = strings.TrimSuffix(s, ", Marmots!!!")
	fmt.Print(s)
	//¡¡¡Hello
}
```