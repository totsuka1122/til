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
insert into billing set content='{"n": 1, "nn": 1, "created":"2021-06-31T00:00:00+09:00", "contract_id": "6a3bc157-228e-421a-b707-615a934137af"}';
insert into billing set content='{"n": 1, "nn": 1, "created":"2020-05-31T00:00:00+09:00", "contract_id": "6a3bc157-228e-421a-b707-615a934137af"}';
insert into billing set content='{"n": 1, "nn": 1, "created":"2020-05-30T00:00:00+09:00", "contract_id": "6a3bc157-228e-421a-b707-615a934137af"}';
```

```sh
SELECT id, content, JSON_EXTRACT(content, '$.mmm') AS mmm FROM billing ORDER BY mmm ASC;
```

複数カラムでソートをかける
```sh
select * from billing order by json_extract(content, '$.n') asc, json_extract(content, '$.nn') asc;
```

whereと合わせる + Limit/Offset
```sh
select * from billing where json_extract(content, '$.n')=1 order by json_extract(content, '$.contract_id') asc limit 5 offset 0;
```

## 竹内さんからもらった

```sql
CREATE TABLE t1 (jdoc JSON);

INSERT INTO t1 VALUES('{"name": "aaa1", "type": 1, "available": true, "start": "2021-06-25T00:00:00+09:00"}');
INSERT INTO t1 VALUES('{"name": "aaa2", "type": 1, "available": false, "start": "2021-06-26T00:00:00+09:00"}');
INSERT INTO t1 VALUES('{"name": "aaa3", "type": 1, "available": false, "start": "2021-06-27T00:00:00+09:00"}');
INSERT INTO t1 VALUES('{"name": "aaa4", "type": 1, "available": true, "start": "2021-06-28T00:00:00+09:00"}');
INSERT INTO t1 VALUES('{"name": "aaa1", "type": 2, "available": true, "start": "2021-06-25T00:00:00+09:00"}');
INSERT INTO t1 VALUES('{"name": "aaa2", "type": 2, "available": false, "start": "2021-06-24T00:00:00+09:00"}');
INSERT INTO t1 VALUES('{"name": "aaa3", "type": 2, "available": false, "start": "2021-06-23T00:00:00+09:00"}');
INSERT INTO t1 VALUES('{"name": "aaa4", "type": 2, "available": true, "start": "2021-06-22T00:00:00+09:00"}');
INSERT INTO t1 VALUES('{"name": "aaa1", "type": 3, "available": true, "start": "2021-06-25T00:00:00+09:00"}');
INSERT INTO t1 VALUES('{"name": "aaa2", "type": 3, "available": false, "start": "2021-07-25T00:00:00+09:00"}');
INSERT INTO t1 VALUES('{"name": "aaa3", "type": 3, "available": false, "start": "2021-08-25T00:00:00+09:00"}');
INSERT INTO t1 VALUES('{"name": "aaa4", "type": 3, "available": true, "start": "2021-09-25T00:00:00+09:00"}');
INSERT INTO t1 VALUES('{"name": "aaa1", "type": 4, "available": true, "start": "2021-06-25T00:00:00+09:00"}');
INSERT INTO t1 VALUES('{"name": "aaa2", "type": 4, "available": false, "start": "2021-05-25T00:00:00+09:00"}');
INSERT INTO t1 VALUES('{"name": "aaa3", "type": 4, "available": false, "start": "2021-04-25T00:00:00+09:00"}');
INSERT INTO t1 VALUES('{"name": "aaa4", "type": 4, "available": true, "start": "2021-03-25T00:00:00+09:00"}');



mysql> SELECT * FROM t1 ORDER BY jdoc->"$.available" ASC, jdoc->"$.type" DESC, CAST(jdoc->>"$.start" AS DATETIME) ASC;
+---------------------------------------------------------------------------------------+
| jdoc                                                                                  |
+---------------------------------------------------------------------------------------+
| {"name": "aaa3", "type": 4, "start": "2021-04-25T00:00:00+09:00", "available": false} |
| {"name": "aaa2", "type": 4, "start": "2021-05-25T00:00:00+09:00", "available": false} |
| {"name": "aaa2", "type": 3, "start": "2021-07-25T00:00:00+09:00", "available": false} |
| {"name": "aaa3", "type": 3, "start": "2021-08-25T00:00:00+09:00", "available": false} |
| {"name": "aaa3", "type": 2, "start": "2021-06-23T00:00:00+09:00", "available": false} |
| {"name": "aaa2", "type": 2, "start": "2021-06-24T00:00:00+09:00", "available": false} |
| {"name": "aaa2", "type": 1, "start": "2021-06-26T00:00:00+09:00", "available": false} |
| {"name": "aaa3", "type": 1, "start": "2021-06-27T00:00:00+09:00", "available": false} |
| {"name": "aaa4", "type": 4, "start": "2021-03-25T00:00:00+09:00", "available": true}  |
| {"name": "aaa1", "type": 4, "start": "2021-06-25T00:00:00+09:00", "available": true}  |
| {"name": "aaa1", "type": 3, "start": "2021-06-25T00:00:00+09:00", "available": true}  |
| {"name": "aaa4", "type": 3, "start": "2021-09-25T00:00:00+09:00", "available": true}  |
| {"name": "aaa4", "type": 2, "start": "2021-06-22T00:00:00+09:00", "available": true}  |
| {"name": "aaa1", "type": 2, "start": "2021-06-25T00:00:00+09:00", "available": true}  |
| {"name": "aaa1", "type": 1, "start": "2021-06-25T00:00:00+09:00", "available": true}  |
| {"name": "aaa4", "type": 1, "start": "2021-06-28T00:00:00+09:00", "available": true}  |
+---------------------------------------------------------------------------------------+
16 rows in set, 16 warnings (0.00 sec)
```
