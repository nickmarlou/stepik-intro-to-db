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

Правое внешнее соединение, чтобы включить источники, которых нет в таблице с клиентами

Левое внешнее соединение, чтобы сохранить источники, для которых нет продаж (чтобы источник не пропал, если у него нет атрибута client_id)

```sql
SELECT
    s.name as source_name,
    SUM(sale.sale_sum) as sale_num
    FROM client as c
    RIGHT OUTER JOIN source as s ON s.id = c.source_id
    LEFT OUTER JOIN sale ON sale.client_id = c.id
    GROUP BY s.id;
```

# Задача 2.3.10

```sql
USE store;

SELECT
    g.name as good_name
    FROM category as c 
    INNER JOIN category_has_good as chg ON c.id=chg.category_id
    INNER JOIN good as g ON chg.good_id=g.id
    WHERE c.name='cakes'
UNION
SELECT
    g.name as good_name
    FROM status as s
    INNER JOIN sale ON s.id=sale.status_id
    INNER JOIN sale_has_good as shg ON sale.id=shg.sale_id
    INNER JOIN good as g ON shg.good_id=g.id
    WHERE s.name='delivering';
```

# Задача 2.3.11

Запрашиваем список категорий с количеством проданных товаров, в том числе и категории, для которых нет заказов.

```sql
SELECT
    # Название категории из таблицы category
    c.name as name,
    # Количество продаж товара при помощи агрегатной функции COUNT из таблицы sale
    COUNT(s.id) as sale_num
    # Начинаем с таблицы sale
    FROM sale as s
    # Присоединяем таблицы, используя ПРАВОЕ ВНЕШНЕЕ СОЕДИНЕНИЕ,
    # чтобы в выборку попали категории, товары из которых не покупали
    # Слева - продажи, справа - категории и т.д.
    # Правое внешнее соединение значит, что элементы из правой таблицы без совпадений сохраняются
    # Например, если для категории (справа) нет продаж (слева)
    RIGHT OUTER JOIN sale_has_good as shg ON s.id=shg.sale_id
    RIGHT OUTER JOIN category_has_good as chg ON shg.good_id=chg.good_id
    RIGHT OUTER JOIN category as c ON chg.category_id=c.id
    # Группируем продажи по категориям товаров
    GROUP BY c.id;
```

# Задача 2.3.12

```sql
SELECT
	s.name as source_name
    FROM source s
    LEFT OUTER JOIN client as c
    ON s.id=c.source_id
    WHERE c.id IS null
UNION
SELECT
	s.name
    FROM source s
    # Правое внешнее соединение значит, что элементы из левой таблицы без совпадений сохраняются
    # Например, если для источника (слева) нет клиентов (справа)
    LEFT OUTER JOIN client as c
    ON s.id=c.source_id
    LEFT OUTER JOIN sale as sl
    ON c.id=sl.client_id
    LEFT OUTER JOIN status as st
    ON sl.status_id=st.id
    WHERE st.name='rejected'
    OR st.id IS null;
```