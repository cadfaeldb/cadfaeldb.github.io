# Empty Table

## Issue Type

Housekeeping

## Problem

You have some empty tables. Maybe they should be dropped?

## Remediation

```sql
# It's always a good idea to rename a table first, before dropping it.
# This way you can quickly rename it back if anything relied on it. 
RENAME TABLE t1 TO t1_to_be_dropped

# Wait a little while and then
DROP TABLE t1_to_be_dropped;
```

## Considerations

This check does use a few tricks to avoid accidentally flagging a table that is likely being used for a queue.
* Checks the auto increment offset is greater than the expected starting point.
* Checks if there is available free space in the table (which normally indicates that this table has had rows added and deleted). 

## External Resources

* [MySQL 8.0 RENAME TABLE Statement](https://dev.mysql.com/doc/refman/8.0/en/rename-table.html)
* [MySQL 8.0 DROP TABLE Statement](https://dev.mysql.com/doc/refman/8.0/en/drop-table.html)