# Correct UTF-8 Encoding

## Problem

UTF-8 (`utf8`) encoding in MySQL supports up to 3 bytes per character (or `utf8mb3`) which limits coverage to characters in the  [Basic Multilingual Plane](https://en.wikipedia.org/wiki/Plane_(Unicode)#Basic_Multilingual_Plane) (BMP).

This means if you try insert a string that contains any character that needs 4 bytes of storage ([emojis, musical notation, mathematical notations, cuneiform and others](https://en.wikipedia.org/wiki/Plane_(Unicode)#Overview)) it will drop that character.

## Remediation

Ensure you create the columns you want to store UTF-8 characters in as `CHARACTER SET utf8mb4`.

```sql
CREATE TABLE tab1 (
    col1 CHAR(20) CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci,
    ...
);
```

You can convert from `utf8mb3` to `utf8mb4`. It's a safe operation as `utf8mb3` is a subset of `utf8mb4`.

```sql
ALTER TABLE tab1
    MODIFY COLUMN col1 CHAR(20) CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
```

## Considerations

* Column type size limitations
    - A TINYTEXT column can hold up to 255 bytes, so it can hold up to 85 3-byte or 63 4-byte characters.
    - Suppose that you have a TINYTEXT column that uses utf8mb3 but must contain more than 63 characters. You cannot convert it to utf8mb4 unless you also change the data type to a longer type such as TEXT.

* Large column types
    * InnoDB has a max index length of 767 bytes (if it's using `COMPACT` or `REDUNDANT` row formats).
      Large text columns should already use sane [key prefixing](https://dev.mysql.com/doc/refman/8.0/en/create-index.html#create-index-column-prefixes). 
      * `utf8mb3` the index cannot exceed 255 characters (`INDEX col1(255)`):
      * `utf8mb8` the index cannot exceed 191 characters (`INDEX col1(191)`):
    

## External Resources

* [MySQL Unicode Support](https://dev.mysql.com/doc/refman/8.0/en/charset-unicode.html)
* [MySQL Converting Between 3-Byte and 4-Byte Unicode Character Sets](https://dev.mysql.com/doc/refman/8.0/en/charset-unicode-conversion.html)
* [What's the difference between utf8 and utf8mb4?](https://www.eversql.com/mysql-utf8-vs-utf8mb4-whats-the-difference-between-utf8-and-utf8mb4/)
* [MySQL Column Prefix Key Parts](https://dev.mysql.com/doc/refman/8.0/en/create-index.html#create-index-column-prefixes)