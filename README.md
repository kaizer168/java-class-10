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
firewall-cmd --zone=public --add-port=3306/tcp --permanent  
firewall-cmd --reload  
  
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
mysql> start slave;
Query OK, 0 rows affected (0.01 sec)
mysql> show slave status \G;
*************************** 1. row ***************************
               Slave_IO_State: Waiting for master to send event
                  Master_Host: 192.168.1.110
                  Master_User: root
                  Master_Port: 3306
                Connect_Retry: 60
              Master_Log_File: mysql-bin.000001
          Read_Master_Log_Pos: 2965
               Relay_Log_File: mysql-1-relay-bin.000002
                Relay_Log_Pos: 320
        Relay_Master_Log_File: mysql-bin.000001
             Slave_IO_Running: Yes
            Slave_SQL_Running: Yes
              Replicate_Do_DB:
          Replicate_Ignore_DB:
           Replicate_Do_Table:
       Replicate_Ignore_Table:
      Replicate_Wild_Do_Table:
  Replicate_Wild_Ignore_Table:
                   Last_Errno: 0
                   Last_Error:
                 Skip_Counter: 0
          Exec_Master_Log_Pos: 2965
              Relay_Log_Space: 529
              Until_Condition: None
               Until_Log_File:
                Until_Log_Pos: 0
           Master_SSL_Allowed: No
           Master_SSL_CA_File:
           Master_SSL_CA_Path:
              Master_SSL_Cert:
            Master_SSL_Cipher:
               Master_SSL_Key:
        Seconds_Behind_Master: 0
Master_SSL_Verify_Server_Cert: No
                Last_IO_Errno: 0
                Last_IO_Error:
               Last_SQL_Errno: 0
               Last_SQL_Error:
  Replicate_Ignore_Server_Ids:
             Master_Server_Id: 110
                  Master_UUID: 000ed271-5de8-11ed-a7f5-000c2902b4dc
             Master_Info_File: /var/lib/mysql/master.info
                    SQL_Delay: 0
          SQL_Remaining_Delay: NULL
      Slave_SQL_Running_State: Slave has read all relay log; waiting for more updates
           Master_Retry_Count: 86400
                  Master_Bind:
      Last_IO_Error_Timestamp:
     Last_SQL_Error_Timestamp:
               Master_SSL_Crl:
           Master_SSL_Crlpath:
           Retrieved_Gtid_Set:
            Executed_Gtid_Set:
                Auto_Position: 0
         Replicate_Rewrite_DB:
                 Channel_Name:
           Master_TLS_Version:
1 row in set (0.00 sec)

ERROR:
No query specified

  
## MySQL Slave 2 IP: 192.168.1.112  
vi /etc/my.cnf    
server-id=112  
[root@mysql-1 ~]# systemctl restart mysqld  
firewall-cmd --zone=public --add-port=3306/tcp --permanent  
firewall-cmd --reload  
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
  
mysql> start slave;
Query OK, 0 rows affected (0.01 sec)

mysql> show slave status \G;
*************************** 1. row ***************************
               Slave_IO_State: Waiting for master to send event
                  Master_Host: 192.168.1.110
                  Master_User: root
                  Master_Port: 3306
                Connect_Retry: 60
              Master_Log_File: mysql-bin.000001
          Read_Master_Log_Pos: 2965
               Relay_Log_File: mysql-2-relay-bin.000002
                Relay_Log_Pos: 320
        Relay_Master_Log_File: mysql-bin.000001
             Slave_IO_Running: Yes
            Slave_SQL_Running: Yes
              Replicate_Do_DB:
          Replicate_Ignore_DB:
           Replicate_Do_Table:
       Replicate_Ignore_Table:
      Replicate_Wild_Do_Table:
  Replicate_Wild_Ignore_Table:
                   Last_Errno: 0
                   Last_Error:
                 Skip_Counter: 0
          Exec_Master_Log_Pos: 2965
              Relay_Log_Space: 529
              Until_Condition: None
               Until_Log_File:
                Until_Log_Pos: 0
           Master_SSL_Allowed: No
           Master_SSL_CA_File:
           Master_SSL_CA_Path:
              Master_SSL_Cert:
            Master_SSL_Cipher:
               Master_SSL_Key:
        Seconds_Behind_Master: 0
