```go
func SplitAfterSeq(s, sep string) iter.Seq[string]
```

`SplitAfterSeq` возвращает итератор по подстрокам строки `s` split после каждого вхождения sep. Итератор возвращает те же строки, что и [SplitAfter](https://pkg.go.dev/strings#SplitAfter) (s, sep), но без построения среза. Он возвращает одноразовый итератор.

```go
package main

import (
	"fmt"
	"strings"
)

func main() {
	fmt.Printf("%q\n", strings.SplitAfterN("a,b,c", ",", 2))
	//["a," "b,c"]
}
```
