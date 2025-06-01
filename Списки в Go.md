В Go мы можем создать двусвязный список используя пакет `"container/list"`:

```go
package main

import (
	"container/list"
	"fmt"
)

func main() {
	// Создаем список
	mylist := list.New()
	// Добавляем каждый элемент в конец
	mylist.PushBack(1)
	mylist.PushBack(2)
	mylist.PushBack(3)
	
	// Пробегаемся по списку и печатаем не пустые элементы
	// Мы не можем пробежаться привычным способом как с массивами,
	// поэтому придется использовать метод Front() 
	// которая вернет первый элемент и затем с помощью Next
	// получать следующий элемент пока он не будет равен nil, что означает конец списка
	
	for temp := mylist.Front(); temp != nil; temp = temp.Next() {
		fmt.Println(temp.Value)
	}
}
```

В предыдущем примере мы использовали метод `PushBack()` который вставлял элементы в конец списка, так же существует `PushFront()` который вставляет в начало.
### Удаление элементов из списка

Для удаления элемента воспользуемся методом `Remove()`, но он принимает не значение, а **указатель** на элемент:

```go
package main

import (
	"container/list"
	"fmt"
)

func printList(l *list.List) {
	for temp := l.Front(); temp != nil; temp = temp.Next() {
		fmt.Printf("%v ", temp.Value)
	}
}

func main() {
	// Создаем список
	mylist := list.New()
	// Добавляем три элемента
	mylist.PushBack(0)
	mylist.PushBack(1)
	mylist.PushBack(2)
	mylist.PushBack(3)
	// получаем указатель на элемент который добавили (*Element)
	elem3 := mylist.PushBack(4)
	printList(mylist)
	// удаляем элемент '3' по указателю (*Element)
	mylist.Remove(elem3)
	// так же можем воспользоваться методом Front()/Back() чтобы получить первый/последний элемент
	mylist.Remove(mylist.Front())
	fmt.Printf("\nПосле удаления:\n")
	printList(mylist)
}
```

Функция `Remove()` получает указатель на элемент в списке, и мы можем использовать его в удобных ситуациях, когда мы хотим отфильтровывать элементы, которые не соответствуют определенному порогу в нашем связанном списке:

```go
package main

import (
	"container/list"
	"fmt"
)

func main() {
	// Создаем контейнер list и добавляем в него элементы
	myList := list.New()
	myList.PushBack(2)
	myList.PushBack(5)
	myList.PushBack(3)
	myList.PushBack(11)
	myList.PushBack(12)

	// Проходимся по элементам и удаляем те, которые меньше 10
	for e := myList.Front(); e != nil; {
		next := e.Next() // Запоминаем следующий элемент перед удалением текущего
		if e.Value.(int) < 10 {
			myList.Remove(e) // Удаляем текущий элемент из списка
		}
		e = next // Переходим к следующему элементу
	}

	// Выводим список после удаления элементов
	for e := myList.Front(); e != nil; e = e.Next() {
		fmt.Println(e.Value)
	}
}
```

