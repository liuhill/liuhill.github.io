---
title: sphinx scws 全文检索安装配置详解
date: 2018-01-29 09:45:21
tags: 
    - scws
    - sphinx
    - 中文分词
    - 全文检索
categories:
  - 技术
---
>转载： http://blog.51yip.com/mysql/1659.html

环境：centos 6.5 x86，php 5.3.3，mysql 5.5.8
### 一，下载sphinx,scws相关包

sphinx下载地址：http://sphinxsearch.com/downloads/release/

sphinx php扩展下载地址：http://pecl.php.net/package/sphinx

scws下载地址：http://www.xunsearch.com/scws/download.php

scws词库下载地址：http://www.xunsearch.com/scws/down/scws-dict-chs-utf8.tar.bz2

### 二，安装sphinx,scws,以及php扩展

- 1,安装sphinx
```
# tar zxvf sphinx-2.2.5-release.tar.gz  
# cd sphinx-2.2.5-release  
# ./configure --prefix=/usr/local/sphinx2 --with-mysql=/usr/local/mysql  
# make && make install  
```
- 2,安装sphinx客户端
```
# cd api/libsphinxclient   //sphinx-2.2.5-release目录下  
# ./configure --prefix=/usr/local/sphinx2/libsphinxclient  
# make && make install  
```
- 3,安装sphinx php扩展

```
# tar zxvf sphinx-1.3.1.tgz  
# cd sphinx-1.3.1  
# phpize  
# ./configure --with-sphinx=/usr/local/sphinx2/libsphinxclient --with-php-config=/usr/bin/php-config  
# make && make install  
```

- 4,安装scws

```
# tar xvjf scws-1.2.2.tar.bz2  
# mkdir /usr/local/scws  
# cd scws-1.2.2  
# ./configure --prefix=/usr/local/scws/  
# make && make install  

```
- 5,安装scws php扩展

```
# cd ./phpext/  
# phpize  
# ./configure --with-php-config=/usr/bin/php-config  
# make && make install  

```
### 三，配置sphinx,scws,php等

- 1,创建测试表和数据

```
mysql> desc users;  
+----------+-------------+------+-----+---------+----------------+  
| Field | Type | Null | Key | Default | Extra |  
+----------+-------------+------+-----+---------+----------------+  
| user_id | int(11) | NO | PRI | NULL | auto_increment |  
| username | varchar(20) | NO | | NULL | |  
+----------+-------------+------+-----+---------+----------------+  
2 rows in set (0.00 sec)  

mysql> select * from users;  
+------------+------------+  
| user_id | username |  
+------------+------------+  
| 1311895262 | 张三 |  
| 1311895263 | tank张二 |  
| 1311895264 | tank张一 |  
| 1311895265 | tank张 |  
+------------+------------+  
4 rows in set (0.00 sec)  
mysql> desc orders;  
+--------------+-------------+------+-----+---------+----------------+  
| Field | Type | Null | Key | Default | Extra |  
+--------------+-------------+------+-----+---------+----------------+  
| id | int(11) | NO | PRI | NULL | auto_increment |  
| user_id | int(11) | NO | | NULL | |  
| create_time | datetime | NO | | NULL | |  
| product_name | varchar(20) | NO | | NULL | |  
| summary | text | NO | | NULL | |  
+--------------+-------------+------+-----+---------+----------------+  
5 rows in set (0.00 sec)  

mysql> select * from orders;  
+----+------------+---------------------+----------------+--------------+  
| id | user_id | create_time | product_name | summary |  
+----+------------+---------------------+----------------+--------------+  
| 9 | 1311895262 | 2014-08-01 00:24:54 | tank is 坦克 | 技术总监 |  
| 10 | 1311895263 | 2014-08-01 00:24:54 | tank is 坦克 | 技术经理 |  
| 11 | 1311895264 | 2014-08-01 00:24:54 | tank is 坦克 | DNB经理 |  
| 12 | 1311895265 | 2014-08-01 00:24:54 | tank is 坦克 | 运维总监 |  
+----+------------+---------------------+----------------+--------------+  
4 rows in set (0.00 sec)  
```
上面二张表，都是真实的mysql表

- 2,配置sphinx.conf，加上以下内容

