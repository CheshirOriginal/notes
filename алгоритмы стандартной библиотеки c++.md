В этом параграфе мы обсудим _алгоритмы стандартной библиотеки_. Они представляют из себя шаблонные функции для обработки последовательностей: подсчёта, поиска, сортировки и т. д. Такие функции, как правило, принимают на вход два итератора, которые ограничивают рассматриваемый диапазон.
## Пары итераторов

В функции стандартной библиотеки часто требуется передать два итератора, которые ограничивают диапазон элементов. К ним предъявляются такие требования:

- это должны быть итераторы одного и того же контейнера;
- от первого итератора ко второму можно перейти, применяя оператор `++`.

Предполагается, что второй итератор при этом ссылается _за_ последний элемент диапазона. Его нельзя разыменовывать оператором `*`, даже если он ссылается на валидный элемент.

==Можно рассматривать пару итераторов как математический полуинтервал, где левый конец включается в диапазон, а правый — нет.==

Все контейнеры имеют функции `begin` и `end`. Они возвращают итераторы, указывающие на начальный и _за_ последний элемент всей последовательности элементов контейнера.

Контейнеры, которые допускают обход в обратном порядке (все кроме `forward_list` и `unordered`-контейнеров), имеют также функции `rbegin` и `rend`. Они возвращают _обратные итераторы_. С их помощью можно проходить по элементам из конца в начало.

Однако можно строить и другие диапазоны. Например, вот так можно отсортировать по возрастанию «середину» вектора кроме первого и последнего элемента:

```cpp
#include <algorithm>
#include <vector>

int main() {
    std::vector<int> data = {100, 42, 17, 80, 20, 0};
    std::sort(data.begin() + 1, data.end() - 1);  // 100, 17, 20, 42, 80, 0
}
```

Впрочем, так можно делать только если мы уверены, что в векторе есть хотя бы два элемента. В противном случае диапазон получится некорректным и поведение программы будет неопределённым.

Заметим, что с помощью итераторов реализован цикл range-based for. В самом деле, конструкция

```cpp
for (const auto& element : container) {
    // делаем что-то с element
}
```

просто превращается компилятором в примерно такой код:

```cpp
for (auto iter = container.begin(); iter != container.end(); ++iter) {
    const auto& element = *iter;
    // делаем что-то с element
}
```

