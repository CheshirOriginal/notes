```go
func TrimSpace(s string) string
```

`TrimSpace` возвращает фрагмент строки `s`, из которого удалены все начальные и конечные пробелы, как определено в Unicode.

```go
package main

import (
	"fmt"
	"strings"
)

func main() {
	fmt.Println(strings.TrimSpace(" \t\n Hello, Gophers \n\t\r\n"))
	//Hello, Gophers
}
```