# MySQL

```bash
ssh username@192.168.0.1 	# use ssh to connect to server. 
sudo su root    			# get root rights
exit            			# log out current user
```

##  Commands 
 
| Command                | Description  | 
|------------------------|:-------------|
| mysql -u username -p   | Login | 
| show databases;        | show all databases   | 
| use database_name;     | uses that database, you can now add and remove tables,.. | 
| show tables;           | show all tables of current database | 
| describe table_name;   | get the table structure (colum-names, properties,..) | 
| **SELECT** column_name1, column_name2 <br>**FROM** table_name;| retrieve records from one or more tables | 
| **UPDATE** table_name <br>**SET** column_name=123455;| modify rows in a table | 