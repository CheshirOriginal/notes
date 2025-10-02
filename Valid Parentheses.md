https://leetcode.com/problems/valid-parentheses/description/
### Условие

Дана строка s, содержащая только символы '(', ')', '{', '}', '[' и ']', определите, является ли входная строка допустимой.

Входная строка действительна, если:

- Открытые скобки должны быть закрыты скобками того же типа.
- Открытые скобки должны быть закрыты в правильном порядке.
- Каждой закрывающейся скобке соответствует открывающая скобка того же типа.
### Пример
**Ввод:** s = "()[]{}"
**Выход:** true

**Ввод:** s = "(]"
**Выход:** false
### Решение

красивое решение с реализацией стека

```go
// суть проста, проходимся по строке, если текущей скобке есть пара в стеке, то удаляем эту пару, если нет то добавляем скобку в стек. Если стек в конце пустой, значит строка валидная
func isValid(s string) bool {  
    stack := CreateStack()  
  
    for i := 0; i < len(s); i++ {  
       if tmp, ok := stack.Top(); ok && (tmp == '(' && s[i] == ')' || tmp == '{' && s[i] == '}' || tmp == '[' && s[i] == ']') {  
          stack.Pop()  
       } else {  
          stack.Push(s[i])  
       }  
    }  
    if stack.IsEmpty() {  
       return true  
    }  
    return false  
}  
  
type ByteStack []byte  
  
func (s *ByteStack) Push(v byte) {  
    *s = append(*s, v)  
}  
  
func (s *ByteStack) Pop() (byte, bool) {  
    if len(*s) == 0 {  
       return 0, false  
    }  
    n := len(*s)  
    res := (*s)[n-1]  
    *s = (*s)[:n-1]  
    return res, true  
}  
  
func (s *ByteStack) Top() (byte, bool) {  
    if len(*s) == 0 {  
       return 0, false  
    }  
    n := len(*s)  
    res := (*s)[n-1]  
    return res, true  
}  
  
func (s *ByteStack) Len() int {  
    return len(*s)  
}  
  
func (s *ByteStack) IsEmpty() bool {  
    return len(*s) == 0  
}  
  
func CreateStack() *ByteStack {  
    return &ByteStack{}  
}
```

Более функциональное решение, но суть та же

```go
func isValid(s string) bool {
    stack := []byte{}
    for i := 0; i < len(s); i++ {
        if len(stack) > 0 &&
           ((stack[len(stack)-1] == '(' && s[i] == ')') ||
            (stack[len(stack)-1] == '{' && s[i] == '}') ||
            (stack[len(stack)-1] == '[' && s[i] == ']')) {
            stack = stack[:len(stack)-1]
        } else {
            stack = append(stack, s[i])
        }
    }
    return len(stack) == 0
}
```