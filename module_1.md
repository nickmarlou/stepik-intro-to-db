# Введение в базовые операции

https://stepik.org/course/551/

## Аггрегация

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