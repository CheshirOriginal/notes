```go
func Contains(s, substr string) bool
```

Содержит отчет, находится ли подстрока `substr` в строке `s`.

```go
package main

import (
	"fmt"
	"strings"
)

func main() {
	fmt.Println(strings.Contains("seafood", "foo")) //true
	fmt.Println(strings.Contains("seafood", "bar")) //false
	fmt.Println(strings.Contains("seafood", "")) //true
	fmt.Println(strings.Contains("", "")) //true
}
```