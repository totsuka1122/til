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
[Multi-Valued Indexes](https://qiita.com/hmatsu47/items/3e49a473bc36aeefc706#:~:text=Multi-Valued%20Indexes%20%E3%81%A8%E3%81%AF,%E3%81%AB%E6%A9%9F%E8%83%BD%E3%81%99%E3%82%8B%20INDEX%20%E3%81%A7%E3%81%99%E3%80%82)
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

## JSONの中でソートをかける

```sh
insert into billing set content='{"n": 1, "nn": 1, "created":"2021-05-31T00:00:00+09:00", "contract_id": "77777777-228e-421a-b707-615a934137af"}';
insert into billing set content='{"n": 2, "nn": 3, "created":"2021-05-31T00:00:00+09:00", "contract_id": "77777777-228e-421a-b707-615a934137af"}';
insert into billing set content='{"n": 2, "nn": 1, "created":"2021-05-31T00:00:00+09:00", "contract_id": "6a3bc157-228e-421a-b707-615a934137af"}';
insert into billing set content='{"n": 2, "nn": 2, "created":"2021-05-31T00:00:00+09:00", "contract_id": "6a3bc157-228e-421a-b707-615a934137af"}';
insert into billing set content='{"n": 1, "nn": 1, "created":"2021-05-31T00:00:00+09:00", "contract_id": "6a3bc157-228e-421a-b707-615a934137af"}';
insert into billing set content='{"n": 1, "nn": 0, "created":"2021-05-31T00:00:00+09:00", "contract_id": "6a3bc157-228e-421a-b707-615a934137af"}';
```

```sh
SELECT id, content, JSON_EXTRACT(content, '$.mmm') AS mmm FROM billing ORDER BY mmm ASC;
```

複数カラムでソートをかける
```sh
select * from billing order by json_extract(content, '$.n') asc, json_extract(content, '$.nn') asc;
```
