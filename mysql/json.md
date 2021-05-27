# MySQL
- document: https://dev.mysql.com/doc/refman/8.0/ja/json-search-functions.html

## indexを貼らずに検索する
```sql
CREATE TABLE billing (
    id INT AUTO_INCREMENT NOT NULL PRIMARY KEY,
    content JSON
 );
 
insert into billing set content='{"mmm": "b", "invoice": [{"name": "x", "value": "xx"}, {"name": "y", "value": "yy"}]}';
insert into billing set content='{"mmm": "a", "invoice": [{"name": "b", "value": "bb"}, {"name": "c", "value": "cc"}]}';
```

```sh
select * from billing where JSON_SEARCH(content, 'one', 'c', NULL,  "$.invoice[*].name");
+----+---------------------------------------------------------------------------------------+
| id | content                                                                               |
+----+---------------------------------------------------------------------------------------+
|  2 | {"mmm": "a", "invoice": [{"name": "b", "value": "bb"}, {"name": "c", "value": "cc"}]} |
+----+---------------------------------------------------------------------------------------+
```

## indexを貼って検索する

```sql
CREATE TABLE billing (
    id INT AUTO_INCREMENT NOT NULL PRIMARY KEY,
    content JSON,
    INDEX name((CAST(content->"$.invoice[*].name" AS CHAR(255) ARRAY)))
 );
 ```
 
 ```sh
 mysql> select * from billing;
+----+---------------------------------------------------------------------------------------+
| id | content                                                                               |
+----+---------------------------------------------------------------------------------------+
|  1 | {"mmm": "b", "invoice": [{"name": "x", "value": "xx"}, {"name": "y", "value": "yy"}]} |
|  2 | {"mmm": "a", "invoice": [{"name": "b", "value": "bb"}, {"name": "c", "value": "cc"}]} |
+----+---------------------------------------------------------------------------------------+
2 rows in set (0.00 sec)

mysql> select * from billing where "c" MEMBER OF (content->"$.invoice[*].name");
+----+---------------------------------------------------------------------------------------+
| id | content                                                                               |
+----+---------------------------------------------------------------------------------------+
|  2 | {"mmm": "a", "invoice": [{"name": "b", "value": "bb"}, {"name": "c", "value": "cc"}]} |
+----+---------------------------------------------------------------------------------------+
1 row in set (0.00 sec)

mysql> select * from billing where "c" MEMBER OF (content->'$.invoice[*].name');
+----+---------------------------------------------------------------------------------------+
| id | content                                                                               |
+----+---------------------------------------------------------------------------------------+
|  2 | {"mmm": "a", "invoice": [{"name": "b", "value": "bb"}, {"name": "c", "value": "cc"}]} |
+----+---------------------------------------------------------------------------------------+
1 row in set (0.00 sec)
 ```
