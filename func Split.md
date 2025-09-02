```go
func Split(s, sep string) []string
```

Разбивает фрагменты `s` на все подстроки, разделенные символом `sep`, и возвращает фрагмент подстрок между этими разделителями.

Если `s` не содержит `sep` и `sep` не пуст, `Split` возвращает срез длиной 1, единственным элементом которого является `s`.

Если `sep` пуст, `Split` разделяется после каждой последовательности UTF-8. Если и `s`, и `sep` пусты, Split возвращает пустой срез.

Эквивалентно [[func SplitN]] со значением -1.

Чтобы выполнить разделение по первому вхождению разделителя, см. [[func Cut]].

```go
package main

import (
	"fmt"
	"strings"
)

func main() {
	fmt.Printf("%q\n", strings.Split("a,b,c", ","))
	//["a" "b" "c"]
	fmt.Printf("%q\n", strings.Split("a man a plan a canal panama", "a "))
	//["" "man " "plan " "canal panama"]
	fmt.Printf("%q\n", strings.Split(" xyz ", ""))
	//[" " "x" "y" "z" " "]
	fmt.Printf("%q\n", strings.Split("", "Bernardo O'Higgins"))
	//[""]
}
```
