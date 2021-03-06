# Табличные методы

Табличный метод — это схема, позволяющая искать информацию в таблице, а не использовать для этого логические выражения, такие как `if` и `case`.

Чем сложнее логические построения, тем привлекательнее табличные методы.

## Применение табличных методов

Допустим, вам нужно определить сколько дней в определённом месяце (и у вас нет инструмента `calendar.monthrange` или аналогичного).

Если использовать `if`, это будет выглядить так:

```python
def get_days_count_in_month(month: int) -> int:
    if (month == 1):
        return 31
    if (month == 2):
        return 28

    ...

    if (month == 12):
        return 31
```

Код получается громоздким и неудобным для чтения. Будет гораздо лучше, если использовать табличный метод:

```python
def get_days_count_in_month(month: int) -> int:
    days_in_months = {
        1: 31,
        2: 28,
        ...
        12: 31
    }

    return days_in_months[month]
```

При применении табличных методов, важно отвечать на два вопроса:

* Как будет выполняться поиск записей в таблице?
  * [Прямой доступ](#таблицы-с-прямым-доступом)
  * [Индексированный доступ](#таблицы-с-индексированным-доступом)
  * [Ступенчатый доступ](#таблицы-со-ступенчатым-доступом)
* Что хранить в таблице?
  * Данные
  * Выражения (код)

## Таблицы с прямым доступом

Чтобы найти в таблице необходимую информацию, вы можете непосредственно выбрать нужную запись по ключу, например:

```python
days_in_months = {
    1: 31,
    2: 28,
    ...
    12: 31
}

days_in_november = days_in_months[11]
```

### Подгонка значений ключа

Не всегда данные годятся для того, чтобы использовать их в качестве ключа. Например, вам нужно рассчитать ставку для страховки и эта ставка зависит от возраста:

- от 0 до 17 лет – 1%,
- от 18 до 65 лет – от 2% до 10%, 
- более 66 лет – 11%.

Как организовать хранения таких ключей в таблице?

#### Дублирование

Продублировать ставку для возрастов от 0 до 17 лет и возрастов старше 66. Это очень простое решение, но оно напрасно потратит память.

#### Преобразование ключа

Использовать функцию для преобразования ключа. Например, `max(min(66, age), 17)`, чтобы преобразовать все значение от 0 до 17 к `17`, а значения более 66 к `66`.

> ...поместите операции, трансформирующие данные в ключ, в отдельный метод (с. 417)

## Таблицы с индексированным доступом

Иногда простого преобразования недостаточно для перехода от таких данных, как `age` к значению ключа.



