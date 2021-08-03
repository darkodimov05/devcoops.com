---
layout: post
title:  "How to import CSV file into MySQL table"
categories: [ mysql ]
image: assets/images/mysql-csv.jpg
tags: [ mysql, csv ]
---
Working with MySQL DB data can sometimes require exporting the data from one table and import it into another. Importing the data through MySQL Workbench or phpmyadmin can cause some limitation issues, so things will get complicated. In this tutorial, I'm gonna show you how to import CSV file into MySQL table, through a simple MySQL query.

## Prerequisites
* MySQL installed
* sudo access

## Import local CSV file into MySQL table
**Step 1**. Before importing the CSV file, make sure that the mysql table already exists. Execute the following commands to clarify:
```bash
mysql> USE database_name;
mysql> SHOW tables;
```

**Step 2**. If the table doesn't exists you can create it with the following command depending on your needs:
```bash
CREATE TABLE table_name (
    id INT NOT NULL AUTO_INCREMENT,
    column_1 VARCHAR(255) NOT NULL,
    column_2 DATE NOT NULL,
    column_3 DECIMAL(10 , 2 ) NULL,
    column_4 INTEGER,
    PRIMARY KEY (id)        
);
``` 

**Step 3**. And the final step to import the CSV file into the mysql table execute:
```bash
LOAD DATA LOCAL INFILE '/home/devcoops/devcoops-file.csv'
INTO TABLE table_name
FIELDS TERMINATED BY ',' 
LINES TERMINATED BY '\n';
```

## Conclusion
In this tutorial you will learn how to import CSV file into MySQL table, through a simple MySQL query. Feel free to leave a comment below and if you find this tutorial useful, follow our official channel on [telegram](https://t.me/devopsblogposts){:target="_blank"}.
