### time

Модуль `time` стандартной библиотеки предоставляет в наше распоряжение ряд примитивов для работы со временем.

## type Time
### Создание структуры Time

Первый примитив – структура `Time` — конкретные дата и время. Создать эту структуру с конкретным значением нам позволяет ряд функций:

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	// func Now() Time
	// возвращает текущую дату и время
	now := time.Now()

	// func Date(year int, month Month, day, hour, min, sec, nsec int, loc *Location) Time
	// возвращает дату и время в соответствии с заданными параметрами: годом, месяцем, днем, временем и пр.
	currentTime := time.Date(
		2020,     // год
		time.May, // месяц
		15,       // день
		10,       // часы
		13,       // минуты
		12,       // секунды
		45,       // наносекунды
		time.UTC, // временная зона
	)

	// func Unix(sec int64, nsec int64) Time
	// возвращает дату и время в соответствии с заданными параметрами: секундами и наносекундами, прошедшими с начала эпохи Unix — 01.01.1970 г.
	
	unixTime := time.Unix(
		150000, // секунды
		1,      // наносекунды
	)

	fmt.Println(now.Format("02-01-2006 15:04:05")) // 15-05-2020 09:58:16
	fmt.Println(currentTime.Format("02-01-2006 15:04:05")) // 15-05-2020 10:13:12
	fmt.Println(unixTime.Format("02-01-2006 15:04:05"))    // 02-01-1970 22:40:00
}
```
### Конвертирование строк в структуру Time

На практике очень часто возникает задача конвертировать данные о дате и времени из строкового вида, чтобы в дальнейшем получить доступ к методам работы со временем. Однако вариантов строкового представления даты и времени очень много, в некоторых случаях нам интересна только дата, а в некоторых — только время. Как объяснить Go, что к чему в строке? Для этого мы задаем Go шаблон, с которым и сравнивается целевая строка.

==Вот основа для шаблона: «Mon Jan 2 15:04:05 MST 2006»: понедельник, 2 января 2006 г. 15:04:05, североамериканское горное стандартное время.== В первый раз это может вызвать сложности, но в дальнейшем вы запомните эти базовые дату и время.

За парсинг данных из строковых отвечают 2 функции:

```go
// func Parse(layout, value string) (Time, error)
// парсит дату и время в строковом представлении
firstTime, err := time.Parse("2006/01/02 15-04", "2020/05/15 17-45")
if err != nil {
	panic(err)
}

// LoadLocation находит временную зону в справочнике IANA
// https://www.iana.org/time-zones
loc, err := time.LoadLocation("Asia/Yekaterinburg")
if err != nil {
	panic(err)
}

// func ParseInLocation(layout, value string, loc *Location) (Time, error)
// парсит дату и время в строковом представлении с отдельным указанием временной зоны
secondTime, err := time.ParseInLocation("Jan 2 06 03:04:05pm", "May 15 20 05:45:10pm", loc)
if err != nil {
	panic(err)
}

fmt.Println(firstTime.Format("02-01-2006 15:04:05"))  // 15-05-2020 17:45:00
fmt.Println(secondTime.Format("02-01-2006 15:04:05")) // 15-05-2020 17:45:10
```

Вновь встречаем тот же метод Format, только теперь его аргумент должен быть гораздо более понятен — Format возвращает нам строковое представление времени в соответствии с заданным шаблоном.

### Методы структуры Time

#### Методы, возвращающие отдельные элементы структуры

Таких методов довольно много и в целом они не должны вызвать никаких проблем, для большей части этих методов мы сделаем короткие примеры:

```go
current := time.Date(2020, time.May, 15, 17, 45, 12, 0, time.Local)

// func (t Time) Date() (year int, month Month, day int)
fmt.Println(current.Date()) // 2020 May 15

// func (t Time) Year() int
fmt.Println(current.Year()) // 2020

// func (t Time) Month() Month
fmt.Println(current.Month()) // May

// func (t Time) Day() int
fmt.Println(current.Day()) // 15

// func (t Time) Clock() (hour, min, sec int)
fmt.Println(current.Clock()) // 17 45 12

// func (t Time) Hour() int
fmt.Println(current.Hour()) //17

// func (t Time) Minute() int
fmt.Println(current.Minute()) // 45

// func (t Time) Second() int
fmt.Println(current.Second()) // 12

// func (t Time) Unix() int64
fmt.Println(current.Unix()) // 1589546712

