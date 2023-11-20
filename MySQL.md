#### Recovery MariaDB service
```sh
For starting mysql, you should config Innodb force recovery (increase this value from 1 to 6). This will give you detail information https://dev.mysql.com/doc/refman/5.7/en/forcing-innodb-recovery.html
```

#### Export
Crontab: escape "%" to "/%".
```sh
mysqldump -uroot -pXXX dbname > /var/backup/dbname.sql
mysqldump -uroot -pXXX dbname | gzip > /var/backup/dbname.sql.gz
mysqldump -uroot -pXXX --databases $(mysql -uroot -pXXX -N information_schema -e "SELECT DISTINCT(TABLE_SCHEMA) FROM tables WHERE TABLE_SCHEMA LIKE 'prefix%'" ) | gzip > /var/backup/alldb.sql.gz
```

#### Import
```sh
mysql -uroot -pXXX dbname < /backups/dbname.sql
zcat /backups/dbname.sql.gz | mysql -uroot -pXXX dbname
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
FROM aaaa AS t1
INNER JOIN bbbb as t2 USING (ID)
WHERE t1.column = "XXX"
```

#### UPDATE + JOIN
```sh
UPDATE aaaa as t1 
INNER JOIN bbbb as t2 ON t1.id = t2.id
SET t1.column = t2."XXX" 
WHERE t1.id = "555"
```

#### Trigger INSERT
```sh
IF @TRIGGERED IS NULL THEN

SET @TRIGGERED = 1;

INSERT IGNORE INTO other_table (
    column1,
    column2
)
VALUES ( 
	NEW.column1,
	NEW.column2
);

SET @TRIGGERED = NULL;

END IF
```

#### Trigger UPDATE
```sh
IF @TRIGGERED IS NULL THEN

SET @TRIGGERED = 1;

UPDATE other_table SET 
    column1 = NEW.column1,
    column2 = NEW.column2
WHERE primary = NEW.primary;

SET @TRIGGERED = NULL;

END IF
```

#### Trigger DELETE
```sh
IF @TRIGGERED IS NULL THEN

SET @TRIGGERED = 1;

DELETE FROM other_table
WHERE primary = OLD.primary;

SET @TRIGGERED = NULL;

END IF
```

#### my.cnf Configuration
```sh
[mysqld]
bind-address = internal network ip

max_connections = 800
tmp_table_size = 128M
key_buffer_size = 256M

query_cache_type = 1
query_cache_size = 16M

#Percona Analysis
long_query_time = 1
slow_query_log = 1
slow_query_log_file = /var/log/mariadb/mariadb-slow.log
```

#### Percona Toolkit (<a href="https://www.percona.com/downloads/percona-toolkit/" target="_blank">Web</a>)
```sh
yum install https://www.percona.com/downloads/percona-toolkit/2.2.19/RPM/percona-toolkit-2.2.19-1.noarch.rpm
pt-query-digest /var/log/mariadb/mariadb-slow.log
```
```sh
pt-duplicate-key-checker -pXXX
```
