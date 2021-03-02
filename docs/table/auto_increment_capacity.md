# Auto Increment Capacity

## Issue Type

Operational

## Problem

Auto incrementing columns have an upper limit based on their data type.
As tables grow there is a chance that the field will run out of capacity and INSERTs will start failing.
This can result in outages and long locking times while you attempt to correct the problem. 

## Remediation

When you run out of capacity, you will need to `ALTER` the column to a large size.

```sql
# Original table design
CREATE TABLE t1 (
    id MEDIUMINT UNSIGNED AUTO_INCREMENT,
    email VARCHAR(255) NOT NULL,
    PRIMARY KEY id
);

# Increase the capacity of the field
ALTER TABLE t1
    MODIFY COLUMN id INT UNSIGNED AUTO_INCREMENT;
```

## Considerations

* `AUTO_INCREMENT` fields do not wrap.
* Altering a large table can result in it locking up for an extended period of time.
  If you're unable to take the table offline to perform this operation, consider using a tool like [Online Schema Change](https://github.com/facebookincubator/OnlineSchemaChange/) to avoid downtime.

## External Resources

* [Online Schema Change](https://github.com/facebookincubator/OnlineSchemaChange/)
* [MySQL 8.0 Integer Type Capacity](https://dev.mysql.com/doc/refman/8.0/en/integer-types.html)