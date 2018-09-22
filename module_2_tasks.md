# Задача 2.3.4

Выборка из трех таблиц с сортировкой по двум атрибутам.

```sql
SELECT good.name as good_name, category.name as category_name
    FROM good
    INNER JOIN category_has_good ON good.id  =category_has_good.good_id
    INNER JOIN category ON category_has_good.category_id = category.id
    ORDER BY good.name, category.name ASC;
```

# Задача 2.3.5

```sql
SELECT first_name, last_name, COUNT(sale_sum) as name_sale_sum
    FROM sale
    INNER JOIN status ON sale.status_id = status.id
    INNER JOIN client ON sale.client_id = client.id
    WHERE status.name='new'
    GROUP BY client.id;
```