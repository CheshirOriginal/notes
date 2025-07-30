
https://leetcode.com/explore/interview/card/top-interview-questions-easy/127/strings/883/
### Условие
Фраза является **палиндромом** , если после преобразования всех заглавных букв в строчные и удаления всех символов, кроме букв и цифр, она читается одинаково слева направо и справа налево. К буквенно-цифровым символам относятся буквы и цифры.

Дана строка `s`, возвращаться `true`_если это **палиндром** , или_ `false`_в противном случае_ .
### Пример

**Ввод:** s = "A man, a plan, a canal: Panama"
**Выход:** true

**Ввод:** s = "race a car"
**Выход:** false
### Решение

```go
func isPalindrome(s string) bool {
    i, j := 0, len(s)-1
    for i < j {
        for i < j && !isAlnum(s[i]) {
            i++
        }
        for i < j && !isAlnum(s[j]) {
            j--
        }
        if i < j {
            if toLower(s[i]) != toLower(s[j]) {
                return false
            }
            i++
            j--
        }
    }
    return true
}

func isAlnum(b byte) bool {
    return ('a' <= b && b <= 'z') ||
           ('A' <= b && b <= 'Z') ||
           ('0' <= b && b <= '9')
}

func toLower(b byte) byte {
    if 'A' <= b && b <= 'Z' {
        return b + ('a' - 'A')
    }
    return b
}
```


