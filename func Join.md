```go
func Join(elems []string , sep string ) string
```

Функция `Join` объединяет элементы первого аргумента в одну строку. Между элементами результирующей строки помещается разделитель `sep`.

```go
package main

import (
	"fmt"
	"strings"
)

func main() {
	s := []string{"foo", "bar", "baz"}
	fmt.Println(strings.Join(s, ", ")) //foo, bar, baz
}
```
