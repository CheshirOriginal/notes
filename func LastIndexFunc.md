```go
func LastIndexFunc(s string, f func(rune) bool) int
```

`LastIndexFunc` возвращает индекс в `s` последней кодовой точки Unicode, удовлетворяющей условию `f(c)`, или -1, если ни одно из них не удовлетворяет.

```go
package main

import (
	"fmt"
	"strings"
	"unicode"
)

func main() {
	fmt.Println(strings.LastIndexFunc("go 123", unicode.IsNumber)) //5
	fmt.Println(strings.LastIndexFunc("123 go", unicode.IsNumber)) //2
	fmt.Println(strings.LastIndexFunc("go", unicode.IsNumber)) //-1
}
```