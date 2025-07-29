
https://leetcode.com/explore/learn/card/linked-list/209/singly-linked-list/1290/
### Условие
Разработайте свою реализацию связного списка. Вы можете использовать односвязный или двусвязный список.  
Узел в односвязном списке должен иметь два атрибута: `val` и `next`. `val`— это значение текущего узла, а `next`— указатель/ссылка на следующий узел.  
Если вы хотите использовать двусвязный список, вам понадобится еще один атрибут `prev`для указания предыдущего узла в связанном списке.

Реализовать `MyLinkedList`:
- `MyLinkedList()`Инициализирует `MyLinkedList` объект.
- `int get(int index)`Получите значение `indexth`Узел в связанном списке. Если индекс недействителен, вернуть `-1`.
- `void addAtHead(int val)`Добавить узел значения `val`перед первым элементом связного списка. После вставки новый узел станет первым узлом связного списка.
- `void addAtTail(int val)`Добавить узел значения `val`как последний элемент связанного списка.
- `void addAtIndex(int index, int val)`Добавить узел значения `val`перед `indexth`узел в связанном списке. Если `index`равна длине связанного списка, узел будет добавлен в конец связанного списка. Если `index`больше длины, узел **не будет вставлен** .
- `void deleteAtIndex(int index)`Удалить `indexth`узел в связанном списке, если индекс действителен.
### Решение

```go
type MyLinkedList struct {
	head   *Node
	tail   *Node
	length int
}

type Node struct {
	val  int
	next *Node
}

func Constructor() MyLinkedList {
	return MyLinkedList{}
}

func (this *MyLinkedList) Get(index int) int {
	if index < 0 || index >= this.length {  
    return -1  
	}  
	tmp := this.head  
	for i := 0; i < index; i++ {  
	    tmp = tmp.next  
	}  
	return tmp.val
}

func (this *MyLinkedList) AddAtHead(val int) {
	newNode := &Node{val: val, next: this.head}
	this.head = newNode
	if this.length == 0 {
		this.tail = newNode
	}
	this.length++
}

func (this *MyLinkedList) AddAtTail(val int) {
	newNode := &Node{val: val}
	if this.length != 0 {
		this.tail.next = newNode
	} else {
		this.head = newNode
	}
	this.tail = newNode
	this.length++
}

func (this *MyLinkedList) AddAtIndex(index int, val int) {
	if index < 0 || index > this.length {
		return
	}
	if index == 0 {
		this.AddAtHead(val)
		return
	}
	if index == this.length {
		this.AddAtTail(val)
		return
	}

	newNode := &Node{val: val}
	tmp := this.head

	for i := 0; i < index-1; i++ {
		tmp = tmp.next
	}

	newNode.next = tmp.next
	tmp.next = newNode
	this.length++
}

func (this *MyLinkedList) DeleteAtIndex(index int) {
	if index < 0 || index > this.length-1 {
		return
	}
	if this.length == 1 {
		this.head = nil
		this.tail = nil
		this.length = 0
		return
	}
	if index == 0 {
		this.head = this.head.next
		this.length--
		return
	}
	tmp := this.head
	for i := 0; i < index-1; i++ {
		tmp = tmp.next
	}
	if tmp.next == this.tail {
		tmp.next = nil
		this.tail = tmp
		this.length--
		return
	}
	tmp.next = tmp.next.next
	this.length--
}

func (this *MyLinkedList) PrintAll() {
	tmp := this.head
	for i := 0; i < this.length; i++ {
		fmt.Println(tmp.val)
		tmp = tmp.next
	}
	fmt.Println("head:", this.head)
	fmt.Println("tail:", this.tail)
}
```


