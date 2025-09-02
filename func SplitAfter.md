```go
func SplitAfter(s, sep string) []string
```

`SplitAfter` разбивает `s` на все подстроки после каждого экземпляра `sep` и возвращает фрагмент этих подстрок.

Если `s` не содержит `sep` и `sep` не пуст, `SplitAfter` возвращает срез длиной 1, единственным элементом которого является `s`.

Если `sep` пуст, `SplitAfter` разделяет данные после каждой последовательности UTF-8. Если и `s`, и `sep` пусты, `SplitAfter` возвращает пустой срез.

Эквивалентно [[func SplitAfterN]] со значением -1.

```go
package main

import (
	"fmt"
	"strings"
)

func main() {
	fmt.Printf("%q\n", strings.SplitAfter("a,b,c", ","))
	//["a," "b," "c"]
}
```
