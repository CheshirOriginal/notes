```go
func Count(s, substr string) int
```

Count подсчитывает количество неперекрывающихся вхождений substr в s. Если substr — пустая строка, Count возвращает 1 + количество кодовых точек Unicode в s.

```go
package main

import (
	"fmt"
	"strings"
)

func main() {
	fmt.Println(strings.Count("cheese", "e")) //3
	fmt.Println(strings.Count("five", "")) //5
}
```