```go
func ToLowerSpecial(c unicode.SpecialCase, s string) string
```

`ToLowerSpecial` возвращает копию строки `s`, в которой все буквы Unicode преобразованы в строчные с использованием сопоставления регистров, заданного параметром `c`.

```go
package main

import (
	"fmt"
	"strings"
	"unicode"
)

func main() {
	fmt.Println(strings.ToLowerSpecial(unicode.TurkishCase, "Örnek İş"))
	//örnek iş
}
```