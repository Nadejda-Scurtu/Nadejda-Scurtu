#include <stdio.h>
#include <stdlib.h>

struct d_t
{
    int x{1}; // предварительная инициализация поля структуры
};

class DataType
{ 
    int x = 1; // предварительная инициализация поля структуры

    // принципиальное отличие class от struct в том, что поля по умолчанию не доступны вне структуры, если она - class
    // для имитации struct все поля необходимо включать под лейбл public 
public:
    // С++ позволяет писать функции внутри структур, такие функции называются "методами"
    void print()
    {
        printf("%p %d\n", this, x);
    }
};

void printa(const d_t * array, const int count) 
// pointer decay - явление характеризующееся потерей информации о 
// свойствах массива при передаче в функцию в качестве аргумента
{
    for (size_t i = 0; i < count; i++)
    {
        printf("%d ", array[i].x + 10);
    }
}

int main(int argc, char const *argv[])
{
    // "куча" - heap (динамическая память)
    // "стопка" - stack (статическая память)

    // переменная создается на stack
    // значение хранится по адресу переменной:
    d_t first = {2}; // инициализация значения первого поля структуры

    // переменная создается на stack, т.к. это указатель
    // значение необходимо создать на heap:
    d_t * second = new d_t {2}; // инициализация первого поля структуры в момент выделения памяти

    // вывод значений поля структуры по значению (.) и по ссылке (->)
    printf("%d %d", first.x, second->x);

    delete second; // освобождение памяти
    second = nullptr; // обнуление адреса, чтобы избежать проблемы c dangling pointer
    // dangling pointer - явление характеризующееся присутствием неиспользуемого адреса в программе

    DataType third; // нельзя инициализировать, так как поле с данными скрыто
    third.print(); // вызов функции из объекта

    // статический массив структур
    d_t a [] = {{1}, {2}, {3}, {4}, {5}};

    printf("\n");
    printa(a, 5);

    // динамический массив структур
    // C-стиль, используя стандартную библиотеку:
    d_t * b = (d_t *)malloc(sizeof(d_t) * 5);
    // С++ использует оператор new:
    d_t * c = new d_t [5];

    // освобождение памяти в случае стандартной библиотеки:
    free(b);
    // освобождение памяти для встроенного оператора (обратите внимание на [] в операторе):
    delete[] c;

    // в целом, избегайте смешивания этих двух способов выделения/освобождения памяти
    // используйте их только в паре друг с другом
    // если пользуетесь конструкциями, которые существуют только в С++, обязательно пользуйтесь парой new/delete
}
