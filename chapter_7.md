# Глава 7. Высококачественные методы

> ...методы — величайшее изобратение в области компьютерных наук. Методы облегчают чтение и понимание программ в большей степени, чем любая другая возможность языка программирования (с. 159)

## Разумные причины создания методов

- **Самая важная причина** — снижение сложности (без абстрагирующей силы методов сложные программы было бы невозможно охватить умом)
- **Самая популярная причина** — желание избежать дублирования кода
- Удачно названный метод формирует понятную промежуточную абстракцию, что облегчает чтение и понимание кода
- Небольшой и понятный метод можно легко переопределить, что поможет при работе с наследованием
- Сокрытие порядка выполнения действий
- Сокрытие операций на указателями
- Улучшение портируемости за счёт изолирования непортируемого кода в отдельных методах
- Упрощение сложных логический проверок за счёт сокрытия деталей проверки в методе с простым, описательным именем
- Методы позволяют выполнять оптимизацию кода в одном месте, повышая быстродействие всех фрагментов, которые его используют

Но методы используют и для простых операций. На практике технически простая операция может быть сложной для понимания, а метод с понятным именем решает эту проблему (например, метод `convert_pixels_to_points()` при чтении кода будет понятнее, чем его содержимое, даже если это всего одна строка кода). К тому же простые операции имеют свойство усложняться со временем.

## Проектирование на уровне методов

На уровне методов важнейшим принципом проектирования является стремление к функциональной связности (cohesion).

> ...цель в том, чтобы каждый метод эффективно решал одну задачу и больше ничего не делал (с. 164)

Существует несколько видов связности. Некоторые из них неприемлимы и требуют исправления.

| Вид                | Описание                                                                                                                      | Оценка | Лечение                                                                                              |
| ------------------ | ----------------------------------------------------------------------------------------------------------------------------- | ------ | ---------------------------------------------------------------------------------------------------- |
| `Функциональная`   | Метод выполняет одну и только одну операцию                                                                                   | 😆     | —                                                                                                    |
| `Последовательная` | Метод содержит операции, которые выполняются в определённой порядке, и используют данные предыдущих этапов                    | 😃     | Разделить на несколько методов                                                                       |
| `Коммуникационная` | Метод содержит операции, которые используют одни и те же данные, но больше никак друг с другом не связаны                     | 😃     | Разделить на несколько методов                                                                       |
| `Временная`        | Операции объединены в метод, потому что выполняются в один интервал времени                                                   | 😃     | Заменить операции на вызов отдельных методов                                                         |
| `Процедурная`      | Операции объединены в метод, потому что выполняются друг за другом, но не связаны одной задачей или же решают её не полностью | 😞     | Вынести операции в отдельные методы; на их основе создать метод, который будет решать задачу целиком |
| `Логическая`       | Метод содержит несколько операций, а выбор выполняемой операции происходит на основе условной логики                          | 😞     | Разделить на несколько методов                                                                       |
| `Случайная`        | Выполняемыми в методе операциями никак не связаны                                                                             | 😡     | Спроектировать и реализовать новые методы с нуля                                                     |