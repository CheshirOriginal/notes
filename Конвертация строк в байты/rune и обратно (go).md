Строка в Go это срез байтов, поэтому мы можем конвертировать байты в строку и наоборот:

```go
package main

import (
    "fmt"
)

func main() {
    a := "str"

    b := []byte(a)

    c := string(b)

    fmt.Println(a) // str

    fmt.Println(b) // [115 116 114] - побайтовый срез

    fmt.Println(c) // str
}
```

тоже самое работает и со срезами типа **rune**:

```go
package main

import (
    "fmt"
)

func main() {
    a := "строка"

    b := []rune(a) // срез рун

    c := string(b)

    fmt.Println(a) // строка

    fmt.Println(b) // [1089 1090 1088 1086 1082 1072] - срез рун

    fmt.Println(c) // строка
}
```