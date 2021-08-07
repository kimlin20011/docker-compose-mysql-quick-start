# Docker Mysql Compose quick start

## docker mysql 設定 / docker mysql install
* 下載mysql image / pull the mysql image
`docker pull mysql`
* 檢查下載結果/ check the pull result 
`docker images`

* bulild compose 
> -d 代表後台執行 / -d represent run in background
`docker-compose up -d`

# 進入容器/ enter the container
`mysql -h 127.0.0.1 -u root -p`

`mysql -h 127.0.0.1 -u test -p`

* 查看目前user/ print all user
`SELECT User, Host FROM mysql.user;`

* 重新設定認證方式 （為了跟node對接）/ reset the authentication way (to connect with node.js)
```
ALTER USER root IDENTIFIED WITH mysql_native_password BY 'nccutest';
ALTER USER test IDENTIFIED WITH mysql_native_password BY 'nccutest';
```

## mysql 設定 / other setting
* pw(root and test): nccutest

## 建立table / build a table
* mysql -u root -p
* 建立database(VC_UMA)
```sql=
CREATE DATABASE VC_UMA DEFAULT CHARACTER SET utf8 COLLATE utf8_general_ci;
```
* 新增新的database賬戶，這邊以帳戶：test、密碼: nccutest來舉例
```sql=
create user test IDENTIFIED BY 'nccutest';
```

* 之後在讓test帳戶，擁有更改UMA（目的）資料庫的權限
```sql=
GRANT ALL PRIVILEGES ON VC_UMA.* TO test;

FLUSH PRIVILEGES;
```
* 之後在離開MySQL使用新的帳戶登入
```shell=
mysql -h 127.0.0.1  -u test -p 
```

### 切換database到UMA/ use built database
```
USE VC_UMA;
```

### resourceSet
* resource_id(PK) VARCHAR(200)
* resourseDID VARCHAR(200)
* resourceName VARCHAR(200)
* scope   VARCHAR(30)
* registerTime int(12)  
* status BOOLEAN
```sql=
CREATE TABLE resourceSet (
    resource_id VARCHAR(200) NOT NULL PRIMARY KEY,
    resourseDID VARCHAR(200) NOT NULL,
    resourceName VARCHAR(200) NOT NULL,
    scope VARCHAR(400),
    registerTime  int(12),
    status BOOLEAN);
```

* check table form
`DESCRIBE resourceSet;`
* print all the info
`select * from resourceSet;`
# resource_registry table

## database 匯入匯出 / database backup and restore
* Backup all database
`docker exec vc_mysql /usr/bin/mysqldump -u root --password=nccutest --all-databases > <path/>backup.sql`

* backup in same file
`docker exec vc_mysql /usr/bin/mysqldump -u root --password=nccutest --all-databases > backup.sql`

* Restore
`cat backup.sql | docker exec -i vc_mysql /usr/bin/mysql -u root --password=nccutest `

###  原文
* Backup
`docker exec CONTAINER /usr/bin/mysqldump -u root --password=root DATABASE > backup.sql`

* Restore
`cat backup.sql | docker exec -i CONTAINER /usr/bin/mysql -u root --password=root DATABASE`


# 其他指令/ other command

* 結束所有container/ stop all the container
`docker stop $(docker ps -a -q)`

* 結束指定容器/ stop the container
`docker stop <id>`

* 查看所有容器 / check all the containers
`docker ps -a`

* 停止容器/ stop the container
`docker stop <name>`
* 重新打開/ start the container 
`docker start <name>`

* 移除容器/ remove container
`docker rm -f <id>`
`docker rm -f $(docker ps -a -q)`

# reference
* [Docker Compose](https://www.runoob.com/docker/docker-compose.html)


