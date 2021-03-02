# Preferred Table Storage Engine

## Issue Type

Design

## Problem

InnoDB has been the preferred database engine since MySQL 5.5 (2009).

While MyISAM used to perform better in certain use cases, improvements to InnoDB in the last 10 years have negated them.

## Remediation

```sql
ALTER TABLE t1 ENGINE=InnoDB;
```

## Considerations

* InnoDB supports transactions, which means you can commit and roll back. MyISAM does not.
* InnoDB has row-level locking. MyISAM has full table-level locking.
* InnoDB has referential integrity which involves supporting foreign keys (RDBMS) and relationship constraints, MyISAM does not (DMBS).
* InnoDB is more reliable as it uses transactional logs for auto recovery. MyISAM does not.

## External Resources

* [MySQL 8.0 The end of MyISAM](https://www.percona.com/blog/2016/10/11/mysql-8-0-end-myisam/)
* [MyISAM around 5x slower than InnoDB](https://dba.stackexchange.com/questions/267540/myisam-around-5x-slower-than-innodb)