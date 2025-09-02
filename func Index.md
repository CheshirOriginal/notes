```go
func Index(s, substr string) int
```

`Index` возвращает индекс первого экземпляра `substr` в `s` или -1, если `substr` отсутствует в `s`.

```go
package main

import (
	"fmt"
	"strings"
)

func main() {
	fmt.Println(strings.Index("chicken", "ken")) //4
	fmt.Println(strings.Index("chicken", "dmr")) //-1
}
```