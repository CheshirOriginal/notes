Контейнеры [`std::set`](https://en.cppreference.com/w/cpp/container/set) и [`std::unordered_set`](https://en.cppreference.com/w/cpp/container/unordered_set) похожи на `map` и `unordered_map` по внутреннему устройству, но они хранят только ключи, без ассоциированных значений. Вот как можно выписать повторяющиеся слова текста в алфавитном порядке (по одному разу каждое):

```c++
#include <iostream>
#include <set>
#include <string>
#include <unordered_set>

int main() {
    // здесь будем хранить все слова (каждое по одному разу)
    std::unordered_set<std::string> words;

    // здесь будем хранить повторяющиеся слова
    // используем set, а не unordered_set, чтобы потом напечатать их по алфавиту
    std::set<std::string> duplicate_words;

    std::string word;
    while (std::cin >> word) {
        if (words.contains(word)) {
            duplicate_words.insert(word);
        } else {
            words.insert(word);
        }
    }

    for (const auto& word : duplicate_words) {
        std::cout << word << "\n";
    }
}
```

Здесь мы применили функцию `contains`, которая появилась только в C++20. При использовании более старого стандарта нужно написать `if (words.find(word) != words.end())`.

Заметим, что при попадании в `else` мы ищем слово в `words` дважды: один раз для проверки в `contains`, а другой раз — в `insert`. Можно было бы обойтись только одним поиском, воспользовавшись тем, что `insert` [возвращает пару](https://en.cppreference.com/w/cpp/container/set/insert) из итератора на элемент и флажка с результатом поиска:

```c++
#include <iostream>
#include <set>
#include <string>
#include <unordered_set>

int main() {
    std::unordered_set<std::string> words;
    std::set<std::string> duplicate_words;
    std::string word;
    while (std::cin >> word) {
        auto [iter, has_been_inserted] = words.insert(word);
        if (!has_been_inserted) {
            duplicate_words.insert(word);
        }
    }
    for (const auto& word : duplicate_words) {
        std::cout << word << "\n";
    }
}
```

Название `set` происходит от математического понятия множества, где элементы хранятся только по одному разу. Однако никаких теоретико-множественных операций (объединения, пересечения, разности) у `set` и `unordered_set` не предусмотрено. 