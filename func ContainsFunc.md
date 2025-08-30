```go
func ContainsFunc(s string, f func(rune) bool) bool
```

Функция ContainsFunc сообщает, удовлетворяют ли какие-либо кодовые точки Unicode r в пределах s условию f(r).

```go
package main

import (
	"fmt"
	"strings"
)

func main() {
	f := func(r rune) bool {
		return r == 'a' || r == 'e' || r == 'i' || r == 'o' || r == 'u'
	}
	fmt.Println(strings.ContainsFunc("hello", f)) //true
	fmt.Println(strings.ContainsFunc("rhythms", f)) //false
}
```
