# 数据库mysql

## 1-基本操作

1. 安装服务端： \`yum install mysql-community-server\`

2. 启动：\`service mysqld start/restart\`

3. 停止：\`service mysqld stop\`

_补充：CentOS7 默认安装Mariadb数据库，为安装mysql需要先移除Mariadb数据库\`yum remove mariadb-libs.x86\_64\`      
_

1. 下载MySQL源：[https://dev.mysql.com/downloads/repo/yum/](https://dev.mysql.com/downloads/repo/yum/)

2. 安装源：yum localinstall mysql57-community-release-el7-8.noarch.rpm

3. 安装MySQL服务 yum install mysql-community-server

4. 默认密码 'cat /var/log/mysqld.log \|grep "password"'

## 2- MySQL拓展知识

1. 远程连接

2. 开启Genelog

3. 新建用户和权限操作

4. 忘记root密码找回

### 3- MySQL客户端工具

1. SQlyog

2. Navicat

3. HeidiSqL

4. Sequal pool

5. PHPAdmin

## 4-删除myria

![](https://i.imgur.com/VbhAOCH.png)

```
\[root@localhost ~\]\# yum search mysql

\[root@localhost ~\]\# yum remove mariadb-libs.x86\_64
```

## 5-下载mysql

[https://dev.mysql.com/downloads/repo/yum/](https://dev.mysql.com/downloads/repo/yum/)

![](https://i.imgur.com/hXJRpKi.png)

[https://dev.mysql.com/get/mysql80-community-release-el7-1.noarch.rpm](https://dev.mysql.com/get/mysql80-community-release-el7-1.noarch.rpm)

```
[root@localhost ~]# cd /tmp/
[root@localhost tmp]# wget https://dev.mysql.com/get/mysql80-community-release-el7-1.noarch.rpm
//安装源码
[root@localhost tmp]# yum localinstall mysql80-community-release-el7-1.noarch.rpm 
//检查是否安装
[root@localhost tmp]# yum search mysql
//安装源
[root@localhost tmp]# yum install mysql-community-server
//查看服务是否启动
[root@localhost tmp]# ps -ef | grep mysql
//重启mysql
[root@localhost tmp]# service mysqld restart
```

## 再次检查

```
[root@localhost tmp]# ps -ef | grep mysql

root 2011 1839 0 18:41 pts/1 00:00:00 grep --color=auto mysql

[root@localhost tmp]# service mysqld restart

Redirecting to /bin/systemctl restart mysqld.service

[root@localhost tmp]# ps -ef | grep mysql

mysql 2097 1 11 18:41 ? 00:00:01 /usr/sbin/mysqld

root 2140 1839 0 18:41 pts/1 00:00:00 grep --color=auto mysql
```

## 查看密码（会自动生成）

```
[root@localhost tmp]# cat /var/log/mysqld.log |grep password

2018-09-02T10:41:29.595218Z 5 [Note] [MY-010454] [Server] A temporary password is generated for root@localhost: %K6sqEG=qqa1

[root@localhost tmp]# mysql -uroot -p%K6sqEG=qqa1
```

## 进入mysql

```
 mysql: [Warning] Using a password on the command line interface can be insecure.

Welcome to the MySQL monitor. Commands end with ; or \g.

Your MySQL connection id is 8

Server version: 8.0.12



Copyright (c) 2000, 2018, Oracle and/or its affiliates. All rights reserved.



Oracle is a registered trademark of Oracle Corporation and/or its

affiliates. Other names may be trademarks of their respective

owners.



Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.



mysql>
```

## 重置密码

```
    mysql> show databases;
    ERROR 1820 (HY000): You must reset your password using ALTER USER statement before executing this statement.
    mysql>
```

## 设置安全策略发现的问题

注意：如果只想设置简单密码需要修改两个全局参数：

```
 //先

mysql> set global validate_password_policy=0;

mysql> set global validate_password_length=1;



//后

mysql> SET PASSWORD = PASSWORD('123456');
```

## MySQL 8 操作上有区别

1、MySQL 8.0 执行代码：

```
  mysql> set global validate_password.policy=0;

Query OK, 0 rows affected (0.00 sec)



mysql> set global validate_password.length=1;

Query OK, 0 rows affected (0.00 sec)
```

```
//设置密码

mysql> alter user user() identified by "123456";

Query OK, 0 rows affected (0.09 sec)



mysql> select version();

+-----------+

| version() |

+-----------+

