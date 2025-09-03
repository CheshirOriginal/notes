```go
func ToValidUTF8(s, replacement string) string
```

`ToValidUTF8` возвращает копию строки `s`, в которой каждая последовательность недопустимых байтов UTF-8 заменена заменяющей строкой, которая может быть пустой.

```go
package main

import (
	"fmt"
	"strings"
)

func main() {
	fmt.Printf("%s\n", strings.ToValidUTF8("abc", "\uFFFD"))
	//abc
	fmt.Printf("%s\n", strings.ToValidUTF8("a\xffb\xC0\xAFc\xff", ""))
	//abc
	fmt.Printf("%s\n", strings.ToValidUTF8("\xed\xa0\x80", "abc"))
	//abc
}
```