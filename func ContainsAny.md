```go
func ContainsAny(s, chars string) bool
```

ContainsAny сообщает, находятся ли какие-либо кодовые точки Unicode в символах в пределах s. (Есть ли хотя бы один из символов строки `chars` в строке `s`)

```go
package main

import (
	"fmt"
	"strings"
)

func main() {
	fmt.Println(strings.ContainsAny("team", "i")) //false
	fmt.Println(strings.ContainsAny("fail", "ui")) //true
	fmt.Println(strings.ContainsAny("ure", "ui")) //true
	fmt.Println(strings.ContainsAny("failure", "ui")) //true
	fmt.Println(strings.ContainsAny("foo", "")) //false
	fmt.Println(strings.ContainsAny("", "")) //false
}
```