Контейнеры `map`, `set` и их мультиверсии предоставляют двусторонние итераторы, которые можно сдвигать на соседние позиции вперёд и назад. Как и в случае последовательных контейнеров, запрещено выходить за пределы диапазона, ограниченного `begin()` и `end()`, и разыменовывать итератор, равный `end()`. Итераторы таких контейнеров и ссылки (указатели) на элементы никогда не инвалидируются.

```c++
#include <iostream>
#include <iterator>
#include <map>
#include <string>

int main() {
    std::map<int, std::string> numbers = {
        {100, "hundred"},
        {3, "three"},
        {42, "forty two"},
        {11, "eleven"},
    };

    auto iter = numbers.find(11);

    if (iter != numbers.end()) {
        // печатаем найденный элемент
        const auto& [key, value] = *iter;
        std::cout << "Found: " << key << ": " << value << "\n";  // Found: 11: eleven

        // печатаем предыдущий элемент
        if (iter != numbers.begin()) {
            const auto& [key, value] = *std::prev(iter);
            std::cout << "Previous: " << key << ": " << value << "\n";  // Previous: 3: three
        } else {
            std::cout << "No previous element\n";
        }

        // печатаем следующий элемент
        if (auto nextIter = std::next(iter); nextIter != numbers.end()) {
            const auto& [key, value] = *nextIter;
            std::cout << "Next: " << key << ": " << value << "\n";  // Next: 42: forty two
        } else {
            std::cout << "No next element\n";
        }
    } else {
        std::cout << "Not found\n";
    }
}
```

Итераторы unordered-контейнеров однонаправленные: их можно сдвигать только вперёд. Это связано с тем, что коллизии в хеш-таблице обычно разрешаются с помощью односвязного списка элементов, а по односвязному списку нельзя двигаться назад. Итераторы unordered-контейнеров могут инвалидироваться только если произошло рехеширование при вставке. Ссылки и указатели никогда не инвалидируются.

Отдельно отметим функцию `erase` у ассоциативных контейнеров. У неё есть несколько перегруженных версий. Одна версия принимает ключ, другая — итератор удаляемого элемента, третья — диапазон итераторов. Разница будет для мультиконтейнеров: если какой-то ключ повторяется, то первая версия `erase` удалит все вхождения таких ключей, а вторая — только конкретные:

```c++
#include <unordered_map>

int main() {
    std::unordered_multimap<std::string, int> data = {
        {"a", 1},
        {"a", 2},
        {"a", 3},
        {"b", 4},
    };

    auto iter = data.find("a");
    if (iter != data.end()) {
        data.erase(iter);  // удаляем первое найденное вхождение с ключом "a"
    }

    data.erase("a");  // удаляем все остальные вхождения с ключом "a"
}
```