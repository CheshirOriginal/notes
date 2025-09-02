```go
func LastIndexByte(s string, c byte) int
```

`LastIndexByte` возвращает индекс последнего экземпляра `c` в `s` или -1, если `c` отсутствует в `s`.

```go
package main

import (
	"fmt"
	"strings"
)

func main() {
	fmt.Println(strings.LastIndexByte("Hello, world", 'l')) //10
	fmt.Println(strings.LastIndexByte("Hello, world", 'o')) //8
	fmt.Println(strings.LastIndexByte("Hello, world", 'x')) //-1
}
```