---
title: this is incompatible with sql_mode=only_full_group_by
date: 2016-09-24 22:57:18
tags:
---
I was working on a legacy project recently and needed to import some data from MySQL 5.5. All the queries in the code worked perfectly in MySQL 5.5, so I assumed an upgrade to 5.7 would be seamless. Not so.

First I got errors due to DateTime columns being populated with zeros during import, then when running this query:

```
select *
  from ebay_order_items
  where
    z_shipmentno is null
    and ExternalTransactionID is not null
    and orderstatus = 'Completed'
    and timestamp > '2015-02-25'
  group by ExternalTransactionID
  order by timestamp desc
```

I got this:
```
Expression #1 of SELECT list is not in GROUP BY
clause and contains nonaggregated column
'1066export.ebay_order_items.TransactionID' which
is not functionally dependent on columns in GROUP BY
clause; this is incompatible with sql_mode=only_full_group_by
```

It turned out that the only_full_group_by mode was made default in version 5.7.5., which breaks many of such naïve queries. In fact, see here for more info.

I could rewrite all queries to be 5.7 compatible (hat tip to George Fekete for the solution) and do something as atrocious as this:

```
select extf.* from (
	select ExternalTransactionID
	from ebay_order_items
	where ExternalTransactionID is not null
	group by ExternalTransactionID
) extf JOIN ebay_order_items eoi
ON (eoi.ExternalTransactionID = extf.ExternalTransactionID)
where eoi.z_shipmentno is null
and eoi.orderstatus = 'Completed'
and eoi.timestamp > '2015-02-25'
order by eoi.timestamp desc
```
… but this would make the already complex refactoring much more difficult. It would be much better to temporarily disable these checks and force MySQL to act like it did in 5.6.

## Permanently changing SQL mode
First, we find out which configuration file our MySQL installation prefers. For that, we need the binary’s location:
```
$ which mysqld
/usr/sbin/mysqld
```
Then, we use this path to execute the lookup:

```
$ /usr/sbin/mysqld --verbose --help | grep -A 1 "Default options"

Default options are read from the following files in the given order:
/etc/my.cnf /etc/mysql/my.cnf ~/.my.cnf
```
We can see that the first favored configuration file is one in the root of the etc folder. That file, however, did not exist on my system so I opted for the second one.

First, we find out the current sql mode:

```
mysql -u homestead -psecret -e "select @@sql_mode"

+-------------------------------------------------------------------------------------------------------------------------------------------+
| @@sql_mode                                                                                                                                |
+-------------------------------------------------------------------------------------------------------------------------------------------+
| ONLY_FULL_GROUP_BY,STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION |
+-------------------------------------------------------------------------------------------------------------------------------------------+
```
Then, we copy the current string this query produced and remove everything we don’t like. In my case, I needed to get rid of NO_ZERO_IN_DATE, NO_ZERO_DATE and of course ONLY_FULL_GROUP_BY. The newly formed string then looks like this:

```
STRICT_TRANS_TABLES,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION

```
We open the configuration file we decided on before (/etc/mysql/my.cnf) and add the following line into the [mysqld] section:

```
[mysqld]
# ... other stuff will probably be here
sql_mode = "STRICT_TRANS_TABLES,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION"

```
Save, exit, and restart MySQL:

```
sudo service mysql restart
```
Voilà, the SQL mode is permanently changed and I can continue developing the legacy project until I need some additional strictness.
