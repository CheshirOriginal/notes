```go
func FieldsFuncSeq(s string, f func(rune) bool) iter.Seq[string]
```

Функция `FieldsFuncSeq` возвращает итератор по подстрокам строки `s`, разбитым на серии кодовых точек Unicode, удовлетворяющие условию f(c). Итератор возвращает те же строки, что и функция [[func FieldsFunc]] (s), но без построения среза.

```go
package main

import (
	"fmt"
	"strings"
	"unicode"
)

func main() {
	text := "The quick brown fox"
	fmt.Println("Split on whitespace(similar to FieldsSeq):")
	for word := range strings.FieldsFuncSeq(text, unicode.IsSpace) {
		fmt.Printf("%q\n", word)
	}
	// Split on whitespace(similar to FieldsSeq):
	//"The"
	//"quick"
	//"brown"
	//"fox"

	mixedText := "abc123def456ghi"
	fmt.Println("\nSplit on digits:")
	for word := range strings.FieldsFuncSeq(mixedText, unicode.IsDigit) {
		fmt.Printf("%q\n", word)
	}
	//Split on digits:
	//"abc"
	//"def"
	//"ghi"
}
```
