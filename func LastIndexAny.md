```go
func LastIndexAny(s, chars string ) int
```

`LastIndexAny` возвращает индекс последнего экземпляра любой кодовой точки Unicode из `chars` в `s` или -1, если в `s` нет ни одной кодовой точки Unicode из `chars`.

```go
package main

import (
	"fmt"
	"strings"
)

func main() {
	fmt.Println(strings.LastIndexAny("go gopher", "go")) //4
	fmt.Println(strings.LastIndexAny("go gopher", "rodent")) //8
	fmt.Println(strings.LastIndexAny("go gopher", "fail")) //-1
}
```