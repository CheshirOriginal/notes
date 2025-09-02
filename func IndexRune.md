```go
func IndexRune(s string , r rune ) int
```

`IndexRune` возвращает индекс первого экземпляра кодовой точки `Unicode` `r` или -1, если `rune` отсутствует в `s`. Если `r` равно `utf8.RuneError` , возвращается первый экземпляр любой недопустимой последовательности байтов UTF-8.

```go
package main

import (
	"fmt"
	"strings"
)

func main() {
	fmt.Println(strings.IndexRune("chicken", 'k')) //4
	fmt.Println(strings.IndexRune("chicken", 'd')) //-1
}
```