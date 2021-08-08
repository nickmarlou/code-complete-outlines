# Организация последовательного кода

## Выражения, следующие в определённом порядке

Проще всего организовать код, выражения в котором должны выполняться в определённом порядке. **Основная идея: ясно показывать как одно выражение зависит от другого так, чтобы это помогало понять и объясняло избранный порядок выполнения.**

Основные принципы:

- Организуйте код так, чтобы зависимости между выражениями были очевидны (за счёт этого будет хорошо понятно в каком порядке выражения должны быть выполнены)

```python
# Плохо :(
revenue.compute_monthly()
revenue.compute_quarterly()
revenue.compute_annual()

# Хорошо :)
monthly_revenue = revenue.compute_monthly()
quarterly_revenue = revenue.compute_quarterly(monthly_revenue)
annual_revenue = revenue.compute_annual(quarterly_revenue)
```

- Называйте методы так, чтобы зависимости были очевидны

```python
monthly_revenue = revenue.compute_monthly()
quarterly_revenue = revenue.compute_quarterly_from_monthly(monthly_revenue)
annual_revenue = revenue.compute_annual_from_quarterly(quarterly_revenue)
```

- Документируйте неявные зависимости или допущения с помощью комментариев (это поможет компенсировать недостатки кода, если их невозможно устранить)

```python
class Revenue():
    def compute_monthly(self, start: Optional[datetime], end: Optional[datetime]) -> List[dict]:
        """
        Метод извлекает из базы данных и возвращает 
        ежемесячный доход за указанный период.

        Если `start` и `end` не переданы, то
        вовзращается доход за весь доступный период.
        """
        ...

revenue = Revenue()

monthly_revenue = revenue.compute_monthly()
quarterly_revenue = revenue.compute_quarterly_from_monthly(monthly_revenue)
annual_revenue = revenue.compute_annual_from_quarterly(quarterly_revenue)
```

- Проверяйте готовы ли зависимости, необходимые для выполнения выражения, и возвращайте ошибки, если нет (это усложнит код и должно быть целесообразно)
