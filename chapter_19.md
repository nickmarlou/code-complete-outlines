# Общие вопросы управления

## Логические выражения

### Использование `true` и `false` в логических проверках

Неявная проверка истинности логической переменной делает код более понятным и читаемым.

```python
# Плохо :(
while(done == False) {
    ...
}

# Лучше!
while(not done) {
    ...
}
```

### Упрощение сложных выражений

Сложные логические выражения следует упрощать. Для этого есть разные техники:

- **Разбивайте сложные проверки на части** при помощи новых логических переменных

```python
# Плохо :(
if order.status == ORDER_STATUSES.NEW && (product.is_available && product.count >= order.count || product.has_infinite_count):
    order.process()

# Лучше!
order_is_new = order.status == ORDER_STATUSES.NEW
products_are_enough = (product.count >= order.count) || product.has_infinite_count
if order_is_new && products_are_enough:
    order.process()
```

- **Размещайте сложные выражения в логических функциях**, чтобы эта проверка не мешала воспринимать основной ход алгоритма

```python
# Ещё лучше!
if check_order_is_ready_to_proces(order, product):
    order.process()

def check_order_is_ready_to_process(order, product) -> bool:
    order_is_new = order.status == ORDER_STATUSES.NEW
    products_are_enough = (product.count >= order.count) || product.has_infinite_count
    order_is_ready_to_process = order_is_new && products_are_enough
    return order_is_ready_to_process
```

- **Используйте для проверки сложных условий** с несколькими переменными **таблицы решений**, а не операторы `if` или `case`
### Составление позитивных логических выражений

Предпочитайте позитивные логические выражения негативным – они создают меньше путаницы. Вот несколько приёмов:

- В операторах `if` заменяйте негативные выражения позитивными

```python
# Плохо :(
if not order.payment.is_success:
    order.payment.notify_error()

# Лучше
is_payment_error = not order.payment.is_success
if is_payment_error:
    order.payment.notify_error()
```

- Упрощайте логические выражения с помощью [законов Деморгана](https://ru.wikipedia.org/wiki/%D0%97%D0%B0%D0%BA%D0%BE%D0%BD%D1%8B_%D0%B4%D0%B5_%D0%9C%D0%BE%D1%80%D0%B3%D0%B0%D0%BD%D0%B0)

```python
# Плохо :(
if not order.payment.is_success or not order.delivery_is_started:
    pass

# Лучше
if not (order.payment.is_success and order.delivery_is_started):
    pass
```

### Использование скобок для пояснения логических выражений

> Если вы благоразумны, то не будете полагаться на собственную или чью-то еще способность правильно запомнить приоритет вычислений (с. 431)

Чтобы сделать сложные логические выражения проще для чтения, следует использовать скобки.

Например, есть условие:

```python
if (a < b == c == d)
```

Его лучше переписать со скобками:

```python
if ((a < b) == (c == d))
```
