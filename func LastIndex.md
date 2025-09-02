```go
func LastIndex(s, substr string ) int
```

`LastIndex` возвращает индекс последнего экземпляра `substr` в `s` или -1, если `substr` отсутствует в `s`.

```go
package main

import (
	"fmt"
	"strings"
)

func main() {
	fmt.Println(strings.Index("go gopher", "go")) //0
	fmt.Println(strings.LastIndex("go gopher", "go")) //3
	fmt.Println(strings.LastIndex("go gopher", "rodent")) //-1
}
```