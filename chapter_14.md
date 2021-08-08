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

## Выражения, следующие в произвольном порядке

Иногда одно выражение не зависит от другого и из него не следует.

В таких случаях всё равно **старайтесь размещать взаимосвязанные выражения как можно ближе друг к другу.** Они могут связаны, так как работают с одними и теми же данными, выполняют схожие задачи и так далее.

После того, как вы сгруппируете взаимосвязанные выражения, может оказаться, что эти группы не зависят друг от друга. В таком случае полезно выделить каждую группу выражений в отдельный метод.

```python
# Плохо :(
marketing_data MarketingData = MarketingData()
sales_data SalesData = SalesData()

marketing_data.compute_quarterly()
sales_data.compute_quarterly()

marketing_data.compute_annual()
sales_data.compute_annual()

# Лучше :)
marketing_data = MarketingData()
marketing_data.compute_quarterly()
marketing_data.compute_annual()

sales_data = SalesData()
sales_data.compute_quarterly()
sales_data.compute_annual()
```
