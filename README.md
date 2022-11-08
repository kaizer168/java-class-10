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
