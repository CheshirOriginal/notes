Особую область применения Go представляют запросы по протоколу HTTP. Протокол HTTP работает поверх TCP, и технически мы можем написать приложение, которое принимает или отправляет запросы по протоколу TCP и тем самым отправлять и получать запросы и по протоколу HTTP. Однако в связи с тем, что данный протокол и в целом сфера веб играет большую роль, то все соответствующие функции по работе с http были выделены в отдельный пакет [net/http](https://golang.org/pkg/net/http/).

Для отправки запросов в пакете net/http определен ряд функций:

```go
func Get(url string) (resp *Response, err error)

func Head(url string) (resp *Response, err error)

func Post(url string, contentType string, body io.Reader) (resp *Response, err error)

func PostForm(url string, data url.Values) (resp *Response, err error)
```

- Get(): отправляет запрос GET

- Head(): отправляет запрос HEAD

- Post(): отправляет запрос POST

- PostForm(): отправляет форму в запросе POST

Рассмотрим выполнение самого простого запроса - запроса GET, для которого применяется одноименный метод:

```go
package main

import (
    "fmt"
    "net/http"
)

func main() {
    resp, err := http.Get("https://google.com/")
    if err != nil {
        fmt.Println(err)
        return
    }
    defer resp.Body.Close()

    for true {
        bs := make([]byte, 1024)
        n, err := resp.Body.Read(bs)
        fmt.Println(string(bs[:n]))
        if n == 0 || err != nil {
            break
        }
    }
}
```

Метод `Get()` в качестве параметра принимает адрес ресурса, к которому надо выполнить запрос, и возвращает объект `*http.Response`, который инкапсулирует ответ. Поле Body cтруктуры http.Response представляет ответ от веб-ресурса и при этом также представляет интерфейс `io.ReadCloser`. А это значит, что это поле по сути является потоком для чтения, и мы можем считать пришедшие данные через метод Read. И кроме того, для того, чтобы закрыть поток, необходимо вызвать метод Close. Поэтому после запроса вызывается метод `defer resp.Body.Close()` и в цикле считываем через метод Read данные и выводим на консоль.

### http.PostForm

Для отправки данных в виде формы у нас есть удобная функция `http.PostForm`. Эта функция принимает два параметра `url` и `url.Values` из пакета `net/url`. `url.Values` - это объект, который содержит ключи в виде строк и каждый ключ может содержать несколько строковых значений (`[]string`). В запросе формы вы можете отправить несколько значений по одному имени поля. Вот почему это фрагмент строки, а не только ключ к значению сопоставления. Чтобы получить наши значения, надо считать его из переменной `result`, и обратиться к нему по ключу. (httpbin отправляет различную информацию о запросе в качестве ответа.)

```go
package main

import (
	"encoding/json"
	"log"
	"net/http"
	"net/url"
)

func main() {

	formData := url.Values{
		"name":     {"hello"},
		"surename": {"golang post form"},
	}

	resp, err := http.PostForm("https://httpbin.org/post", formData)
	if err != nil {
		log.Fatalln(err)
	}

	var result map[string]interface{}

	json.NewDecoder(resp.Body).Decode(&result)

	log.Println(result["form"])
}
```

### Query параметры

```go
package main

import (
	"fmt"
	"net/http"
	"net/url"
)

func main() {
	// Создаем URL с параметрами
	baseURL := "https://example.com/api/resource"
	params := url.Values{}
	params.Add("param1", "value1")
	params.Add("param2", "value2")

	fullURL := baseURL + "?" + params.Encode()

	// Отправляем GET-запрос
	response, err := http.Get(fullURL)
	if err != nil {
		fmt.Println("Ошибка при отправке GET-запроса:", err)
		return
	}
	defer response.Body.Close()

	// Чтение ответа
	// ...

	// Обработка ответа
	// ...
}
```

#### Также можно "собрать" URL с query параметрами

В этом примере мы не отправляем запрос, а просто создаем ссылку с параметрами которую позже можно будет использовать в любых методах.

```go
package main

import (
	"fmt"
	"log"
	"net/url"
)

func main() {
	// Парсим базовый URL "https://www.example.com"
	base, err := url.Parse("https://www.example.com")
	if err != nil {
		log.Println(err)
		return
	}

	// Добавляем путь к базовому URL, создавая "https://www.example.com/path"
	base.Path += "path"

	// Создаем query параметры запроса
	params := url.Values{}
	params.Add("id", "15")
	params.Add("name", "Dima")

	// Кодируем параметры запроса в строку и устанавливаем как часть запроса в URL
	base.RawQuery = params.Encode()

	// Выводим итоговый URL
	fmt.Printf("Encoded URL is %q\n", base.String())
}
```

Вывод: `Encoded URL is "https://www.example.com/path?id=15&name=Dima"`

### http.Client

Для осуществления HTTP-запросов также может применяться структура `http.Client`. Чтобы отправить запрос к веб-ресурсу, можно использовать один из ее методов:

```go
func (c *Client) Do(req *Request) (*Response, error)

func (c *Client) Get(url string) (resp *Response, err error)

func (c *Client) Head(url string) (resp *Response, err error)

func (c *Client) Post(url string, contentType string, body io.Reader) (resp *Response, err error)

func (c *Client) PostForm(url string, data url.Values) (resp *Response, err error)
```

Во многом они аналогичны тем одноименным функциям (за исключением метода Do), которые определены в пакете net/http и которые были рассмотрены в прошлой теме. Например, выполнение самого простого запроса GET:

```go
package main
import (
    "fmt"
    "net/http"
    "io"
    "os"
)

func main() {
   client := http.Client{}
   resp, err := client.Get("https://google.com/")
   if err != nil {
         fmt.Println(err)
         return
   }
   defer resp.Body.Close()
   io.Copy(os.Stdout, resp.Body)
}
```

### Настройка клиента

Структура http.Client имеет ряд полей, которые позволяют настроить ее поведение:

- Timeout: устанавливает таймаут для запроса

- Jar: устанавливает куки, отправляемые в запросе

- Transport: определяет механизм выполнения запроса


Установка таймаута:

```go
package main

import (
    "fmt"
    "net/http"
    "io"
    "os"
    "time"
)

func main() {
    client := http.Client{
        Timeout: 6 * time.Second,
    }

    resp, err := client.Get("https://google.com/")
    if err != nil {
        fmt.Println(err)
        return
    }
    defer resp.Body.Close()
    io.Copy(os.Stdout, resp.Body)
}
```

### Request

Для управления запросом и его параметрами в Go используется объект http.Request. Он позволяет установить различные настройки, добавить куки, заголовки, определить тело запроса. Для создания объекта http.Request применяется функция `http.NewRequest()`:

```go
func NewRequest(method, url string, body io.Reader) (*Request, error)
```

Функция принимает три параметра. Первый параметр - тип запроса в виде строки ("GET", "POST"). Второй параметр - адрес ресурса. Третий параметр - тело запроса.

Для отправки объекта Request можно применять метод `Do()`:

```go
Do(req *http.Request) (*http.Response, error)
```

Например:

```go
package main

import (
    "fmt"
    "net/http"
    "io"
    "os"
)

func main() {
    client := &http.Client{}
    req, err := http.NewRequest(
         "GET", "https://google.com/", nil,

    )

    // добавляем заголовки

    req.Header.Add("Accept", "text/html") // добавляем заголовок Accept

    req.Header.Add("User-Agent", "MSIE/15.0") // добавляем заголовок User-Agent

    resp, err := client.Do(req)
    if err != nil {
        fmt.Println(err)
        return
    }
    defer resp.Body.Close()

    io.Copy(os.Stdout, resp.Body)
}
```

