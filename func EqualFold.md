```go
func EqualFold(s, t string) bool
```

`EqualFold` сообщает, равны ли строки `s` и `t`, интерпретируемые как строки UTF-8, при простом преобразовании регистра Unicode, что является более общей формой нечувствительности к регистру.

==т.е. сравнивает строки без учета регистра==

```go
package main

import (
	"fmt"
	"strings"
)

func main() {
	fmt.Println(strings.EqualFold("Go", "go")) // true 
	fmt.Println(strings.EqualFold("AB", "ab")) // true 
	fmt.Println(strings.EqualFold("ß", "ss"))  // false
}
```