В C++20 для каждого алгоритма были добавлены так называемые [constrained-версии](https://en.cppreference.com/w/cpp/algorithm/ranges). Эти версии вместо пары итераторов принимают диапазон (range), оформленный в виде отдельного объекта. В частности, в роли такого объекта может выступать сам контейнер. Это позволяет вызывать сортировку, например, так:

```cpp
#include <algorithm>
#include <vector>

int main() {
    std::vector<int> data = {100, 42, 17, 80, 20, 0};
    std::ranges::sort(data);  // 0, 17, 20, 42, 80, 100
}
```
## Подсчёт

Рассмотрим, пожалуй, самый простой пример — алгоритм [`std::count`](https://en.cppreference.com/w/cpp/algorithm/count). Эта функция подсчитывает, сколько элементов последовательности равны заданному. Конечно, такую задачу можно решить с помощью банального цикла:

```cpp
#include <iostream>
#include <vector>

int main() {
    std::vector<int> v = {2, 7, 1, 8, 2, 8};

    // сколько в последовательности восьмёрок?
    int counter = 0;
    for (int elem : v) {
        if (elem == 8) {
            ++counter;
        }
    }
    std::cout << counter << "\n";  // 2
}
```

Однако использование готового стандартного алгоритма всегда предпочтительнее:

```cpp
#include <algorithm>
#include <iostream>
#include <vector>

int main() {
    std::vector<int> v = {2, 7, 1, 8, 2, 8};
    std::cout << std::count(v.begin(), v.end(), 8) << "\n";  // 2
}
```

Функцию `std::count` можно применять к итераторам любого последовательного контейнера. Применим её, например, к списку:

```cpp
#include <algorithm>
#include <iostream>
#include <list>

int main() {
    std::list<int> v = {2, 7, 1, 8, 2, 8};
    std::cout << std::count(v.begin(), v.end(), 8) << "\n";
}
```

Такие функции как `std::count` позволяют упростить код и сделать его более лаконичным и выразительным. Важно понимать, что это самые обыкновенные шаблонные функции, написанные на C++. Вы даже можете посмотреть их реализацию, если откроете заголовочный файл `algorithm`. Давайте попробуем в качестве упражнения написать такую функцию самостоятельно.

```cpp
template <typename Iter, typename Value>  // два шаблонных параметра: тип итератора и тип эталонного элемента
int count(Iter first, Iter last, const Value& value) {
    int counter = 0;
    for (Iter iter = first; iter != last; ++iter) {
        if (*iter == value) {
            ++counter;
        }
    }
    return counter;
}
```

Обратите внимание, что параметры `first` и `last` передаются в функцию по значению. Это стандартное соглашение: считается, что итераторы — лёгкие элементы, которые можно дёшево копировать. А вот про тип `Value` мы заранее ничего не знаем, поэтому передаём параметр `value` по константной ссылке. Так как `first` передаётся по значению, то этот параметр можно менять внутри функции. Поэтому часто можно встретить такую реализацию:

```cpp
template <typename Iter, typename Value>
int count(Iter first, Iter last, const Value& value) {
    int counter = 0;
    // Смело меняем first, это всё равно копия переданного аргумента:
    while (first != last) {
        if (*first == value) {
            ++counter;
        }
        ++first;
    }
    return counter;
}
```

На самом деле тип возвращаемого значения этой функции не в точности `int`, а `typename iterator_traits<Iter>::difference_type`, построенный по типу итератора.
## Поиск

Алгоритм [`std::find`](https://en.cppreference.com/w/cpp/algorithm/find) ищет последовательным перебором первое вхождение элемента, равного заданному. Результатом является итератор, который указывает на найденный элемент. Если же ничего не найдено, то возвращается правый конец переданного полуинтервала.

```cpp
#include <algorithm>
#include <deque>
#include <iostream>

int main() {
    std::deque<int> d = {3, 14, 15, 92, 6};

    // Такой элемент есть, мы его точно найдём
    auto iter1 = std::find(d.begin(), d.end(), 15);
    // Итераторы дека можно вычитать, напечатается индекс найденного элемента
    std::cout << (iter1 - d.begin()) << "\n";

    auto start = d.begin();
    // К итераторам дека и вектора можно прибавлять целые числа
    auto end = start + 3;
    // Полуинтервал [start; end) теперь ограничивает подпоследовательность 3, 14, 15

    auto iter2 = std::find(start, end, 19);
    if (iter2 == end) {
        std::cout << "No such element!\n";
    } else {
        std::cout << *iter2 << "\n";
    }
    // Напечатает No such element
}
```

Вот его возможная реализация:

```cpp
template <typename Iter, typename Value>
Iter find(Iter first, Iter last, const Value& value) {
    while (first != last) {
        if (*first == value) {
            return first;
        }
        ++first;
    }
    return last;
}
```

Имеется также алгоритм [`std::search`](https://en.cppreference.com/w/cpp/algorithm/search), который ищет не отдельный элемент, а подпоследовательность элементов.
## Алгоритмы и встроенные функции

У [ассоциативных контейнеров](https://education.yandex.ru/handbook/cpp/article/associative-containers) есть встроенная функция `find`, которая возвращает похожий по смыслу итератор. Разберёмся, в чём различие между общим алгоритмом `find` и встроенной функцией. Алгоритм `std::find` написан в общем виде. Он работает только с итераторами и ничего не знает о физическом устройстве контейнера. Поэтому он ищет элемент последовательным перебором с линейной сложностью O(n). Но в ассоциативных контейнерах можно найти элемент быстрее: за O(log⁡n) в `set` и `map` и в среднем за O(1) в `unordered_set` и `unordered_map`.

```cpp
#include <algorithm>
#include <set>

int main() {
    std::set<int> numbers = {2, 3, 5, 7, 11, 13, 17, 19};

    // Предпочтительный способ:
    auto iter1 = numbers.find(15);

    // Допустимо, но неэффективно!
    auto iter2 = std::find(numbers.begin(), numbers.end(), 15);
}
```

==Тут мы встречаемся с общей идеей, заложенной в дизайн стандартной библиотеки C++: если некоторый алгоритм имеет более эффективную реализацию для конкретного контейнера, то он будет реализован у этого контейнера в виде встроенной функции. Наоборот, если возможна лишь общая реализация, то специальной встроенной функции не будет.== Поэтому, например, у вектора и дека нет своей встроенной функции `find`.

Заметим, что найти элемент по ключу в контейнерах `map` и `unordered_map` с помощью алгоритма `std::find` вообще не получится. Дело в том, что итераторы таких контейнеров ссылаются на пару из константного ключа и значения, а не просто на ключ. А значение как правило нам заранее неизвестно: его как раз обычно и требуется получить по ключу.

```cpp
#include <algorithm>
#include <map>
#include <utility>

int main() {
    std::map<int, int> m = {
        {1, 30},
        {2, 28},
        {3, 31},
        // ...
    };

    auto it1 = m.find(12);   // правильный способ поиска по ключу
    auto it2 = std::find(m.begin(), m.end(), 12);  // не скомпилируется!

    std::pair<const int, int> value = {12, 31};
    // Скомпилируется, но неэффективно и бессмысленно:
    auto it3 = std::find(m.begin(), m.end(), value);
}
```

## Алгоритмы, принимающие предикат

В алгоритмах `count` и `find` мы передавали третьим аргументом конкретное значение, которое требовалось найти в последовательности. Но иногда бывает нужно найти любое значение, удовлетворяющее некоторому условию. Такое условие можно оформить в виде функции, получающей на вход элемент последовательности и возвращающей логический ответ (`true`, если элемент подходит и `false`, если не подходит). Такие функции называют _предикатами_. Для работы с ними имеются алгоритмы, в названии которых добавляется суффикс `_if`.

Например, подсчитаем, сколько заглавных латинских букв в строке. Передадим в функцию `count_if` предикат, оформленный в виде лямбда-функции:

```cpp
#include <algorithm>
#include <iostream>
#include <string>

int main() {
    std::string s = "iPhone SE";

    std::cout << std::count_if(
        s.begin(),
        s.end(),
        [](char c) {
            return 'A' <= c && c <= 'Z';
        }
    ) << "\n";  // 3
}
```

Можно было бы внутри лямбда-функции вместо двойного неравенства воспользоваться функцией [`std::isupper`](https://en.cppreference.com/w/cpp/string/byte/isupper). К сожалению, просто передать `std::isupper` в `std::count_if` не получится: компилятор не сможет без конкретных аргументов отличить эту функцию от [другой перегруженной версии](https://en.cppreference.com/w/cpp/locale/isupper).

Напишем для примера собственную реализацию алгоритма `find_if`. Тип предиката сделаем просто шаблонным параметром.

```cpp
template <typename Iter, typename Predicate>
Iter find_if(Iter first, Iter last, Predicate p) {
    while (first != last) {
        if (p(*first)) {  // применяем предикат
            return first;
        }
        ++first;
    }
    return last;
}
```

Отметим [семейство алгоритмов](https://en.cppreference.com/w/cpp/algorithm/all_any_none_of) `std::all_of`, `std::any_of` и `std::none_of`, также принимающих предикаты. Их смысл понятен из названия. Например, `any_of` проверяет, что какой-то элемент последовательности удовлетворяет предикату. Типичная реализация `any_of` сводится к вызову `find_if` и сравнению результата с итератором `last`.
## Алгоритмы, модифицирующие последовательность

Рассмотрим алгоритм [`std::reverse`](https://en.cppreference.com/w/cpp/algorithm/reverse). Он переставляет элементы последовательности в обратном порядке:

```cpp
#include <algorithm>
#include <iostream>
#include <string>

int main() {
    std::string s = "No lemon, no melon!";
    std::reverse(s.begin(), s.end());
    std::cout << s << "\n";  // !nolem on ,nomel oN
}
```

Его возможная реализация выглядит так:

```cpp
template <typename Iter>
void reverse(Iter first, Iter last) {
    while (first != last) {
        --last;
        if (first == last) {
            break;
        }
        std::swap(*first, *last);
        ++first;
    }
}
```

Сложность тут в том, что `last` указывает за последний элемент, и этот итератор надо аккуратно подвинуть назад, прежде чем переставлять элементы. Здесь вызывается функция [`std::swap`](https://en.cppreference.com/w/cpp/algorithm/swap), реализацию которой мы уже писали в [параграфе «Функции»](https://education.yandex.ru/handbook/cpp/article/functions).

Важно понимать, что алгоритмы работают с итераторами, и поэтому они ничего не знают о физическом способе хранения элементов в контейнерах. Алгоритмы знают лишь о логическом порядке перебора элементов. Поэтому алгоритмы не могут физически что-либо удалить из контейнера или добавить в него. Лучшее, что они могут сделать, — это переупорядочить элементы в последовательности.

Рассмотрим алгоритм [`std::unique`](https://en.cppreference.com/w/cpp/algorithm/unique). Этот алгоритм переставляет элементы так, чтобы в последовательности не было подряд идущих дубликатов. Уменьшить размер последовательности алгоритм не может. Поэтому в конце последовательности остаётся некоторый набор ненужных элементов. Их можно потом явно удалить, вызвав функцию `erase` у контейнера:

```cpp
#include <algorithm>
#include <vector>

int main() {
    std::vector<int> v = {5, 5, 3, 2, 2, 5, 9, 1};
    auto iter = std::unique(v.begin(), v.end());
    // В векторе окажется 5, 3, 2, 5, 9, 1, 9, 1
    //                                      ^ iter будет указывать сюда
    v.erase(iter, v.end());  // удаляем хвост из ненужных элементов
}
```

Обратите внимание, что третья пятёрка не удалилась, потому что она не расположена рядом с другими пятёрками. Обычно `std::unique` применяют вместе с `std::sort`, чтобы одинаковые элементы оказались рядом:

```cpp
std::sort(v.begin(), v.end());
v.erase(std::unique(v.begin(), v.end()), v.end());
```

Среди других алгоритмов, модифицирующих последовательность, имеются

- [`std::fill`](https://en.cppreference.com/w/cpp/algorithm/fill) и [`std::generate`](https://en.cppreference.com/w/cpp/algorithm/generate) (заполнение заданными значениями),
- [`std::rotate`](https://en.cppreference.com/w/cpp/algorithm/rotate) (циклический сдвиг),
- [`std::replace`](https://en.cppreference.com/w/cpp/algorithm/replace) (замена элементов),
- [`std::remove`](https://en.cppreference.com/w/cpp/algorithm/remove) (переупорядочивание элементов, чтобы указанный элемент не встречался в начале),
- [`std::shuffle`](https://en.cppreference.com/w/cpp/algorithm/shuffle) (случайная перестановка элементов).
## Запись в другую последовательность

Алгоритм [`std::copy`](https://en.cppreference.com/w/cpp/algorithm/copy) копирует содержимое одной последовательности в другую последовательность. Он принимает три аргумента: обычную пару итераторов, задающих входной диапазон, и итератор, обозначающий начало выходного диапазона. Четвёртый аргумент не нужен: никаких проверок корректности всё равно не делается.

В этом примере мы копируем диапазон элементов в обратном порядке из вектора в список:

```cpp
#include <algorithm>
#include <iostream>
#include <list>
#include <vector>

int main() {
    std::vector<int> v = {3, 14, 15, 92, 6};
    std::list<int> l;
    l.resize(v.size());  // теперь в списке l 5 нулей

    std::copy(v.rbegin(), v.rend(), l.begin());

    for (int x : l) {
        std::cout << x << " ";
    }
    std::cout << "\n";  // 6 92 15 14 3
}
```

В контейнере, в который мы копируем элементы, должно быть достаточно места, чтобы они поместились. Если бы мы не вызвали `resize` для списка `l`, то программа попала бы в неопределенное поведение!

```cpp
// Вот возможная реализация алгоритма copy:
template <typename InputIter, typename OutputIter>
OutputIter copy(InputIter first, InputIter last, OutputIter out) {
    while (first != last) {
        *out = *first;
        ++first;
        ++out;
    }
    return out;
}
```

Обратите внимание, что шаблонные типы входных и выходных итераторов, вообще говоря, разные. Функция возвращает выходной итератор, указывающий _за_ последний записанный элемент. Этот итератор позволяет потом физически удалить лишние элементы выходной последовательности, которые больше не нужны:

```cpp
#include <algorithm>
#include <list>
#include <vector>

int main() {
    std::vector<int> v = {3, 14, 15, 92, 6};
    std::list<int> l(10);  // 10 нулей

    auto iter = std::copy(v.begin(), v.end(), l.begin());
    // 3 14 15 92 6 0 0 0 0 0, iter указывает на первый ноль

    l.erase(iter, l.end());  // 3 14 15 92 6
}
```

В стандартной библиотеке для модифицирующих алгоритмов есть версии с суффиксом `_copy` в имени, которые ведут себя похожим образом. Вместо модификации исходной последовательности они записывают результат в отдельную выходную последовательность. В ней должно быть достаточно места, чтобы результат поместился. Вот пример использования функции [`std::replace_copy`](https://en.cppreference.com/w/cpp/algorithm/ranges/replace_copy):

```cpp
#include <algorithm>
#include <iostream>
#include <string>
#include <vector>

int main() {
    std::vector<std::string> words1 = {"a", "cat", "and", "a", "dog"};
    std::vector<std::string> words2(words1.size());  // 5 пустых строк

    const std::string from = "a";
    const std::string to = "the";
    std::replace_copy(words1.begin(), words1.end(), words2.begin(), from, to);

    for (const auto& word : words1) {
        std::cout << word << " ";  // a cat and a dog
    }
    std::cout << "\n";

    for (const auto& word : words2) {
        std::cout << word << " ";  // the cat and the dog
    }
    std::cout << "\n";
}
```
## Адаптеры для вставки

В стандартной библиотеке есть конструкции, которые притворяются обычными итераторами, но на самом деле не являются таковыми. Например, шаблонная функция [`std::back_inserter`](https://en.cppreference.com/w/cpp/iterator/back_inserter) принимает последовательный контейнер и возвращает _адаптер_, который ведёт себя как итератор, но при попытке записи в него добавляет элемент в контейнер через `push_back`. Такие адаптеры удобно использовать с модифицирующими алгоритмами.

```cpp
#include <algorithm>
#include <iterator>
#include <list>
#include <vector>

int main() {
    std::vector<int> v = {3, 14, 15, 92, 6};
    std::list<int> l;  // список пустой!
    std::copy(v.begin(), v.end(), std::back_inserter(l));  // 3 14 15 92 6
}
```

Также есть функции [`std::front_inserter`](https://en.cppreference.com/w/cpp/iterator/front_inserter) и [`std::inserter`](https://en.cppreference.com/w/cpp/iterator/inserter). Последняя кроме контейнера принимает ещё и итератор, перед которым должна производиться вставка. Разумеется, этот итератор не должен инвалидироваться в процессе работы алгоритма.
## Сортировка

Мы уже знакомы с `std::sort`. Заметим, что этот алгоритм [нестабильный](https://en.wikipedia.org/wiki/Sorting_algorithm#Stability): после сортировки порядок одинаковых элементов может измениться. Это может быть заметно, например, во время сортировки вектора структур при сравнении по какому-то ключевому полю. В стандартной библиотеке имеется чуть менее эффективная функция [`stable_sort`](https://en.cppreference.com/w/cpp/algorithm/stable_sort) с асимптотической сложностью O(n(log⁡n)^2), сохраняющая порядок эквивалентных элементов.

Важно заметить, что алгоритмы сортировки требуют, чтобы на входе были _итераторы произвольного доступа_. Такие итераторы предоставляют контейнеры с индексами — `vector`, `string`, `array`, `deque`. К итераторам этих контейнеров можно прибавлять целые числа и их можно вычитать друг из друга. В сортировке это используется для быстрого обращения к элементу в середине последовательности.

Вызов общего алгоритма сортировки с итераторами списка просто не скомпилируется. Однако, как и с функцией `find` у ассоциативных контейнеров, у списков есть [встроенная](https://en.cppreference.com/w/cpp/container/list/sort) функция `sort`. Она использует сортировку слиянием, которая чуть менее эффективна на практике, но имеет ту же асимптотическую сложность O(nlog⁡n), что и `std::sort`.

```cpp
#include <algorithm>
#include <list>

int main() {
    std::list<int> data = {3, 14, 15, 92, 6};
    std::sort(data.begin(), data.end());  // не скомпилируется!
    data.sort();  // OK, встроенная функция сортировки списка
}
```

Проверить, отсортирована ли последовательность, можно с помощью функции [`std::is_sorted`](https://en.cppreference.com/w/cpp/algorithm/is_sorted). А с помощью функции [`std::partial_sort`](https://en.cppreference.com/w/cpp/algorithm/partial_sort) можно построить только начало отсортированного массива, не сортируя хвост:

```cpp
#include <algorithm>
#include <vector>

int main() {
    std::vector<int> data = {3, 14, 15, 92, 6};
    std::partial_sort(data.begin(), data.begin() + 3, data.end());
    // первыми тремя элементами в векторе будут 3, 6, 14
}
```

## Алгоритмы бинарного поиска

К отсортированным последовательностям в контейнерах можно применять особый набор алгоритмов. Рассмотрим алгоритмы для [бинарного поиска](https://en.wikipedia.org/wiki/Binary_search_algorithm):

```cpp
#include <algorithm>
#include <iostream>
#include <vector>

int main() {
    // Отсортирован по возрастанию:
    std::vector<int> data = {1, 4, 5, 9, 9, 13, 47};

    // Элемент будет найден:
    if (std::binary_search(data.begin(), data.end(), 5)) {
        std::cout << "Found\n";
    } else {
        std::cout << "Not found\n";
    }
}
```

Если [`binary_search`](https://en.cppreference.com/w/cpp/algorithm/binary_search) возвращает `true` или `false`, то алгоритмы [`lower_bound`](https://en.cppreference.com/w/cpp/algorithm/lower_bound) и [`upper_bound`](https://en.cppreference.com/w/cpp/algorithm/upper_bound) возвращают итераторы. Алгоритм `lower_bound` возвращает итератор на _первый элемент, не меньший заданного_. Алгоритм `upper_bound` возвращает итератор на _первый элемент, больший заданного_:

```cpp
#include <algorithm>
#include <iostream>
#include <vector>

int main() {
    // Числа идут по возрастанию:
    std::vector<int> data = {1, 4, 5, 9, 9, 13, 47};

    auto iter1 = std::lower_bound(
        data.begin(), data.end(),
        8
    );  // *iter1 == 9

    auto iter2 = std::upper_bound(
        data.begin(), data.end(),
        47
    );  // iter2 == data.end()

    // все элементы исходной последовательности, такие, что 8 <= x <= 47,
    // попадут в полуинтервал [iter1, iter2)
    for (auto iter = iter1; iter != iter2; ++iter) {
        std::cout << *iter << " ";  // 9 9 13 47
    }
    std::cout << "\n";
}
```

В частности, пара `std::lower_bound(first, last, x)` и `std::upper_bound(first, last, x)` ограничивает диапазон элементов, эквивалентных `x`. Пару таких итераторов можно получить за один вызов из функции [`std::equal_range`](https://en.cppreference.com/w/cpp/algorithm/equal_range).

![[Pasted image 20251014201451.png]]

Как и в случае с `find`, у ассоциативных контейнеров есть собственные встроенные реализации функций [`lower_bound`](https://en.cppreference.com/w/cpp/container/map/lower_bound), [`upper_bound`](https://en.cppreference.com/w/cpp/container/map/upper_bound) и [`equal_range`](https://en.cppreference.com/w/cpp/container/map/equal_range).

Если последовательность не будет упорядочена, то результат работы всех этих функций может быть некорректным.

Формально эти функции можно применять и к итераторам списка, не имеющим произвольного доступа. В этом случае время работы будет не логарифмическим, а линейным.

## Теоретико-множественные алгоритмы

Над отсортированными последовательностями можно выполнять операции

- слияния ([`std::merge`](https://en.cppreference.com/w/cpp/algorithm/merge)),
- объединения ([`std::set_union`](https://en.cppreference.com/w/cpp/algorithm/set_union))
- пересечения ([`std::set_intersection`](https://en.cppreference.com/w/cpp/algorithm/set_intersection)),
- разности ([`std::set_difference`](https://en.cppreference.com/w/cpp/algorithm/set_difference)),
- [симметрической разности](https://en.wikipedia.org/wiki/Symmetric_difference) ([`std::set_symmetric_difference`](https://en.cppreference.com/w/cpp/algorithm/set_symmetric_difference)).

Также можно проверять вложения ([`std::includes`](https://en.cppreference.com/w/cpp/algorithm/includes)).

Эти алгоритмы работают за линейное время от суммарной длины последовательностей. Перечисленные операции соответствуют математическим [мультимножествам](https://en.wikipedia.org/wiki/Multiset), а не множествам, потому что элементы в последовательностях могут повторяться. Каждый элемент учитывается со своей кратностью.

Все эти операции (кроме `includes`) принимают пять аргументов:

- два итератора первого диапазона,
- два итератора второго диапазона,
- итератор выходной последовательности для записи ответа.

Элементы выходной последовательности также будут отсортированы.

```cpp
#include <algorithm>
#include <deque>
#include <iterator>
#include <list>
#include <vector>

int main() {
    std::vector<int> in1 = {1, 3, 5, 5, 7};
    std::list<int> in2 = {1, 1, 2, 3};
    std::deque<int> out;

    std::merge(
        in1.begin(), in1.end(),
        in2.begin(), in2.end(),
        std::back_inserter(out)
    );  // 1, 1, 1, 2, 3, 3, 5, 5, 7

    out.clear();
    std::set_union(
        in1.begin(), in1.end(),
        in2.begin(), in2.end(),
        std::back_inserter(out)
    );  // 1, 1, 2, 3, 5, 5, 7

    out.clear();
    std::set_intersection(
        in1.begin(), in1.end(),
        in2.begin(), in2.end(),
        std::back_inserter(out)
    );  // 1, 3

    out.clear();
    std::set_difference(
        in1.begin(), in1.end(),
        in2.begin(), in2.end(),
        std::back_inserter(out)
    );  // 5, 5, 7

    out.clear();
    std::set_symmetric_difference(
        in1.begin(), in1.end(),
        in2.begin(), in2.end(),
        std::back_inserter(out)
    );  // 1, 2, 5, 5, 7

    std::includes(
        in2.begin(), in2.end(),
        in1.begin(), in1.end()
    );  // false
}
```