Master_SSL_Verify_Server_Cert: No
                Last_IO_Errno: 0
                Last_IO_Error:
               Last_SQL_Errno: 0
               Last_SQL_Error:
  Replicate_Ignore_Server_Ids:
             Master_Server_Id: 110
                  Master_UUID: 000ed271-5de8-11ed-a7f5-000c2902b4dc
             Master_Info_File: /var/lib/mysql/master.info
                    SQL_Delay: 0
          SQL_Remaining_Delay: NULL
      Slave_SQL_Running_State: Slave has read all relay log; waiting for more updates
           Master_Retry_Count: 86400
                  Master_Bind:
      Last_IO_Error_Timestamp:
     Last_SQL_Error_Timestamp:
               Master_SSL_Crl:
           Master_SSL_Crlpath:
           Retrieved_Gtid_Set:
            Executed_Gtid_Set:
                Auto_Position: 0
         Replicate_Rewrite_DB:
                 Channel_Name:
           Master_TLS_Version:
1 row in set (0.00 sec)

ERROR:
No query specified

## MySQL Slave 3 IP: 192.168.1.113  
vi /etc/my.cnf    
server-id=113  
firewall-cmd --zone=public --add-port=3306/tcp --permanent  
firewall-cmd --reload  
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
  
mysql> start slave;
Query OK, 0 rows affected (0.01 sec)

mysql> show slave status \G;
*************************** 1. row ***************************
               Slave_IO_State: Waiting for master to send event
                  Master_Host: 192.168.1.110
                  Master_User: root
                  Master_Port: 3306
                Connect_Retry: 60
              Master_Log_File: mysql-bin.000001
          Read_Master_Log_Pos: 2965
               Relay_Log_File: mysql-3-relay-bin.000002
                Relay_Log_Pos: 320
        Relay_Master_Log_File: mysql-bin.000001
             Slave_IO_Running: Yes
            Slave_SQL_Running: Yes
              Replicate_Do_DB:
          Replicate_Ignore_DB:
           Replicate_Do_Table:
       Replicate_Ignore_Table:
      Replicate_Wild_Do_Table:
  Replicate_Wild_Ignore_Table:
                   Last_Errno: 0
                   Last_Error:
                 Skip_Counter: 0
          Exec_Master_Log_Pos: 2965
              Relay_Log_Space: 529
              Until_Condition: None
               Until_Log_File:
                Until_Log_Pos: 0
           Master_SSL_Allowed: No
           Master_SSL_CA_File:
           Master_SSL_CA_Path:
              Master_SSL_Cert:
            Master_SSL_Cipher:
               Master_SSL_Key:
        Seconds_Behind_Master: 0
Master_SSL_Verify_Server_Cert: No
                Last_IO_Errno: 0
                Last_IO_Error:
               Last_SQL_Errno: 0
               Last_SQL_Error:
  Replicate_Ignore_Server_Ids:
             Master_Server_Id: 110
                  Master_UUID: 000ed271-5de8-11ed-a7f5-000c2902b4dc
             Master_Info_File: /var/lib/mysql/master.info
                    SQL_Delay: 0
          SQL_Remaining_Delay: NULL
      Slave_SQL_Running_State: Slave has read all relay log; waiting for more updates
           Master_Retry_Count: 86400
                  Master_Bind:
      Last_IO_Error_Timestamp:
     Last_SQL_Error_Timestamp:
               Master_SSL_Crl:
           Master_SSL_Crlpath:
           Retrieved_Gtid_Set:
            Executed_Gtid_Set:
                Auto_Position: 0
         Replicate_Rewrite_DB:
                 Channel_Name:
           Master_TLS_Version:
1 row in set (0.00 sec)

ERROR:
No query specified

  
