[Курс "Базы данных"](https://stepik.org/course/551/syllabus)

1. Введение и базовые операции SQL

    1.1 Термины и определения

    1.2 Базовые операции
    
    1.3 Аггрегация данных

# 1. Введение и базовые операции SQL

## 1.1 Термины и определения

**Основные понятия БД**

* Сущность - класс, хранящейся в базе данных, таблица
* Объект - экземпляр сущности
* Атрибут - свойства, характеризующие сущность, колонки в таблице
* Кортеж - строка в таблице, набор значений конкретных атрибутов
* Домен - набор допустимых значений атрибута
* Идентификатор - атрибут с уникальным значением для данной таблицы

**Основные компоненты СУБД**

* Ядро - процессы, сеть, память, файловая система и т.д.
* Диспетчер данных - транзакции, кэш
* Диспетчер запросов - парсер запроса, оптимизатор, исполнитель
* Набор инструментов для служебных операций - резервное копирование, восстановление, мониторинг

## 1.2 Базовые операции

### 1.2.1 Выборка данных (SELECT)

Выбрать все данные из таблицы `billing`.

```SQL
SELECT * FROM billing;
```

Выбрать столбцы `name` и `total` из таблицы `billing`, чтобы значение`total` было больше 900

```SQL
SELECT email, total FROM billing WHERE total > 900;
```

Условия, заданные при помощи ключевого слова **WHERE** можно комбинировать с помощью ключевого слова **AND**.

```SQL
SELECT email, total FROM billing WHERE total > 900 AND currency='USD';
```

Для указания нескольких вариантов значения атрибута, можно использовать конструкцию _IN()_.

```SQL
SELECT email, total FROM billing WHERE total > 900 AND currency IN('USD', 'EUR');
```

Если нужно исключить конкретные значения атрибута, можно использовать _NOT_.

```SQL
SELECT email, total FROM billing WHERE total > 900 AND currency NOT IN('USD', 'EUR');
```

###  1.2.2 Добавление записи (INSERT)

Добавим новую запись в таблицу `billing`. По умолчанию нужно указать значения всех атрибутов в том же порядке, в котором они были расположены, когда создавалась таблица.

```SQL
INSERT INTO billing VALUES(
    'iamgroot@mail.ru',
    'tor@mail.ru',
    '500.00', 'USD',
    '2012-08-15',
    'Hello, Tor! I am groot.'
);
```

После названия таблицы можно указать конкретные атрибуты, которые нужно вставить.

```SQL
INSERT INTO billing(
    payer_email, recepient_email,
    sum, currency, billing_date
)
) VALUES(
    'iamgroot@mail.ru',
    'tor@mail.ru',
    '500.00', 'USD',
    '2012-08-15'
);
```

### 1.2.3 Обновление записей (UPDATE)

Можно обновить атрибут `currency` для всех записей в таблице `billing` с помощью ключевого слова **SET**.

```SQL
UPDATE billing SET currency='USD';
```

Указать какие именно кортежи следует обновлять можно с помощью ключевого слова **WHERE**.

```SQL
UPDATE billing SET currency='USD' WHERE payer_email='iamgroot@mail.ru';
```

### 1.2.4 Удаление записей (DELETE)

Удалить запись можно с помощью ключевого слова **DELETE**.

```SQL
DELETE FROM billing WHERE payer_email IS NULL OR payer_email='';
```

## 1.3 Аггрегация данных

В SQL несколько функций для аггрегации

* COUNT
* MAX
* MIN
* SUM
* AVG

Эти функции часто используются с оператором *GROUP BY*

```sql
/* Используем базу данных `project_simple` */
USE project_simple;

/* Вычисляем количество строк в таблице `project` */
SELECT COUNT(1) FROM project;

/* Вычисляем среднее значение атрибута `budget` в таблице `project` */
SELECT AVG(budget) FROM project;

/* Выводим время начала и финиша и разницу между ними */
SELECT
    project_finish,
    project_start,
    DATEDIFF(project_finish project_start)
FROM project;

/* Выводим среднее, максимум и минимум для разницы между стартом и финишем */
SELECT
    client_name,
    AVG(DATEDIFF(project_finish, project_start)) as avg_days,
    MAX(DATEDIFF(project_finish, project_start)) as max_days,
    MIN(DATEDIFF(project_finish, project_start)) as min_days
FROM project;

/* Выводим имя клиента, среднее, максимум и минимум для разницы между стартом и финишем
Исключаем строки с пустям `project_finish`
Группируем результат по клиентами (!аггрегация не по всей таблице)
Сортируем по максимальной длительности в порядке убывания
Ограничиваем выдачу 10 строками */

SELECT
    client_name,
    AVG(DATEDIFF(project_finish, project_start)) as avg_days,
    MAX(DATEDIFF(project_finish, project_start)) as max_days,
    MIN(DATEDIFF(project_finish, project_start)) as min_days
FROM project WHERE project_finish is not null
group by client_name
order by max_days DESC
LIMIT 10;

```