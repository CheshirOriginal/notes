```go
func Cut(s, sep string) (before, after string, found bool)
```

Вырезает фрагменты `s` вокруг первого вхождения `sep`, возвращая текст до и после `sep`, `found` сообщает, встречается ли `sep` в `s`. Если `sep` не встречается в `s`, `cut` возвращает `s`, "", `false`.

```go
package main

import (
	"fmt"
	"strings"
)

func main() {
	show := func(s, sep string) {
		before, after, found := strings.Cut(s, sep)
		fmt.Printf("Cut(%q, %q) = %q, %q, %v\n", s, sep, before, after, found)
	}
	show("Gopher", "Go") //"", "pher", true
	show("Gopher", "ph") //"Go", "er", true
	show("Gopher", "er") //"Goph", "", true
	show("Gopher", "Badger") //"Gopher", "", false
}
```

