---
title: Save MySQL query results into a text or CSV file
date: 2017-03-10 17:37:27
tags:
---
MySQL provides an easy mechanism for writing the results of a select statement into a text file on the server. Using extended options of the INTO OUTFILE nomenclature, it is possible to create a comma separated value (CSV) which can be imported into a spreadsheet application such as OpenOffice or Excel or any other applciation which accepts data in CSV format.


Given a query such as

```
SELECT order_id,product_name,qty FROM orders
```

which returns three columns of data, the results can be placed into the file /tmo/orders.txt using the query:

```
SELECT order_id,product_name,qty FROM orders
INTO OUTFILE '/tmp/orders.txt'
```

This will create a tab-separated file, each row on its own line. To alter this behavior, it is possible to add modifiers to the query:

```
SELECT order_id,product_name,qty FROM orders
INTO OUTFILE '/tmp/orders.csv'
FIELDS TERMINATED BY ','
ENCLOSED BY '"'
LINES TERMINATED BY '\n'
```

In this example, each field will be enclosed in “double quotes,” the fields will be separated by commas, and each row will be output on a new line separated by a newline (\n). Sample output of this command would look like:
```
"1","Tech-Recipes sock puppet","14.95" "2","Tech-Recipes chef's hat","18.95"
...
```
Keep in mind that the output file must not already exist and that the user MySQL is running as has write permissions to the directory MySQL is attempting to write the file to.
