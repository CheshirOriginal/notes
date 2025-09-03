```go
func SplitSeq(s, sep string) iter.Seq[string]
```

`SplitSeq` возвращает итератор по всем подстрокам строки `s`, разделённым символом `sep`. Итератор возвращает те же строки, что и [[func Split]] (s, sep), но без построения среза. Он возвращает одноразовый итератор.

```go
package main

import (
	"fmt"
	"strings"
)

func main() {
	s := "a,b,c,d"
	for part := range strings.SplitSeq(s, ",") {
		fmt.Printf("%q\n", part)
	}
	//"a" 
	//"b" 
	//"c" 
	//"d"
}
```