```
source myorder  
{  
 type = mysql  
 sql_host = localhost  
 sql_user = root  
 sql_pass =  
 sql_db = test  
 sql_query_pre = SET NAMES utf8  
 sql_query_pre = SET SESSION query_cache_type=OFF  
 sql_query = \  
 SELECT a.id, a.user_id,b.username, UNIX_TIMESTAMP(a.create_time) AS create_time, a.product_name, a.summary \  
 FROM orders a left join users b on a.user_id = b.user_id  
 sql_attr_uint = user_id  
 sql_field_string = username  
 sql_field_string = product_name  
 sql_attr_timestamp = create_time  

 sql_ranged_throttle = 0  
 #sql_query_info = SELECT * FROM orders WHERE id=$id  
}  

index myorder  
{  
 source = myorder  
 path = /usr/local/sphinx2/var/data/myorder  
 docinfo = extern  
 mlock = 0  
 morphology = none  
 min_word_len = 1  
 #charset_type = zh_cn.utf-8  
 html_strip = 1  
 charset_table = U+FF10..U+FF19->0..9, 0..9, U+FF41..U+FF5A->a..z, U+FF21..U+FF3A->a..z,A..Z->a..z, a..z, U+0149, U+017F, U+0138, U+00DF, U+00FF, U+00C0..U+00D6->U+00E0..U+00F6,U+00E0..U+00F6, U+00D8..U+00DE->U+00F8..U+00FE, U+00F8..U+00FE, U+0100->U+0101, U+0101,U+0102->U+0103, U+0103, U+0104->U+0105, U+0105, U+0106->U+0107, U+0107, U+0108->U+0109,U+0109, U+010A->U+010B, U+010B, U+010C->U+010D, U+010D, U+010E->U+010F, U+010F,U+0110->U+0111, U+0111, U+0112->U+0113, U+0113, U+0114->U+0115, U+0115, U+0116->U+0117,U+0117, U+0118->U+0119, U+0119, U+011A->U+011B, U+011B, U+011C->U+011D, U+011D,U+011E->U+011F, U+011F, U+0130->U+0131, U+0131, U+0132->U+0133, U+0133, U+0134->U+0135,U+0135, U+0136->U+0137, U+0137, U+0139->U+013A, U+013A, U+013B->U+013C, U+013C,U+013D->U+013E, U+013E, U+013F->U+0140, U+0140, U+0141->U+0142, U+0142, U+0143->U+0144,U+0144, U+0145->U+0146, U+0146, U+0147->U+0148, U+0148, U+014A->U+014B, U+014B,U+014C->U+014D, U+014D, U+014E->U+014F, U+014F, U+0150->U+0151, U+0151, U+0152->U+0153,U+0153, U+0154->U+0155, U+0155, U+0156->U+0157, U+0157, U+0158->U+0159, U+0159,U+015A->U+015B, U+015B, U+015C->U+015D, U+015D, U+015E->U+015F, U+015F, U+0160->U+0161,U+0161, U+0162->U+0163, U+0163, U+0164->U+0165, U+0165, U+0166->U+0167, U+0167,U+0168->U+0169, U+0169, U+016A->U+016B, U+016B, U+016C->U+016D, U+016D, U+016E->U+016F,U+016F, U+0170->U+0171, U+0171, U+0172->U+0173, U+0173, U+0174->U+0175, U+0175,U+0176->U+0177, U+0177, U+0178->U+00FF, U+00FF, U+0179->U+017A, U+017A, U+017B->U+017C,U+017C, U+017D->U+017E, U+017E, U+0410..U+042F->U+0430..U+044F, U+0430..U+044F,U+05D0..U+05EA, U+0531..U+0556->U+0561..U+0586, U+0561..U+0587, U+0621..U+063A, U+01B9,U+01BF, U+0640..U+064A, U+0660..U+0669, U+066E, U+066F, U+0671..U+06D3, U+06F0..U+06FF,U+0904..U+0939, U+0958..U+095F, U+0960..U+0963, U+0966..U+096F, U+097B..U+097F,U+0985..U+09B9, U+09CE, U+09DC..U+09E3, U+09E6..U+09EF, U+0A05..U+0A39, U+0A59..U+0A5E,U+0A66..U+0A6F, U+0A85..U+0AB9, U+0AE0..U+0AE3, U+0AE6..U+0AEF, U+0B05..U+0B39,U+0B5C..U+0B61, U+0B66..U+0B6F, U+0B71, U+0B85..U+0BB9, U+0BE6..U+0BF2, U+0C05..U+0C39,U+0C66..U+0C6F, U+0C85..U+0CB9, U+0CDE..U+0CE3, U+0CE6..U+0CEF, U+0D05..U+0D39, U+0D60,U+0D61, U+0D66..U+0D6F, U+0D85..U+0DC6, U+1900..U+1938, U+1946..U+194F, U+A800..U+A805,U+A807..U+A822, U+0386->U+03B1, U+03AC->U+03B1, U+0388->U+03B5, U+03AD->U+03B5,U+0389->U+03B7, U+03AE->U+03B7, U+038A->U+03B9, U+0390->U+03B9, U+03AA->U+03B9,U+03AF->U+03B9, U+03CA->U+03B9, U+038C->U+03BF, U+03CC->U+03BF, U+038E->U+03C5,U+03AB->U+03C5, U+03B0->U+03C5, U+03CB->U+03C5, U+03CD->U+03C5, U+038F->U+03C9,U+03CE->U+03C9, U+03C2->U+03C3, U+0391..U+03A1->U+03B1..U+03C1,U+03A3..U+03A9->U+03C3..U+03C9, U+03B1..U+03C1, U+03C3..U+03C9, U+0E01..U+0E2E,U+0E30..U+0E3A, U+0E40..U+0E45, U+0E47, U+0E50..U+0E59, U+A000..U+A48F, U+4E00..U+9FBF,U+3400..U+4DBF, U+20000..U+2A6DF, U+F900..U+FAFF, U+2F800..U+2FA1F, U+2E80..U+2EFF,U+2F00..U+2FDF, U+3100..U+312F, U+31A0..U+31BF, U+3040..U+309F, U+30A0..U+30FF,U+31F0..U+31FF, U+AC00..U+D7AF, U+1100..U+11FF, U+3130..U+318F, U+A000..U+A48F,U+A490..U+A4CF  
 ngram_len = 1  
 ngram_chars = U+4E00..U+9FBF, U+3400..U+4DBF, U+20000..U+2A6DF, U+F900..U+FAFF,U+2F800..U+2FA1F, U+2E80..U+2EFF, U+2F00..U+2FDF, U+3100..U+312F, U+31A0..U+31BF,U+3040..U+309F, U+30A0..U+30FF,U+31F0..U+31FF, U+AC00..U+D7AF, U+1100..U+11FF,U+3130..U+318F, U+A000..U+A48F, U+A490..U+A4CF  
}  
```
___注意:___ 新的sphinx，不支持sql_query_info，charset_type设置了，