| 8.0.12 |

+-----------+

1 row in set (0.00 sec)
```

## 8-远程连接

```
mysql> show databases;

+--------------------+

| Database |

+--------------------+

| information_schema |

| mysql |

| performance_schema |

| sys |

+--------------------+

4 rows in set (0.02 sec)
```

```
mysql> use mysql
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A
```

```php
Database changed

mysql> show tables;

+---------------------------+

| Tables_in_mysql |

+---------------------------+

| columns_priv |

| component |

| db |

| default_roles |

| engine_cost |

| func |

| general_log |

| global_grants |

| gtid_executed |

| help_category |

| help_keyword |

| help_relation |

| help_topic |

| innodb_index_stats |

| innodb_table_stats |

| password_history |

| plugin |

| procs_priv |

| proxies_priv |

| role_edges |

| server_cost |

| servers |

| slave_master_info |

| slave_relay_log_info |

| slave_worker_info |

| slow_log |

| tables_priv |

| time_zone |

| time_zone_leap_second |

| time_zone_name |

| time_zone_transition |

| time_zone_transition_type |

| user |

+---------------------------+

33 rows in set (0.00 sec)
```

```
mysql&gt; select \* from user /G;

ERROR 1064 \(42000\): You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near '/G' at line 1

mysql&gt; select \* from user \G;

\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\* 1. row \*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*

                  Host: localhost

                  User: mysql.infoschema

           Select\_priv: Y

           Insert\_priv: N

           Update\_priv: N

           Delete\_priv: N

           Create\_priv: N

             Drop\_priv: N

           Reload\_priv: N

         Shutdown\_priv: N

          Process\_priv: N

             File\_priv: N

            Grant\_priv: N

       References\_priv: N

            Index\_priv: N

            Alter\_priv: N

          Show\_db\_priv: N

            Super\_priv: N

 Create\_tmp\_table\_priv: N

      Lock\_tables\_priv: N

          Execute\_priv: N

       Repl\_slave\_priv: N

      Repl\_client\_priv: N

      Create\_view\_priv: N

        Show\_view\_priv: N

   Create\_routine\_priv: N

    Alter\_routine\_priv: N

      Create\_user\_priv: N

            Event\_priv: N

          Trigger\_priv: N

Create\_tablespace\_priv: N

              ssl\_type: 

            ssl\_cipher: 

           x509\_issuer: 

          x509\_subject: 

         max\_questions: 0

           max\_updates: 0

       max\_connections: 0

  max\_user\_connections: 0

                plugin: caching\_sha2\_password

 authentication\_string: $A$005$THISISACOMBINATIONOFINVALIDSALTANDPASSWORDTHATMUSTNEVERBRBEUSED

      password\_expired: N

 password\_last\_changed: 2018-09-02 18:41:31

     password\_lifetime: NULL

        account\_locked: Y

      Create\_role\_priv: N

        Drop\_role\_priv: N

Password\_reuse\_history: NULL

   Password\_reuse\_time: NULL

\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\* 2. row \*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*

                  Host: localhost

                  User: mysql.session

           Select\_priv: N

           Insert\_priv: N

           Update\_priv: N

           Delete\_priv: N

           Create\_priv: N

             Drop\_priv: N

           Reload\_priv: N

         Shutdown\_priv: N

          Process\_priv: N

             File\_priv: N

            Grant\_priv: N

       References\_priv: N

            Index\_priv: N

            Alter\_priv: N

          Show\_db\_priv: N

            Super\_priv: Y

 Create\_tmp\_table\_priv: N

      Lock\_tables\_priv: N

          Execute\_priv: N

       Repl\_slave\_priv: N

      Repl\_client\_priv: N

      Create\_view\_priv: N

        Show\_view\_priv: N

   Create\_routine\_priv: N

    Alter\_routine\_priv: N

      Create\_user\_priv: N

            Event\_priv: N

          Trigger\_priv: N

Create\_tablespace\_priv: N

              ssl\_type: 

            ssl\_cipher: 

           x509\_issuer: 

          x509\_subject: 

         max\_questions: 0

           max\_updates: 0

       max\_connections: 0

  max\_user\_connections: 0

                plugin: caching\_sha2\_password

 authentication\_string: $A$005$THISISACOMBINATIONOFINVALIDSALTANDPASSWORDTHATMUSTNEVERBRBEUSED

      password\_expired: N

 password\_last\_changed: 2018-09-02 18:41:31

     password\_lifetime: NULL

        account\_locked: Y

      Create\_role\_priv: N

        Drop\_role\_priv: N

