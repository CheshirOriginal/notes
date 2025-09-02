```go
func FieldsSeq(s string) iter.Seq[string]
```

`FieldsSeq` возвращает итератор по подстрокам строки `s`, разделённым на группы пробельных символов, как определено в `unicode.IsSpace` . Итератор возвращает те же строки, что и функция [[func Fields]] (s), но без построения среза.

```go
package main

import (
	"fmt"
	"strings"
)

func main() {
	text := "The quick brown fox"
	fmt.Println("Split string into fields:")
	for word := range strings.FieldsSeq(text) {
		fmt.Printf("%q\n", word)
	}
	//Split string into fields:
	//"The"
	//"quick"
	//"brown"
	//"fox"

	textWithSpaces := "  lots   of   spaces  "
	fmt.Println("\nSplit string with multiple spaces:")
	for word := range strings.FieldsSeq(textWithSpaces) {
		fmt.Printf("%q\n", word)
	}
	//Split string with multiple spaces:
	//"lots"
	//"of"
	//"spaces"
}
```