```
WARNING: key 'sql_query_info' was permanently removed from Sphinx configuration. Refer to documentation for details.
WARNING: key 'charset_type' was permanently removed from Sphinx configuration. Refer to documentation for details.
```
- 3,安装scws词库

```
# tar xvjf scws-dict-chs-utf8.tar.bz2 -C /usr/local/scws/etc/  

# chown tank:tank /usr/local/scws/etc/dict.utf8.xdb  
```
在这里一定要加权限，也就是说让php-fpm或者php-cgi的运行用户，拥有dict.utf8.xdb的所有权限。如果不这么做的话，php 扩展调用词库会报如下错误：
```
Warning: SimpleCWS::add_dict(): Failed to add the dict file
```
怎么查看php-fpm,php-cgi的运行用户呢？
```
# ps aux |grep php-fpm  
root 23487 0.0 0.1 284928 4652 ? Ss Nov05 0:00 php-fpm: master process (/etc/php-fpm.conf)  
tank 23488 0.0 1.3 336108 52328 ? S Nov05 0:02 php-fpm: pool www  //在这里就是tank了  
tank 23489 0.0 0.8 310484 34028 ? S Nov05 0:02 php-fpm: pool www  
tank 23490 0.0 0.7 306620 30156 ? S Nov05 0:02 php-fpm: pool www  
tank 23491 0.0 0.8 310096 33748 ? S Nov05 0:02 php-fpm: pool www  
tank 23492 0.0 1.2 331812 47712 ? S Nov05 0:02 php-fpm: pool www  
tank 24669 0.0 1.2 333520 48896 ? S Nov05 0:01 php-fpm: pool www  
tank 29747 0.0 0.7 305000 27340 ? S 03:27 0:00 php-fpm: pool www  
tank 29761 0.0 1.0 320536 39928 ? S 03:27 0:00 php-fpm: pool www  
root 30705 0.0 0.0 103260 872 pts/5 S+ 04:11 0:00 grep php-fpm  
```
4,配置php.ini
```
# vim /etc/php.ini  
[sphinx]  
extension = sphinx.so  

[scws]  
extension = scws.so  
scws.default.charset = utf-8  
scws.default.fpath = /usr/local/scws/etc  
```

### 四，启动sphinx,php-fpm

- 1,启动sphinx
```
# /usr/local/sphinx2/bin/indexer --config /usr/local/sphinx2/etc/sphinx.conf --all  
# /usr/local/sphinx2/bin/searchd --config /usr/local/sphinx2/etc/sphinx.conf  
```
- 2,重启php-fpm
```
# /etc/init.d/php-fpm restart  
```
前二次，我安装sphinx，必须在mysql中安装sphinx存储插件，而这次没有，看下图

{% qnimg sphinx_no_plugin.jpg %}

### 五，测试sphinx全文检索

