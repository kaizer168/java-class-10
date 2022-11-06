## Master 0 IP: 192.168.1.110  
vi /etc/my.cnf  
sync_binlog=1  
binlog-do-db=hello  
binlog-ignore-db=mysql  
log-bin=mysql-bin  
server-id=110  
