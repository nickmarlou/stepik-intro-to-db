# Задача 3.5.6

```sql
CREATE TABLE IF NOT EXISTS `best_offer_sale` (
    `id` INT NOT NULL,
    `name` VARCHAR(255),
    `dt_start` DATETIME NOT NULL,
    `dt_finish` DATETIME NOT NULL,
    PRIMARY KEY(`id`))
    ENGINE = InnoDB,
    DEFAULT CHARACTER SET = utf8,
    COLLATE = utf8_general_ci;
```

# Задача 3.5.9

```sql
ALTER TABLE `client`
    DROP FOREIGN KEY fk_client_source1,
    DROP COLUMN `code`,
    DROP COLUMN `source_id`;
```

# Задача 3.5.10

```sql
DROP TABLE `source`;
```

# Задача 3.5.11

```sql
ALTER TABLE `sale_has_good`
    ADD COLUMN `num` INT NOT NULL,
    ADD COLUMN `price` DECIMAL(18,2) NOT NULL;
```

# Задача 3.5.12

```sql
ALTER TABLE `client`
    ADD COLUMN `source_id` INT,
    ADD CONSTRAINT `source_id` FOREIGN KEY (source_id) REFERENCES source(id);
```