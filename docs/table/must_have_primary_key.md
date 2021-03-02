# Title

## Issue Type

Design

## Problem

MySQL tables should have a PRIMARY KEY.

Since you're probably using [InnoDB](preferred_engine), there are a bunch of reasons you'll want a PRIMARY KEY:

* InnoDB uses a mechanism called clustered index. It’s the key that the database record gets stored against.
  If you have this value you can look up the memory location of your record.
  It’s synonymous with primary key unless you don’t set one.
  If you don’t set one InnoDB then starts a process of trying to determine one.
    * It first looks for an appropriate NON NULL UNIQUE field to treat as the clustered index.
    * If that doesn’t work, then it tries to find a relevant INDEX to use instead.
    * Failing that it falls back to creating its own AUTO INCREMENTing field that it uses internally.
      You’re using the disk space but you don’t get the benefits of being able to use the ID value.
* InnoDB INSERTS use a mutex to manage locking. With an implicit clustered key you can experience random stalls and locking. Parallel inserts into more than one table with implicit keys could be heavily performance constrained.
* If you’re using binary logs you’ll use considerably more disk space due to needing to log the entire row for operations rather than just the PRIMARY KEY (like DELETEs)
* If you’re doing replication (via binary logs), you will encounter serious performance issues as all modifications will require full table scans due to lack of a PRIMARY KEY for look-ups.
* MySQL 8 replication will break if you have InnoDB tables without a PRIMARY KEY.
* If you’re using CLUSTERS, TRANSACTIONS become exponentially slower due to consistency checking between nodes.

## Remediation

You can pick an appropriate column to make the primary key (a UNIQUE column is a good option).

```sql
ALTER TABLE t1
    ADD PRIMARY KEY (col1);
```

Or you can add an auto incrementing column

```sql
ALTER TABLE t1 ADD id int NOT NULL AUTO_INCREMENT PRIMARY KEY
```

## Considerations



## External Resources

* [Why MySQL tables need a PRIMARY KEY](https://federico-razzoli.com/why-mysql-tables-need-a-primary-key)
* [How does InnoDB behave without a PRIMARY KEY](https://blog.jcole.us/2013/05/02/how-does-innodb-behave-without-a-primary-key/)
