MySQL Workbench Step by Step Guide
==================================

MySQL Workbench is for
1. SQL Development
	for working with queries, table-data and scripts
2. Data Modeling
	for Designing Data-Models & Diagrams
3. Server Administration
	for Monitoring, Configuring, and Securing the servers
	
	
SQL Development
===============
To use the "SQL Development"-module create  "New Connection" or use an existing one.
In Windows, Workbench detects if you have MySQL on your machine, and creates connections, if you have.

Create "New Connection":
* Hostname+Port identify the server-instance you want to connect to.
* "Default Schema" indicates the active Database when the session opens. (useful when you frequently connect to the same DB)
* You can also set "per connection" options, e.g. enable compression,  Password saved/ or enter it everytime we connect?
* you can work with all stored "connections" with "Manage Connections" Dialog, that displays all connections with their settings
  useful if you connect to several sql-instances across a network

Double-Click the "Connection" to open the SQL-Editor Tab, which contains:
* a "query pane" for typing and executing queries
* a "SQL Additions pane" for containing SQL-Statements 
* an Output-Pane 
* Object-Browser to display meta-data

SQL-EDITOR:
------------
* Queries are executed against the "default"-DB, chosen by the "connection" or by double-clicking the schema.
* After selecting a database, you can execute statements against that database.
* In the query pane you can write as many statements as you wish, and execute 
  all of them or only the one where the cursor is. (See also Toolbar-icons)
* When executing multiple queries, each one creates its results-tab.
* reformat sql statements 
* plugin to make keywords UPPERCASE (best-practise)
 
 
Object-Browser:
--------------
* shows schemas and their structural contents
* and lets you see the object information for dbs, tables, columns and other objects.
* when you click the table it shows you its information
* shows other structural info like: --> index attached to the table and 
									--> names and definition of any foreign key.
* It lets you generate the "Create" statement for objects like "tables" and "stored routines".
 
"SQL-Additions" Pane
---------------------
 * its on the right of the editor 
 * for saving your own snippets of SQL-CODE
 * or getting existing snippets.
 * e.g.: for db-management various SHOW statements
		 for DDL: CREATE, ALTER, DROP snippets
		 and DML statements for querying and modifying data
 
 
Output-Pane
------------ 
 * records statements that have been executed
 * shows rows that have been returned.
 
EDITING DATA in the Database:
* you can even edit data in the Output-Pane (if its not aggregated and has a unique key)
* or by right-clicking a table in the Object-Browser --> "Edit table data"
 
DATA-MODELING
==============
* The Data-Modeling-Module lets you work with EER-Models.
* You can create a Model out of an existing Database by reverse-engineering it (Database --> "Reverse Engineer...")
* Each Model can have multiple diagramms (EER-Diagram)
* After making change to the schema, you can apply those changes to the DB by "Database"-->"Synchronize Model"
 you can sync in both directions:
 Model  <--> Database
 
 Forward Engineering:<br>
 Create a Model in the tool and use it to create the underlying database
 without having to write the CREATE table statements.
 
SERVER-Administration
=====================
 * for monitoring & configuring the server-instance
 * it has graphs for system and mysql metrics
 * list of connections and their status
 * you can start & stop an instance from within workbench
 * view status and system-variables of the running server-instance
 * view Server Logs (error-logs, binary-logs,..)
 * startup-options (Configuration-->option file)
 * configure users, their roles and other privileges
 * export& import data from within workbench
   --> exports each table to a separate file // export to a single file
 * import reads file
 
 
 
 STEPS:
 =====================
1. File --> New Model (gives us a Model of the Database)
2. right-click DB (mysql), --> "edit Schema"- change name e.g "myCMS"
3. you can create several diagrams for one DB. 
	* create diagramm "Add Diagram"
	* from the "Catalog Tree" you can drop out the tables
	* there you can create relations
	* create a new table (icon, in iconbar) + columns
	* Modeling Additions (templates)
4. Collation to utf8_general_ci (ci means case sensitive)
5. "Add Table" --> Name: "users" and add fields<br>
 **PK** primary key<br>
 **NN** Not Null<br>
 **UN** Unsigned cant be negative<br>
 **AI** Auto-increment <br>
6. "Add Table" --> Name: "sales" and add fields
7. "Add View" ???
8. "Add Routine" process data

 
When you create a table, your PRIMARY KEY becomes the **index**.

**Entities:**<br>
* Singular/Plural -->choose one!
* PascalCase,Camelcase, lowercase  --> choose one!

## Data Types CHEATSHEET (TODO)
* TINYINT
* MEDIUMINT

* Unicode for international language
* Large Binary Objects BLOBS 
* Character Large Objects CLOBS (Large amount of text )



**Cascading delete:**<br>
when you delete a row from a table, then it will delete all related rows in other tables.

**Cascading nullify:**<br>
when you delete a row from a table, then it will set the ID all related rows in other tables to NULL.

**NO ACTION:**<br>
when you delete a row from a tabe, then no action will be done to related rows.


Normalization:
==============
**1NF** says that each of your columns and each of your tables should contain one value just one value, and there should be no repeating groups<br>
The **2NF** has the rather puzzling official description that any non-key field should be dependent on the entire primary key. (is needed when you use composite key)<br>
**3NF**  it's that no non-key field, meaning, a column that is not part of the primary key is dependent on another non-key field. I


###INDEX:
**Clustered Index:**<br>
The most basic, the first index that's created, the primary index on any table is what's called the clustered index.
And that means pick a column as the clustered index and the database will order the data in that table based on that column. 
Meaning that on that physical disk itself, where these rows of data are stored as bits and bytes, they're actually sequenced that way.

**Secondary Index:**<br>
I can create a secondary index, a non-clustered index. 
This is more like having an index at the back of the textbook. 
It's created as a separate piece by itself with its own meaningful order. 
So in this case, I'm imagining I have this index existing, it's sorting now by last name ascending, the way that we can't actually do in the regular table, 
because we're already sorting by CustomerID. We're basically creating a map here of how we can go to LastName.

Stored Procedure:
=================
A statement that we want to reuse.<br>
```
CREATE PROCEDURE HighlyPaid()
	 SELECT * FROM Employee
	 WHERE Salary >50000
	 ORDER BY LastName, FirstName
END;
CALL HighlyPaid();
```


BACKUP/ EXPORTING / DUMP:
=========================

1. On the "HOME"-view select one of the MySQL Connections: (localhost)
2. In the "Localhost" view click on "Server"--> "Data export"
3. In the "Data Export" view select the table(s) and whether you want to export only their structure, or structure and data
4. Click "Start Export"
 
 

# User Defined variables: 
z.B.: 
```
SET @rank := 0;
UPDATE tab 
SET Position=(@rank := @rank + 10)
WHERE NodeId = 2
```
Sind 2 statements! müssen in prepared statement separat aufgerufen werden.
 
Combining 2 updates in 1 query:
```
UPDATE albums SET isFeatured = '0' WHERE isFeatured = '1'
UPDATE albums SET isFeatured = '1' WHERE id = '$id'
= 
UPDATE albums SET isFeatured = IF(id!='$id', '0','1')
```

## Links 
[MySQL Workbench Tutorial - youtube](https://www.youtube.com/watch?v=X_umYKqKaF0) <br>
[Introduction to MySQL Workbench - youtube](https://www.youtube.com/watch?v=RSHevYMwCVw) <br>
 

 
 
 
 
 
 
 
 
 
 
 


	