И так, преобразования между целыми числами делают очень просто. Вам нужно обернуть в скобки переменную которую вы хотите конвертировать и слева от скобок написать нужный тип, например:  
Преобразование `int8` в `int32` делается таким образом:

```go
var index int8 = 15
var bigIndex int32
bigIndex = int32(index)

// Выведем:
fmt.Println(bigIndex)         // 15
fmt.Printf("%T \n", bigIndex) // int32

// По аналогии выше легко понять как конвертировать в другие типы:
var a int32 = 22
var b uint64
b = uint64(a)

// Выведем
fmt.Println(b)         // 22
fmt.Printf("%T \n", b) // uint64
```

Для того чтобы узнать какое максимальное значение мы можем положить в определенный тип воспользуемся константами из пакета math:

```go
fmt.Println(math.MaxInt8)   // 127
fmt.Println(math.MaxUint8)  // 255
fmt.Println(math.MaxInt16)  // 32767
fmt.Println(math.MaxUint16) // 65535
// ...
```

