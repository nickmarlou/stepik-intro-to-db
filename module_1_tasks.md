# Задача 1.3.4

```sql
USE store_simple;

SELECT
    COUNT(1) as total
FROM project;
```

# Задача 1.3.5

```sql
USE store_simple;

SELECT
    category as category,
    COUNT(sold_num) FROM store as sold_num
group by category
order by category;
```

# Задача 1.3.6

```sql
USE store_simple;

/* Для правильной аггрегации обязательно в блоке SELECT
для аггрегируемых аттрибутов должна быть указана
функция-агрегатор, например, SUM, COUNT, MAX и т.д. */

SELECT
    category,
    SUM(price * sold_num) as revenue
FROM store
GROUP BY category
ORDER BY revenue DESC
LIMIT 5
```

# Задача 1.3.7

```sql
USE project_simple;

SELECT
    COUNT(1) as total,
    SUM(budget) as budget_sum,
    AVG(DATEDIFF(project_finish, project_start)) as average_project_time
FROM project;
```