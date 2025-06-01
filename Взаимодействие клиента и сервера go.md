В прошлой теме было рассмотрено создание простейшего сервера TCP, к которому подключается клиент, и этому клиенту отправляется некоторое сообщение. Теперь рассмотрим, как клиент может посылать сообщения и получать ответ.

Для начала определим следующий сервер:

```go
package main
import (
    "fmt"
    "net"
)

var dict = map[string]string {
    "red": "красный",
    "green": "зеленый",
    "blue": "синий",
    "yellow": "желтый",
}

func main() {
    listener, err := net.Listen("tcp", ":4545")
    if err != nil {
        fmt.Println(err)
        return
    }
    defer listener.Close()

    fmt.Println("Server is listening...")

    for {
        conn, err := listener.Accept()
        if err != nil {
            fmt.Println(err)
            conn.Close()
            continue
        }
        go handleConnection(conn) // запускаем горутину для обработки запроса
    }
}

// обработка подключения

func handleConnection(conn net.Conn) {
    defer conn.Close()

    for {
        // считываем полученные в запросе данные
        input := make([]byte, (1024 * 4))
        n, err := conn.Read(input)
        if n == 0 || err != nil {
            fmt.Println("Read error:", err)
            break
        }  
        source := string(input[0:n])
        // на основании полученных данных получаем из словаря перевод
        target, ok := dict[source]
        if ok == false { // если данные не найдены в словаре
            target = "undefined"
        }
        // выводим на консоль сервера диагностическую информацию
        fmt.Println(source, "-", target)
        // отправляем данные клиенту
        conn.Write([]byte(target))
    }
}
```

Сервер имитирует поведение программы для перевода слов. Для этого определен словарь dit, который содержит англоязычные слова и их перевод.

В бесконечном цикле сервер принимает подключения. Однако вместо прямой обработки подключения сервер запускает горутину в виде функции handleConnection, в которой и обрабатывается подключение. Это позволит входящим клиентам не ждать, пока первый из них будет обработан. Таким образом, все входящие клиенты в определенной степени будут обрабатываться одновременно.

В функции handleConnection получаем запрос от клиента. Для этого выделяем буфер достаточной длины в 4096 байт.

В данном случае мы ожидаем, что запрос от клиента не превысит 4096 байт, однако точный размер запроса и его максимальный размер не всегда бывают известны. В этом случае мы можем применять различные техники, в частности, в бесконечном цикле считывать данные запроса от клиента и только потом их обрабатывать. Но в данном случае мы разберем более простую ситуацию.

Получив запрос, преобразовав его в строку, получаем значение из словаря и отправляем его обратно клиенту.

Для взаимодействия с этим сервером определим следующий клиент:

```go
package main
import (
    "fmt"
    "net"
)

func main() {
    conn, err := net.Dial("tcp", "127.0.0.1:4545")
    if err != nil {
        fmt.Println(err)
        return
    }
    defer conn.Close()

    for {
        var source string
        fmt.Print("Введите слово: ")
        _, err := fmt.Scanln(&source)
        if err != nil {
            fmt.Println("Некорректный ввод", err)
            continue
        }
        
        // отправляем сообщение серверу

        if n, err := conn.Write([]byte(source));
        n == 0 || err != nil {
            fmt.Println(err)
            return
        }
        
        // получем ответ

        fmt.Print("Перевод:")
        buff := make([]byte, 1024)
        n, err := conn.Read(buff)
        if err != nil{ break}
        fmt.Print(string(buff[0:n]))
        fmt.Println()
    }
}
```

На клиенте в бесконечном цикле вводим слово для перевода и отправляем серверу сообщение. И затем получаем от сервера ответ и выводим его на консоль.

Запустим сервер.

![[Pasted image 20240627162630.png]]

Затем запустим программу клиента и введем какое-либо значение и сервер возвратит перевод слова:

![[Pasted image 20240627162646.png]]

### Http сервер

Как вы уже знаете для работы с http используется пакет `net/http`, напишем hello-world, используя модуль http:

```go
package main

import (
	"fmt"
	"net/http"
)

// Обработчик HTTP-запросов
func handler(w http.ResponseWriter, r *http.Request) {
	w.Write([]byte("Привет, Stepik!"))
}

func main() {
	// Регистрируем обработчик для пути "/"
	http.HandleFunc("/", handler)

	// Запускаем веб-сервер на порту 8080
	err := http.ListenAndServe(":8080", nil)
	if err != nil {
		fmt.Println("Ошибка запуска сервера:", err)
	}
}
```

Перейдя по ссылке `http://localhost:8080` в браузере, вы увидите сообщение "Привет, stepik!". Рассмотрим данный код подробнее:

`http.HandleFunc` - регистрирует обработчик и принимает функцию которая должна принимать  `http.ResponseWriter, *http.Request.`  ResponseWriter это интерфейс который нужен, для того чтобы отвечать клиенту. У него есть метод `Write([]byte)` с помощью которого мы и отвечаем "Привет, Stepik!". А вот `*http.Request` уже конкретная структура, в которой содержится информация о запросе, от туда мы можем узнать много информации, включая метод, URL, тело запроса и т.д. Давайте попробуем вывести информацию о запросе:

```go
// Обработчик HTTP-запросов 
func handler(w http.ResponseWriter, r *http.Request) {
	fmt.Println(r.Method) // Тип метода 
	fmt.Println(r.URL) // запрашиваемый
	URL fmt.Println(r.Proto) // версия протокола 
	w.Write([]byte("Привет, Stepik!")) 
}
```

### Query параметры

