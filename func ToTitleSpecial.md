```go
func ToTitleSpecial(c unicode.SpecialCase, s string) string
```

`ToTitleSpecial` возвращает копию строки `s`, в которой все буквы Unicode сопоставлены с их заглавными буквами Unicode, отдавая приоритет специальным правилам регистра.

```go
package main

import (
	"fmt"
	"strings"
	"unicode"
)

func main() {
	fmt.Println(strings.ToTitleSpecial(unicode.TurkishCase, "dünyanın ilk borsa yapısı Aizonai kabul edilir"))
	//DÜNYANIN İLK BORSA YAPISI AİZONAİ KABUL EDİLİR
}
```