#### Хорошие имена  

Хорошее имя - это:
- Последовательное (легко угадываемое),  
- короткое (легко набираемое),  
- точное (легкое для понимания).

#### Практическое правило

Чем больше расстояние между объявлением имени и его использованием, тем длиннее должно быть имя.

#### Следует использовать MixedCase

`peopleCount`

Все аббревиатуры должны быть заглавными, как в `ServeHTTP` и `IDProcessor`.

#### Локальные переменные  

Делайте их короткими; длинные имена не позволяют понять, что делает код. В обычных комбинациях переменных и типов могут использоваться действительно короткие имена:  
  
Предпочитайте индексировать `i`.  
Предпочитайте `r` для чтения.  
Предпочитайте `b` для буферизации.  
  
Избегайте избыточных имен, учитывая их контекст:  
  
Предпочитайте `count` вместо `runeCount` внутри функции с именем `RuneCount`.  
Предпочитайте `ok` вместо `keyInMap` в инструкции.

Пример плохого именования:

```go
func RuneCount(buffer []byte) int {
    runeCount := 0
    for index := 0; index < len(buffer); {
        if buffer[index] < RuneSelf {
            index++
        } else {
            _, size := DecodeRune(buffer[index:])
            index += size
        }
        runeCount++
    }
    return runeCount
}
```

Пример хорошего именования:

```go
func RuneCount(b []byte) int {
    count := 0
    for i := 0; i < len(b); {
        if b[i] < RuneSelf {
            i++
        } else {
            _, n := DecodeRune(b[i:])
            i += n
        }
        count++
    }
    return count
}
```

#### Параметры  
  
Если типы являются описательными, они должны быть краткими:

```go
func AfterFunc(d Duration, f func()) *Timer

func Escape(w io.Writer, s []byte)
```

Там, где типы более неоднозначны, названия могут содержать документацию:

```go
func Unix(sec, nsec int64) Time

func HasPrefix(s, prefix []byte) bool
```

#### Возвращаемые значения  

Имена возвращаемых значений в экспортируемых функциях следует указывать только в целях документации.  
  
Это хорошие примеры именованных возвращаемых значений:

```go
func Copy(dst Writer, src Reader) (written int64, err error)

func ScanBytes(data []byte, atEOF bool) (advance int, token []byte, err error)
```

#### Получатели    
  
По общему правилу, это один или два символа, которые отражают тип получателя, поскольку обычно они встречаются почти в каждой строке:

```go
func (b *Buffer) Read(p []byte) (n int, err error)

func (sh serverHandler) ServeHTTP(rw ResponseWriter, req *Request)

func (r Rectangle) Size() Point
```

Имена получателей должны совпадать во всех методах типа. (Не используйте `r` в одном методе и `rdr` в другом.)

#### Интерфейсы  

Интерфейсы, которые определяют только один метод, обычно представляют собой просто имя функции с добавлением "er".

```go
type Reader interface {
    Read(p []byte) (n int, err error)
}
```

#### Ошибки  

Типы ошибок должны иметь вид FooError:

```go
type ExitError struct {
    ...
}
```

Значения ошибок должны иметь вид ErrFoo:

```go
var ErrFormat = errors.New("image: unknown format")
```

