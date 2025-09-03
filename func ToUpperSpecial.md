```go
func ToUpperSpecial(c unicode.[SpecialCase, s string) string
```

`ToUpperSpecial` возвращает копию строки `s`, в которой все буквы Unicode преобразованы в верхний регистр с использованием сопоставления регистров, заданного параметром `c`.

```go
package main

import (
	"fmt"
	"strings"
	"unicode"
)

func main() {
	fmt.Println(strings.ToUpperSpecial(unicode.TurkishCase, "örnek iş"))
	//ÖRNEK İŞ
}

```