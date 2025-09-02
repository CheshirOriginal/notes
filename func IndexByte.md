```go
func IndexByte(s string , c byte) int
```

`IndexByte` возвращает индекс первого экземпляра `c` в `s` или -1, если `c` отсутствует в `s`.

```go
package main

import (
	"fmt"
	"strings"
)

func main() {
	fmt.Println(strings.IndexByte("golang", 'g')) //0
	fmt.Println(strings.IndexByte("gophers", 'h')) //3
	fmt.Println(strings.IndexByte("golang", 'x')) //-1
}
```