Вначале надо получить запрошенный адрес через переменную `URL`. Далее у адреса вызывается метод `Query()`, который и возвращает строку запроса. Чтобы получить значение отдельного параметра, применяется метод `Get()`, в который передается имя параметра.

Например, получим данные строки запроса:

```go
package main

import (
	"fmt"
	"net/http"
)

// Обработчик HTTP-запросов
func handler(w http.ResponseWriter, r *http.Request) {
	fmt.Println("RawQuery: ", r.URL.String()) // URL с параметрами
	fmt.Println("Name: ", r.URL.Query().Get("name")) // значение параметра
	fmt.Println("IsExist: ", r.URL.Query().Has("name")) // существует ли такой параметр
	w.Write([]byte("Привет, Stepik!"))
}

func main() {
	// Регистрируем обработчик для пути "/"
	http.HandleFunc("/", handler)

	// Запускаем веб-сервер на порту 8080
	err := http.ListenAndServe(":8080", nil)
	if err != nil {
		fmt.Println("Ошибка запуска сервера:", err)
	}
}
```

### Собственные обработчики

#### HTTP-обработчики

В прошлых шагах мы писали простые обработчики:

```go
// Обработчик HTTP-запросов
func handler(w http.ResponseWriter, r *http.Request) {
	w.Write([]byte("Привет, Stepik!"))
}
```

Обработчик — это то, что принимает запрос и возвращает ответ. В Go обработчики реализуют интерфейс со следующей сигнатурой: 

```go
type Handler interface {
        ServeHTTP(ResponseWriter, *Request)
}
```

Мы использовали `http.HandleFunc("/", handler)`, таким образом оборачивая другую функцию, принимающую `http.ResponseWriter` и `http.Request`, в `ServeHTTP`. Таким образом, обработчики формируют ответы на запросы. Для каждой цели в языке Go реализованы разные обработчики. 

Обработка HTTP-запросов в Go завязывается на двух вещах: ServeMux'ы и Handler'ы. На самом деле когда мы вызываем HandleFunc то под капотом используется стандартный `DefaultServeMux`

[ServeMux](http://golang.org/pkg/net/http/#ServeMux) - это HTTP-request роутер (или мультиплексор). Он сравнивает входящие запросы со списком предопределенных URL - путей и вызывает нужный обработчик, соответсвующий этому url-пути.

Handler'ы ответственны за написание ответов и тел запросов. Почти любой объект может быть handler'ом, покуда он удовлетворяет `http.Handler` интерфейсу. Проще говоря, он должен иметь метод `ServeHTTP` с последующей сигнатурой:

`ServeHTTP(http.ResponseWriter, *http.Request)`

Давайте напишем сервер со своим `serverMux`:

```go
package main

import (
	"fmt"
	"net/http"
)

func handler(w http.ResponseWriter, r *http.Request) {
	w.Write([]byte("OK!"))
}

func main() {
	// создаем свой serverMux
	serverMux := http.NewServeMux()
	serverMux.HandleFunc("/", handler)
	// Запускаем веб-сервер на порту 8080 с нашим serverMux (в прошлых примерах был nil)
	err := http.ListenAndServe(":8080", serverMux)
	if err != nil {
		fmt.Println("Ошибка запуска сервера:", err)
	}
}
```

Кратко:

- В `main` мы используем `[http.NewServeMux](http://golang.org/pkg/net/http/#NewServeMux)⁣, чтобы создать` пустой ServeMux.
- Потом мы используем  HandleFunc⁣, чтобы создать новый handler. Этот обработчик перенаправляет все запросы.
- В конце мы создаем сервер с помощью [`http.ListenAndServe`](http://golang.org/pkg/net/http/#ListenAndServe), передавая ServeMux, чтобы он мог контролировать запросы.

Сигнатура для ListenAndServe - `ListenAndServe(addr string, handler Handler)`, но мы передавали ServeMux  вторым параметром.

Мы можем это сделать, т.к ServeMux имеет `[ServeHTTP](http://golang.org/pkg/net/http/#ServeMux.ServeHTTP)` метод, это значит, что он удовлетворяет типу Handler.

ServeMux - особенный handler, он передает запрос другому обработчику. Связки обработчиков — обычная практика в Go.

Давайте создадим свой собственный обработчик, который возвращает дату на запрос:

```go
type timeHandler struct {
  format string
}

func (th *timeHandler) ServeHTTP(w http.ResponseWriter, r *http.Request) {
  tm := time.Now().Format(th.format)
  w.Write([]byte("The time is: " + tm))
}
```

У нас есть структура `timeHandler`, и мы описали метод с сигнатурой `ServeHTTP(http.ResponseWriter, *http.Request)` в ней. Это все, что нам нужно, чтобы описать handler.

```go
func main() {
  mux := http.NewServeMux()

  th1123 := &timeHandler{format: time.RFC1123}
  mux.Handle("/time/rfc1123", th1123)

  th3339 := &timeHandler{format: time.RFC3339}
  mux.Handle("/time/rfc3339", th3339)

  log.Println("Listening...")
  http.ListenAndServe(":3000", mux)
}
```

В функции `main` мы инициализировали `timeHandler` как любую другую структуру, используя `&`  для инициализации указателя. И как в предыдущем примере, мы используем `mux.Handle⁣`, чтобы зарегистрировать наш ServeMux.

Когда мы запустим наше приложение, любой запрос к url `/time` будет обрабатываться `timeHandler.ServeHTTP` методом.

Заметьте, что мы можем переиспользовать `timeHandler` по разным url-путям:

В этом примере мы передаём простую строку в обработчик, но в настоящем приложении вы можете использовать такой подход, чтобы передавать шаблоны, соединения с базами данных, или другой контекст. Это хорошая альтернатива для глобальных переменных.