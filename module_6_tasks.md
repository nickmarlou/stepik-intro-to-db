# Задача 6.3.6

> **Важно!** Форвард-инжиниринг модели [store.mdb](https://github.com/amyasnov/stepic-db-intro/tree/master/week_5) может сопровождаеться ошибками из-за ключевого слова VISIBLE в коде скрипте. Это слово из скрипта нужно удалить.

```sql
USE store;

explain
select 
  name, 
    ifnull((select category.name from category 
    join category_has_good on category.id=category_has_good.category_id
        where category_has_good.good_id=good.id
        order by category.name limit 1)
  , 0) as first_category 
from good where name='F%';
```

# Задача 6.3.7

```sql
USE store;

SELECT * FROM good;

CREATE INDEX good_name_idx
    USING BTREE
    ON good (`name`);

explain
select 
  name, 
    ifnull((select category.name from category 
    join category_has_good on category.id=category_has_good.category_id
        where category_has_good.good_id=good.id
        order by category.name limit 1)
  , 0) as first_category 
from good where name like 'F%';
```

