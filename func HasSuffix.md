```go
func HasSuffix(s, suffix string) bool
```

`HasSuffix` сообщает, заканчивается ли строка `s` суффиксом.

```go
package main

import (
	"fmt"
	"strings"
)

func main() {
	fmt.Println(strings.HasSuffix("Amigo", "go")) //true
	fmt.Println(strings.HasSuffix("Amigo", "O")) //false
	fmt.Println(strings.HasSuffix("Amigo", "Ami")) //false
	fmt.Println(strings.HasSuffix("Amigo", "")) //true
}
```