- 1,命令行的测试

```
[root@localhost phpext]# mysql -h 127.0.0.1 -P 9306  
Welcome to the MySQL monitor. Commands end with ; or \g.  
Your MySQL connection id is 1  
Server version: 2.2.5-id64-release (r4825)  

Copyright (c) 2000, 2013, Oracle and/or its affiliates. All rights reserved.  

Oracle is a registered trademark of Oracle Corporation and/or its  
affiliates. Other names may be trademarks of their respective  
owners.  

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.  

mysql> select * from myorder where match('张');  
+------+------------+------------+-------------+----------------+  
| id | user_id | username | create_time | product_name |  
+------+------------+------------+-------------+----------------+  
| 9 | 1311895262 | 张三 | 1406823894 | tank is 坦克 |  
| 10 | 1311895263 | tank张二 | 1406823894 | tank is 坦克 |  
| 11 | 1311895264 | tank张一 | 1406823894 | tank is 坦克 |  
| 12 | 1311895265 | tank张 | 1406823894 | tank is 坦克 |  
+------+------------+------------+-------------+----------------+  
4 rows in set (0.00 sec)  
```
- 2,利用php 扩展
```
<?php  
 header("Content-type: text/html; charset=utf-8");  
 $b_time = microtime(true);  
 echo '<p>'.$b_time.'</p>';  
 $key = "张三";  
 $index = "myorder";  
 //========================================分词  

 $so = scws_new();  
 $so->set_charset('utf-8');  
 //默认词库  
 $so->add_dict(ini_get('scws.default.fpath') . '/dict.utf8.xdb');  
 //自定义词库  
// $so->add_dict('./dd.txt',SCWS_XDICT_TXT);  
 //默认规则  
 $so->set_rule(ini_get('scws.default.fpath') . '/rules.utf8.ini');  

 //设定分词返回结果时是否去除一些特殊的标点符号  
 $so->set_ignore(true);  

 //设定分词返回结果时是否复式分割，如“中国人”返回“中国＋人＋中国人”三个词。  
 // 按位异或的 1 | 2 | 4 | 8 分别表示: 短词 | 二元 | 主要单字 | 所有单字  
 //1,2,4,8 分别对应常量 SCWS_MULTI_SHORT SCWS_MULTI_DUALITY SCWS_MULTI_ZMAIN SCWS_MULTI_ZALL  
 $so->set_multi(false);  

 //设定是否将闲散文字自动以二字分词法聚合  
 $so->set_duality(false);  

 //设定搜索词  
 $so->send_text($key);  
 $words_array = $so->get_result();  
 $words = "";  
 foreach($words_array as $v)  
 {  
 $words = $words.'|('.$v['word'].')';  
 }  

 //加入全词  
 #$words = '('.$key.')'.$words;  
 $words = trim($words,'|');  
 $so->close();  
 echo '<p>输入：'.$key.'</p>';  
 echo '<p>分词：'.$words.'</p>';  
//========================================搜索  
 $sc = new SphinxClient();  
 $sc->SetServer('127.0.0.1',9312);  
 #$sc->SetMatchMode(SPH_MATCH_ALL);  
 $sc->SetMatchMode(SPH_MATCH_EXTENDED);  
 $sc->SetArrayResult(TRUE);  
 $res = $sc->Query($words,$index);  
 print_r($res);  
 $e_time = microtime(true);  
 $time = $e_time - $b_time;  
 echo '<p>'.$e_time.'</p>';  

 echo '<p>'.$time.'</p>';  
 exit;  
?>
```
结果如下：

```
<p>1415214126.9106</p><p>输入：张三</p><p>分词：(张三)</p>Array  
(  
 [error] =>  
 [warning] =>  
 [status] => 0  
 [fields] => Array  
 (  
 [0] => username  
 [1] => product_name  
 [2] => summary  
 )  

 [attrs] => Array  
 (  
 [user_id] => 1  
 [username] => 7  
 [create_time] => 2  
 [product_name] => 7  
 )  

 [matches] => Array  
 (  
 [0] => Array  
 (  
 [id] => 9  
 [weight] => 2500  
 [attrs] => Array  
 (  
 [user_id] => 1311895262  
 [username] => 张三  
 [create_time] => 1406823894  
 [product_name] => tank is 坦克  
 )  

 )  

 )  

 [total] => 1  
 [total_found] => 1  
 [time] => 0  
 [words] => Array  
 (  
 [张] => Array  
 (  
 [docs] => 4  
 [hits] => 4  
 )  

 [三] => Array  
 (  
 [docs] => 1  
 [hits] => 1  
 )  

 )  

)  
<p>1415214126.9516</p><p>0.041085958480835</p>  
```
