https://stepik.org/course/551/

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