Password\_reuse\_history: NULL

   Password\_reuse\_time: NULL

\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\* 3. row \*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*

                  Host: localhost

                  User: mysql.sys

           Select\_priv: N

           Insert\_priv: N

           Update\_priv: N

           Delete\_priv: N

           Create\_priv: N

             Drop\_priv: N

           Reload\_priv: N

         Shutdown\_priv: N

          Process\_priv: N

             File\_priv: N

            Grant\_priv: N

       References\_priv: N

            Index\_priv: N

            Alter\_priv: N

          Show\_db\_priv: N

            Super\_priv: N

 Create\_tmp\_table\_priv: N

      Lock\_tables\_priv: N

          Execute\_priv: N

       Repl\_slave\_priv: N

      Repl\_client\_priv: N

      Create\_view\_priv: N

        Show\_view\_priv: N

   Create\_routine\_priv: N

    Alter\_routine\_priv: N

      Create\_user\_priv: N

            Event\_priv: N

          Trigger\_priv: N

Create\_tablespace\_priv: N

              ssl\_type: 

            ssl\_cipher: 

           x509\_issuer: 

          x509\_subject: 

         max\_questions: 0

           max\_updates: 0

       max\_connections: 0

  max\_user\_connections: 0

                plugin: caching\_sha2\_password

 authentication\_string: $A$005$THISISACOMBINATIONOFINVALIDSALTANDPASSWORDTHATMUSTNEVERBRBEUSED

      password\_expired: N

 password\_last\_changed: 2018-09-02 18:41:31

     password\_lifetime: NULL

        account\_locked: Y

      Create\_role\_priv: N

        Drop\_role\_priv: N

Password\_reuse\_history: NULL

   Password\_reuse\_time: NULL

\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\* 4. row \*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*

                  Host: localhost

                  User: root

           Select\_priv: Y

           Insert\_priv: Y

           Update\_priv: Y

           Delete\_priv: Y

           Create\_priv: Y

             Drop\_priv: Y

           Reload\_priv: Y

         Shutdown\_priv: Y

          Process\_priv: Y

             File\_priv: Y

            Grant\_priv: Y

       References\_priv: Y

            Index\_priv: Y

            Alter\_priv: Y

          Show\_db\_priv: Y

            Super\_priv: Y

 Create\_tmp\_table\_priv: Y

      Lock\_tables\_priv: Y

          Execute\_priv: Y

       Repl\_slave\_priv: Y

      Repl\_client\_priv: Y

      Create\_view\_priv: Y

        Show\_view\_priv: Y

   Create\_routine\_priv: Y

    Alter\_routine\_priv: Y

      Create\_user\_priv: Y

            Event\_priv: Y

          Trigger\_priv: Y

Create\_tablespace\_priv: Y

              ssl\_type: 

            ssl\_cipher: 

           x509\_issuer: 

          x509\_subject: 

         max\_questions: 0

           max\_updates: 0

       max\_connections: 0

  max\_user\_connections: 0

                plugin: caching\_sha2\_password

 authentication\_string: $A$005$T3MMC2E2\]H2:\)r\)K1kjTPSPItmIDBUsZec7aKUnFC/oUxbfUa8pTQ/B5WonD

      password\_expired: N

 password\_last\_changed: 2018-09-02 19:16:55

     password\_lifetime: NULL

        account\_locked: N

      Create\_role\_priv: Y

        Drop\_role\_priv: Y

Password\_reuse\_history: NULL

   Password\_reuse\_time: NULL

4 rows in set \(0.00 sec\)



ERROR: 

No query specified
```

---

```
mysql&gt; select Host,User from user \G;

\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\* 1. row \*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*

Host: localhost

User: mysql.infoschema

\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\* 2. row \*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*

Host: localhost

User: mysql.session

\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\* 3. row \*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*

Host: localhost

User: mysql.sys

\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\* 4. row \*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*

Host: localhost

User: root

4 rows in set \(0.01 sec\)



ERROR: 

No query specified
```

---

```
mysql&gt;  update user set host ='%' where Host = 'localhost' and User = 'root'; 

Query OK, 1 row affected \(0.03 sec\)

Rows matched: 1  Changed: 1  Warnings: 0
```

---

```
mysql&gt; flush privileges;

Query OK, 0 rows affected \(0.00 sec\)
```

---



