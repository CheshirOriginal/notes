Односвязный список [std::forward_list](https://en.cppreference.com/w/cpp/container/forward_list) нужен там, где требуется сэкономить память на хранении ссылок на предыдущий узел. ==По такому контейнеру можно пройтись только вперёд, а вставка разрешена только в начало (или после указанного итератора).== Этот контейнер встречается в некоторых реализациях хеш-таблицы `unordered_map` для хранения элементов с одинаковыми хешами.

```c++
#include <forward_list>
#include <iostream>
#include <iterator>

int main() {
    std::forward_list<int> fl = {3, 42, 5};
    fl.push_front(2);
    // fl.push_back(10);  // ошибка компиляции!

    auto iter = std::next(fl.begin());
    iter = fl.erase_after(iter);
    fl.insert_after(iter, 4);

    for (int x : fl) {
        std::cout << x << "\n";  // 2 3 5 4
    }
}
```

Функции [`insert_after`](https://en.cppreference.com/w/cpp/container/forward_list/insert_after) и [`erase_after`](https://en.cppreference.com/w/cpp/container/forward_list/erase_after) своими названиями подчёркивают своё отличие от `insert` и `erase` у других контейнеров, работающих с текущей позицией. В односвязном списке не получится вставить элемент на текущую позицию, как это делает `insert` в `std::list`, поскольку к предыдущему элементу невозможно обратиться, а его ссылку на следующий элемент надо поправить.