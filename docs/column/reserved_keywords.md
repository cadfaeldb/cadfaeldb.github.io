# Reserved Keywords

## Problem

MySQL reserves keywords for special usage in its implementation of the SQL query language standard.
Each new release of MySQL adds new keywords.

To avoid confusing MySQL, you can quote identifiers (schema, table and column names) in backticks (`).
This doesn't always happen in code and can cause existing code to break when you upgrade your MySQL Server version.

## Remediation

It's better to try avoiding the use of reserved keywords as identifiers.

## External Resources

* [MySQL 5.7 Reserved Keywords](https://dev.mysql.com/doc/refman/5.7/en/keywords.html)
* [MySQL 8 Reserved Keywords](https://dev.mysql.com/doc/refman/8.0/en/keywords.html)