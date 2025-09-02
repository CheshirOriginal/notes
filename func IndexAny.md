```go
func IndexAny(s, chars string) int
```

`IndexAny` возвращает индекс первого экземпляра любой кодовой точки `Unicode` из символов в `s` или -1, если в `s` нет ни одной кодовой точки `Unicode` из символов.

```go
package main

import (
	"fmt"
	"strings"
)

func main() {
	fmt.Println(strings.IndexAny("chicken", "aeiouy")) //2
	fmt.Println(strings.IndexAny("crwth", "aeiouy")) //-1
}
```