// func (t Time) Weekday() Weekday
fmt.Println(current.Weekday()) // Friday

// func (t Time) YearDay() int
fmt.Println(current.YearDay()) // 136
```

Какие-то дополнительные комментарии к представленным методам не требуются — имена методов и возвращаемые значения говорят сами за себя.

#### Конвертирование структуры Time в строку

С методом Format мы уже знакомы.

```go
// func (t Time) Format(layout string) string
current := time.Date(2020, time.May, 15, 17, 45, 12, 0, time.Local)
fmt.Println(current.Format("02-01-2006 15:04:05")) // 15-05-2020 17:45:12
```

#### Сравнение структур Time

```go
firstTime := time.Date(2020, time.May, 15, 17, 45, 12, 0, time.Local)
secondTime := time.Date(2020, time.May, 15, 16, 45, 12, 0, time.Local)

// func (t Time) After(u Time) bool
// true если позже
fmt.Println(firstTime.After(secondTime)) // true

// func (t Time) Before(u Time) bool
// true если раньше
fmt.Println(firstTime.Before(secondTime)) // false

// func (t Time) Equal(u Time) bool
// true если равны
fmt.Println(firstTime.Equal(secondTime)) // false
```

#### Методы, изменяющие структуру Time

```go
now := time.Date(2020, time.May, 15, 17, 45, 12, 0, time.Local)

// func (t Time) Add(d Duration) Time
// изменяет дату в соответствии с параметром - "продолжительностью"
future := now.Add(time.Hour * 12) // перемещаемся на 12 часов вперед

// func (t Time) AddDate(years int, months int, days int) Time
// изменяет дату в соответствии с параметрами - количеством лет, месяцев и дней
past := now.AddDate(-1, -2, -3) // перемещаемся на 1 год, два месяца и 3 дня назад

// func (t Time) Sub(u Time) Duration
// вычисляет время, прошедшее между двумя датами
fmt.Println(future.Sub(past)) // 10332h0m0s
```

Обратите внимание, что в методах Add и AddDate могут использоваться и отрицательные значения, это позволяет не только «добавлять» время (что видно из названий методов), но и «отнимать» его.

### type Duration

В предыдущем шаге мы увидели такой тип как `Duration` — продолжительность. Рассмотрим его подробнее. Внутри `Duration` представляет из себя `int64`, определяющий количество наносекунд, прошедших между двумя моментами времени.

Создается экземпляр типа Duration одной из следующих функций:

```go
now := time.Now()
past := now.AddDate(0, 0, -1)
future := now.AddDate(0, 0, 1)

// func Since(t Time) Duration
// вычисляет период между текущим моментом и заданным временем в прошлом
fmt.Println(time.Since(past).Round(time.Second)) // 24h0m0s

// func Until(t Time) Duration
// вычисляет период между текущим моментом и заданным временем в будущем
fmt.Println(time.Until(future).Round(time.Second)) // 24h0m0s

// func ParseDuration(s string) (Duration, error)
// преобразует строку в Duration с использованием аннотаций:
// "ns" - наносекунды,
// "us" - микросекунды,
// "ms" - миллисекунды,
// "s" - секунды,
// "m" - минуты,
// "h" - часы.
dur, err := time.ParseDuration("1h12m3s")
if err != nil {
	panic(err)
}
fmt.Println(dur.Round(time.Hour).Hours()) // 1
```

Время — вещь текучая (и в общем, конечно, не вещь вовсе), поэтому не всегда нам удается получить то значение, какое мы ожидаем. Чтобы увидеть конкретный результат, который мы ожидали получить, мы в дополнение к рассматриваемой функции использовали метод Round, округляющий значение до ближайшего целого с заданной точностью.

У типа Duration помимо метода Round, который мы рассмотрели выше, есть ряд других методов, позволяющих вернуть часть значения: часы, минуты, секунды и пр.

```go
func (d Duration) Hours() float64
func (d Duration) Minutes() float64
func (d Duration) Seconds() float64
func (d Duration) Milliseconds() int64
func (d Duration) Microseconds() int64
func (d Duration) Nanoseconds() int64
```

Завершая разговор об этом типе отметим, что модуль time содержит ряд констант типа Duration:

```go
const (
	Nanosecond  Duration = 1
	Microsecond          = 1000 * Nanosecond
	Millisecond          = 1000 * Microsecond
	Second               = 1000 * Millisecond
	Minute               = 60 * Second
	Hour                 = 60 * Minute
)
```

