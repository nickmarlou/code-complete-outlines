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

- Разбивайте сложные проверки на части при помощи новых логических переменных

```python
# Плохо :(
if (order.status == ORDER_STATUSES.NEW && (product.is_available && product.count >= order.count || product.has_infinite_count)) {
    order.process()
}

# Лучше!
order_is_new = order.status == ORDER_STATUSES.NEW
products_are_enough = (product.count >= order.count) || product.has_infinite_count
if (order_is_new && products_are_enough) {
    order.process()
}
```

- Размещайте сложные выражения в логических функциях, чтобы эта проверка не мешала воспринимать основной ход алгоритма

```python
# Ещё лучше!
if (check_order_is_ready_to_proces(order, product)) {
    order.process()
}

def check_order_is_ready_to_process(order, product) -> bool:
    order_is_new = order.status == ORDER_STATUSES.NEW
    products_are_enough = (product.count >= order.count) || product.has_infinite_count
    order_is_ready_to_process = order_is_new && products_are_enough
    return order_is_ready_to_process
```

- Используйте для проверки сложных условий с несколькими переменными таблицы решений, а не операторы `if` или `case`
