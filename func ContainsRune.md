```go
func ContainsRune(s string, r rune) bool
```

ContainsRune сообщает, находится ли кодовая точка Unicode r в пределах s.

```go
package main

import (
	"fmt"
	"strings"
)

func main() {
	// Определяет, содержит ли строка определенную кодовую точку Unicode.
	// Например, код строчной буквы «а» — 97.
	fmt.Println(strings.ContainsRune("aardvark", 97)) //true
	fmt.Println(strings.ContainsRune("timeout", 97)) //false
}
```