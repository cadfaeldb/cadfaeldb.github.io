# Sane Auto Increment

## Problem

Any auto incrementing field is auto-populated by MySQL and starts at 1 and increases with each following row insert.

As such the value of the auto incrementing field will always be greater than or equal to 1.

Auto incrementing fields have a primary purpose, unique identification. Thus they should always be the exclusive primary key.

## Remediation

* Create auto incrementing columns as unsigned as the negative range is never used.
* Change your PRIMARY KEY to use the auto incrementing column.
  Change the old PRIMARY KEY to an INDEX or UNIQUE constraint.
```SQL
CREATE TABLE t1 (
    id INT UNSIGNED AUTO_INCREMENT,
    email VARCHAR(255) NOT NULL,
    PRIMARY KEY email
);

ALTER TABLE t1
    DROP PRIMARY KEY,
    ADD PRIMARY KEY (id),
    ADD UNIQUE (email);
```

## Considerations

Valid cases for negative auto incrementing values:
* Auto incrementing field could have a starting point in the negative range.
```sql
ALTER TABLE t1 AUTO_INCREMENT = -100;
```
* Inserts into a table can manually specify a value for the auto incrementing field in the negative range.
```sql
INSERT INTO t1 (id, email) VALUES (-1000, 'bob@smith.com');
```

## External Resources

* [MySQL 8.0 Auto Increment](https://dev.mysql.com/doc/refman/8.0/en/example-auto-increment.html)