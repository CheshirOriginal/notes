**При работе с наследованием ваши деструкторы всегда должны быть виртуальными**. Рассмотрим следующий пример:

```cpp
#include <iostream>

class Parent
{
public:
    ~Parent() // примечание: Деструктор не виртуальный
    {
        std::cout << "Calling ~Parent()" << std::endl;
    }
};

class Child: public Parent
{
private:
    int* m_array;
public:
    Child(int length)
    {
        m_array = new int[length];
    }

    ~Child() // примечание: Деструктор не виртуальный
    {
        std::cout << "Calling ~Child()" << std::endl;
        delete[] m_array;
    }
};

int main()
{
    Child *child = new Child(7);
    Parent *parent = child;
    delete parent;
    return 0;
}
```

Поскольку `parent` является указателем класса Parent, то при его уничтожении компилятор будет смотреть, является ли деструктор класса Parent виртуальным. Поскольку это не так, то компилятор вызовет только деструктор класса Parent.

Результат выполнения программы:

```
Calling ~Parent()
```

Тем не менее, нам нужно, чтобы delete вызывал деструктор класса Child (который, в свою очередь, будет вызывать деструктор класса Parent), иначе `m_array` не будет удален. Это можно выполнить, сделав деструктор класса Parent виртуальным:

```cpp
#include <iostream>

class Parent
{
public:
    virtual ~Parent() // примечание: Деструктор виртуальный
    {
        std::cout << "Calling ~Parent()" << std::endl;
    }
};

class Child: public Parent
{
private:
    int* m_array;
public:
    Child(int length)
    {
        m_array = new int[length];
    }

    virtual ~Child() // примечание: Деструктор виртуальный
    {
        std::cout << "Calling ~Child()" << std::endl;
        delete[] m_array;
    }
};

int main()
{
    Child *child = new Child(7);
    Parent *parent = child;
    delete parent;
    return 0;
}
```

Результат выполнения программы:

```
Calling ~Child()   
Calling ~Parent()
```

==**Правило: При работе с наследованием ваши деструкторы должны быть виртуальными.**==


