#### EXPORT
```sh
mysqldump -uroot -pXXX dbname > /backups/dbname.sql
mysqldump -uroot -pXXX dbname | gzip > /backups/dbname.sql.gz
```

#### IMPORT
```sh
mysql -uroot -pXXX dbname < /backups/dump.sql
```

#### INSERT + ON DUPLICATE KEY UPDATE
```sh
INSERT INTO table (primaryID, column1, column2, column3)
VALUES (1, "aaa", "bbb", "ccc"),(2, "xxx", "yyy", "zzz")
ON DUPLICATE KEY UPDATE column1 = VALUES(column1), column2 = VALUES(column2)
```

#### DELETE + JOIN
```sh
DELETE t1
FROM aaa AS t1
INNER JOIN bbb as t2 USING (ID)
WHERE t1.column = "XXX"
```
