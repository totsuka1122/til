# MySQL
- document: https://dev.mysql.com/doc/refman/8.0/ja/json-search-functions.html

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
