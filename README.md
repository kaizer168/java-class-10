## MySQL Master 0 IP: 192.168.1.110  
vi /etc/my.cnf  
sync_binlog=1  
binlog-do-db=hello  
binlog-ignore-db=mysql  
log-bin=mysql-bin  
server-id=110  

firewall-cmd --zone=public --add-port=3306/tcp --permanent  
firewall-cmd --reload  

mysql -uroot -p  
Enter password:  
mysql> grant replication slave on \*.\* to 'root'@'192.168.1.111' identified by '<password>';  
Query OK, 0 rows affected, 1 warning (0.03 sec)  

mysql> grant replication slave on \*.\* to 'root'@'192.168.1.112' identified by '<password>';  
Query OK, 0 rows affected, 1 warning (0.00 sec)  

mysql> grant replication slave on \*.\* to 'root'@'192.168.1.113' identified by '<password>';  
Query OK, 0 rows affected, 1 warning (0.00 sec)  

mysql> flush privileges;  
Query OK, 0 rows affected (0.00 sec)  

mysql> show master status;  
+------------------+----------+--------------+------------------+-------------------+  
| File             | Position | Binlog_Do_DB | Binlog_Ignore_DB | Executed_Gtid_Set |  
+------------------+----------+--------------+------------------+-------------------+  
| mysql-bin.000001 |     2965 | hello        | mysql            |                   |  
+------------------+----------+--------------+------------------+-------------------+  
1 row in set (0.00 sec)  

## MySQL Slave 1 IP: 192.168.1.111  
vi /etc/my.cnf    
server-id=111  
[root@mysql-1 ~]# systemctl restart mysqld  
  [root@mysql-3 ~]# mysql -uroot -p  
Enter password:  

mysql> change master to  
    -> master_host='192.168.1.110',  
    -> master_port=3306,  
    -> master_user='root',  
    -> master_password='<password>',  
    -> master_log_file='mysql-bin.000001',  
    -> master_log_pos=2965,  
    -> MASTER_AUTO_POSITION=0;  
Query OK, 0 rows affected, 2 warnings (0.01 sec)  

  
## MySQL Slave 2 IP: 192.168.1.112  
vi /etc/my.cnf    
server-id=112  
[root@mysql-1 ~]# systemctl restart mysqld  
  [root@mysql-3 ~]# mysql -uroot -p  
Enter password:  

mysql> change master to  
    -> master_host='192.168.1.110',  
    -> master_port=3306,  
    -> master_user='root',  
    -> master_password='<password>',  
    -> master_log_file='mysql-bin.000001',  
    -> master_log_pos=2965,  
    -> MASTER_AUTO_POSITION=0;  
Query OK, 0 rows affected, 2 warnings (0.01 sec)  
  
## MySQL Slave 3 IP: 192.168.1.113  
vi /etc/my.cnf    
server-id=113  
[root@mysql-1 ~]# systemctl restart mysqld  
  [root@mysql-3 ~]# mysql -uroot -p  
Enter password:  

mysql> change master to  
    -> master_host='192.168.1.110',  
    -> master_port=3306,  
    -> master_user='root',  
    -> master_password='<password>',  
    -> master_log_file='mysql-bin.000001',  
    -> master_log_pos=2965,  
    -> MASTER_AUTO_POSITION=0;  
Query OK, 0 rows affected, 2 warnings (0.01 sec)  
  
