# Нестандартные управляющие структуры

## Множественные возвраты из метода

Большинство языков поддерживают возможность возврата управления после частичного выполнения функции – обычно это делает оператор `return` или его аналоги.

Основные принципы применения:

- Используйте множественный `return`, если при получении определённого результата, можно сразу вернуть управление наверх, не дожидаясь завершения полного выполнения кода функции (в таких случаях `return` обычно повышает читаемость)

```python
# Хорошо
class ComparisonOption(Enum):
    EQUAL = 0
    FIRST = 1
    SECOND = 2

def compare(first_integer: int, second_integer: int) -> ComparisonOption:
    if (first_integer > second_integer):
        return ComparisonOption.FIRST

    if (second_integer > first_integer):
        return ComparisonOption.SECOND

    return ComparisonOption.EQUAL
```

- Упрощайте сложную обработку множества ошибок с помощью *сторожевых операторов* (досрочного `return` или его аналогов)

```python
logger = logging.getLogger(__name__)

# Плохо :(
def do_something_with_file(file, encryption_key):
    if file.name_is_valid():
        if file.open():
            if encryption_key.is_valid():
                if file.decrypt():
                    do_something()
                else:
                    logger.error("Cannot decrypt file")
            else:
                logger.error("Encryption key is not valid")
        else:
            logger.error("Cannot open file")
    else:
        logger.error("Name is not valid")


# Лучше!
def do_something_with_file(file, encryption_key):
    if not file.name_is_valid():
        logger.error("Name is not valid")
        return

    if not file.open():
        logger.error("Cannot open file")
        return

    if not encryption_key.is_valid():
        logger.error("Encryption key is not valid")
        return
    
    if not file.decrypt():
        logger.error("Cannot decrypt file")
        return

    do_something()
```

- Количество операторов `return` следует минимизировать, добавляйте очередной `return` только если это улучшает читабельность