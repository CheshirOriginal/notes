В стандартной библиотеке C++ есть четыре мультиконтейнера:

- [`std::multimap`](https://en.cppreference.com/w/cpp/container/multimap) (в заголовочном файле `map`);
- [`std::multiset`](https://en.cppreference.com/w/cpp/container/multiset) (в заголовочном файле `set`);
- [`std::unordered_multimap`](https://en.cppreference.com/w/cpp/container/unordered_multimap) (в заголовочном файле `unordered_map`);
- [`std::unordered_multiset`](https://en.cppreference.com/w/cpp/container/unordered_multiset) (в заголовочном файле `unordered_set`).

Они аналогичны обычным ассоциативным контейнерам, которые мы рассматривали выше, но в мультиконтейнерах один и тот же ключ может встретиться несколько раз.

Пусть, например, мы хотим сохранять для каждого слова в текстовом файле его порядковый номер. Слова в тексте могут повторяться, поэтому воспользуемся контейнером `multimap`:

```c++
#include <iostream>
#include <map>

int main() {
    std::multimap<std::string, int> positions;

    std::string word;
    int position = 0;
    while (std::cin >> word) {
        positions.insert({word, position});
        ++position;
    }
}
```

В этом случае мы могли бы применить вместо `std::multimap<std::string, int>` контейнер `std::map<std::string, std::vector<int>>`. Разница будет в использовании и в накладных расходах.

Для обхода `multimap` потребуется один цикл, а для `map` с вектором — два вложенных цикла (по ключам и по элементам вектора для данного ключа). Вектор имеет накладные расходы на хранение метаинформации и резерва, а в `multimap` все данные будут храниться в одном сбалансированном дереве.

Наконец, итераторы `multimap` стабильны, а у вектора могут инвалидироваться. Применять `multimap` имеет смысл там, где повторы ключей сравнительно редки.