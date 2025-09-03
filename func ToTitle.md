```go
func ToTitle(s string) string
```

`ToTitle` возвращает копию строки `s`, в которой все буквы Unicode сопоставлены с их заглавными буквами Unicode.

```go
package main

import (
	"fmt"
	"strings"
)

func main() {
	fmt.Println(strings.ToTitle("her royal highness"))
	//HER ROYAL HIGHNESS
	fmt.Println(strings.ToTitle("loud noises"))
	//LOUD NOISES
	fmt.Println(strings.ToTitle("брат")) 
	//БРАТ
}
```