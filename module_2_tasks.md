# Задача 2.3.4

Выборка из трех таблиц с сортировкой по двум атрибутам.

```sql
SELECT good.name as good_name, category.name as category_name
    FROM good
    INNER JOIN category_has_good ON good.id=category_has_good.good_id
    INNER JOIN category ON category_has_good.category_id = category.id
    ORDER BY good.name, category.name ASC;
```

# Задача 2.3.5

```sql
SELECT first_name, last_name, COUNT(sale_sum) as name_sale_sum
    FROM sale
    INNER JOIN status ON sale.status_id=status.id
    INNER JOIN client ON sale.client_id=client.id
    WHERE status.name='new'
    GROUP BY client.id;
```

# Задача 2.3.7

Выборка из трёх таблиц, в том числе товаров, для которых категория не указана.

```sql
USE store;

SELECT g.name as good_name, c.name as category_name
    FROM good as g
    LEFT OUTER JOIN category_has_good as chg ON g.id=chg.good_id
    LEFT OUTER JOIN category as c ON chg.category_id = c.id;
```

# Задача 2.3.8

Выборка из трёх таблиц, в том числе товаров, для которых категория не указана и категорий, для которых нет товаров.

Делаем две выборки и используем *UNION*, чтобы объединить их.

```sql
SELECT g.name as good_name, c.name as category_name
    FROM good as g
    LEFT OUTER JOIN category_has_good as chg ON g.id=chg.good_id
    LEFT OUTER JOIN category as c ON chg.category_id = c.id
UNION
SELECT g.name as good_name, c.name as category_name
    FROM good as g
    LEFT OUTER JOIN category_has_good as chg ON g.id=chg.good_id
    RIGHT OUTER JOIN category as c ON chg.category_id = c.id;
```

# Задача 2.3.9

```sql
SELECT
    s.name as source_name,
    SUM(sale.sale_sum) as sale_num
    FROM client as c
    #Правое внешнее соединение, чтобы включить источники, которых нет в таблице с клиентами
    RIGHT OUTER JOIN source as s ON s.id = c.source_id
    #Левое внешнее соединение, чтобы сохранить источники, для которых нет продаж (чтобы источник не пропал, если у него нет атрибута client_id)
    LEFT OUTER JOIN sale ON sale.client_id = c.id
    GROUP BY s.id;
```