---
title: sphinx-scws-全文检索(PHP7、MariaDB)
date: 2018-01-29 09:45:21
tags: 
    - scws
    - sphinx
    - 中文分词
    - 全文检索
categories:
  - 技术
---

环境：centos 7 x86_64，php 7.1.7， MariaDB 10.1.23
### 一，下载sphinx,scws相关包

sphinx下载地址：http://sphinxsearch.com/downloads/release/

sphinx php扩展下载地址：http://pecl.php.net/package/sphinx

scws下载地址：http://www.xunsearch.com/scws/download.php

scws词库下载地址：http://www.xunsearch.com/scws/down/scws-dict-chs-utf8.tar.bz2

### 二，安装sphinx,scws,以及php扩展

- 1,安装sphinx
```
# curl http://sphinxsearch.com/files/sphinx-2.3.1-beta.tar.gz
# tar zxvf sphinx-2.3.1-beta.tar.gz  
# cd sphinx-2.3.1-beta  
# ./configure --prefix=/usr/local/sphinx --with-mysql=/usr/local/mariadb LIBS=-liconv
# make && make install  
```
- 2,安装sphinx客户端
```
# cd api/libsphinxclient   //sphinx-2.3.1-beta目录下  
# ./configure --prefix=/usr/local/sphinx/libsphinxclient  
# make && make install  
```
- 3,安装sphinx php扩展

```
# git clone https://github.com/php/pecl-search_engine-sphinx.git  
# cd pecl-search_engine-sphinx
# git checkout php7  
# phpize  
# ./configure  --with-sphinx=/usr/local/sphinx/libsphinxclient --with-php-config=/usr/local/php/bin/php-config  
# make && make install  
```

- 4,安装scws

```
# cd /usr/local/src  
# wget http://www.xunsearch.com/scws/down/scws-1.2.3.tar.bz2
# tar xvjf scws-1.2.3.tar.bz2  
# mkdir /usr/local/scws     
# cd scws-1.2.3     
# ./configure --prefix=/usr/local/scws/     
# make && make install   
```
- 5,安装scws php扩展

```
# cd /usr/local/src/sphinx/scws-1.2.3/phpext    
# phpize     
# ./configure --with-php-config=/usr/local/php/bin/php-config   
# make && make install   

```

- 6 安装scws词库

```
# wget http://www.xunsearch.com/scws/down/scws-dict-chs-utf8.tar.bz2
# tar xvjf scws-dict-chs-utf8.tar.bz2 -C /usr/local/scws/etc/  

# www为php-fpm运行用户  
# chown www:www /usr/local/scws/etc/dict.utf8.xdb  
```

在这里一定要加权限，也就是说让php-fpm或者php-cgi的运行用户，拥有dict.utf8.xdb的所有权限。如果不这么做的话，php 扩展调用词库会报如下错误：
```
Warning: SimpleCWS::add_dict(): Failed to add the dict file
```
怎么查看php-fpm,php-cgi的运行用户呢？
```
# ps aux |grep php-fpm  
[www@localhost src]$ ps aux |grep php-fpm
www       51250  0.0  0.0 103340   912 pts/0    S+   11:35   0:00 grep php-fpm
root     102701  0.0  0.0 256360  7064 ?        Ss   Jan23   0:07 php-fpm: master process (/usr/local/php/etc/php-fpm.conf)                                                                    
www      102702  0.0  0.0 259536 12332 ?        S    Jan23   0:00 php-fpm: pool     //使用php的用户 www                                                                                                            
www      102703  0.0  0.0 259316 11792 ?        S    Jan23   0:00 php-fpm: pool www                                                                                                            
www      102704  0.0  0.0 261576 14060 ?        S    Jan23   0:00 php-fpm: pool www                                                                                                            
www      102705  0.0  0.0 259304 11852 ?        S    Jan23   0:00 php-fpm: pool www                                                                                                            
www      102706  0.0  0.0 259128 12016 ?        S    Jan23   0:00 php-fpm: pool www                                                                                                            
www      102707  0.0  0.0 259400 12100 ?        S    Jan23   0:00 php-fpm: pool www                                                                                                            
www      102708  0.0  0.0 259296 12092 ?        S    Jan23   0:00 php-fpm: pool www                                                                                                            

```


### 三，配置sphinx,scws,php等

- 1,创建测试表和数据 test.sql

```
test.sql
/*
Navicat MySQL Data Transfer

Source Server         : 192.168.101.130
Source Server Version : 50527
Source Host           : 192.168.101.130:3306
Source Database       : test

Target Server Type    : MYSQL
Target Server Version : 50527
File Encoding         : 65001

Date: 2016-08-14 13:46:48
*/

SET FOREIGN_KEY_CHECKS=0;

-- ----------------------------
-- Table structure for `userinfo`
-- ----------------------------
DROP TABLE IF EXISTS `userinfo`;
CREATE TABLE `userinfo` (
  `id` int(11) unsigned NOT NULL AUTO_INCREMENT,
  `userid` int(11) unsigned NOT NULL DEFAULT '0',
  `addtime` datetime NOT NULL,
  `post` varchar(20) NOT NULL DEFAULT '',
  `summary` text NOT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=21 DEFAULT CHARSET=utf8;

-- ----------------------------
-- Records of userinfo
-- ----------------------------
INSERT INTO `userinfo` VALUES ('17', '1', '2012-06-01 00:24:54', '洪', '奥运');
INSERT INTO `userinfo` VALUES ('18', '2', '2014-08-19 10:24:54', '中国人', '洪荒');
INSERT INTO `userinfo` VALUES ('19', '3', '2015-08-01 12:24:54', '洪荒之', 'nimne');
INSERT INTO `userinfo` VALUES ('20', '4', '2013-08-01 00:24:54', '洪荒之力', '23金');

-- ----------------------------
-- Table structure for `users`
-- ----------------------------
DROP TABLE IF EXISTS `users`;
CREATE TABLE `users` (
  `id` int(11) unsigned NOT NULL AUTO_INCREMENT,
  `username` varchar(20) NOT NULL DEFAULT '',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=5 DEFAULT CHARSET=utf8;

-- ----------------------------
-- Records of users
-- ----------------------------
INSERT INTO `users` VALUES ('1', 'zhangsan');
INSERT INTO `users` VALUES ('2', 'zhangsi');
INSERT INTO `users` VALUES ('3', '李四');
INSERT INTO `users` VALUES ('4', '王五');
```

导入数据 `source test.sql`
{% qnimg 20160814140124005.jpg %}

数据源
```
select
	a.id,
	a.userid,
    b.username,
    UNIX_TIMESTAMP(a.addtime) AS addtime,
    a.post,
    a.summary

FROM
	userinfo a

LEFT JOIN users b ON a.userid = b.id
```

```
+----+--------+----------+------------+--------------+---------+
| id | userid | username | addtime    | post         | summary |
+----+--------+----------+------------+--------------+---------+
| 17 |      1 | zhangsan | 1338481494 | 洪           | 奥运    |
| 18 |      2 | zhangsi  | 1408415094 | 中国人       | 洪荒    |
| 19 |      3 | 李四     | 1438403094 | 洪荒之       | nimne   |
| 20 |      4 | 王五     | 1375287894 | 洪荒之力     | 23金    |
+----+--------+----------+------------+--------------+---------+

```


{% qnimg 20160814140002253.jpg %}

- 2,配置sphinx.conf，加上以下内容
其中searchd 以下3个配置比较重要。`mysql -h0 -P 9306`访问。9312可以给php访问。query_log_format 记录日志的格式。

- 1 # php
`listen          = 9312`

- 2 # mysql
 `listen          = 9306:mysql41`

- 3 # log_format

  `query_log_format = sphinxql`

- sphinx.conf


```
# vim /usr/local/sphinx/etc/sphinx-test.conf

source users
{
	type			= mysql
	sql_host		= 127.0.0.1
	sql_user		= root
	sql_pass		= 123456
	sql_db			= test
	sql_port		= 3306	# optional, default is 3306
	sql_query_pre = SET NAMES utf8
        sql_query_pre = SET SESSION query_cache_type=OFF  
        sql_query = 	SELECT a.id, a.userid,b.username, UNIX_TIMESTAMP(a.addtime) AS addtime, a.post, a.summary 	FROM userinfo a left join users b on a.userid = b.id  
	sql_attr_uint = userid  
	sql_field_string = username  
	sql_field_string = post  
	sql_attr_timestamp = addtime
	sql_ranged_throttle = 0  
	#sql_attr_uint		= group_id
	#sql_attr_timestamp	= date_added
	#sql_ranged_throttle	= 0
}
source src1throttled : users
{
	sql_ranged_throttle	= 100
}

index users
{  
	source = users
	path = /usr/local/sphinx/var/data/users
	docinfo = extern  
	mlock = 0  
	morphology = none  
	min_word_len = 1  
	html_strip = 1  
  charset_table = U+FF10..U+FF19->0..9, 0..9, U+FF41..U+FF5A->a..z, U+FF21..U+FF3A->a..z,A..Z->a..z, a..z, U+0149, U+017F, U+0138, U+00DF, U+00FF, U+00C0..U+00D6->U+00E0..U+00F6,U+00E0..U+00F6, U+00D8..U+00DE->U+00F8..U+00FE, U+00F8..U+00FE, U+0100->U+0101, U+0101,U+0102->U+0103, U+0103, U+0104->U+0105, U+0105, U+0106->U+0107, U+0107, U+0108->U+0109,U+0109, U+010A->U+010B, U+010B, U+010C->U+010D, U+010D, U+010E->U+010F, U+010F,U+0110->U+0111, U+0111, U+0112->U+0113, U+0113, U+0114->U+0115, U+0115, U+0116->U+0117,U+0117, U+0118->U+0119, U+0119, U+011A->U+011B, U+011B, U+011C->U+011D, U+011D,U+011E->U+011F, U+011F, U+0130->U+0131, U+0131, U+0132->U+0133, U+0133, U+0134->U+0135,U+0135, U+0136->U+0137, U+0137, U+0139->U+013A, U+013A, U+013B->U+013C, U+013C,U+013D->U+013E, U+013E, U+013F->U+0140, U+0140, U+0141->U+0142, U+0142, U+0143->U+0144,U+0144, U+0145->U+0146, U+0146, U+0147->U+0148, U+0148, U+014A->U+014B, U+014B,U+014C->U+014D, U+014D, U+014E->U+014F, U+014F, U+0150->U+0151, U+0151, U+0152->U+0153,U+0153, U+0154->U+0155, U+0155, U+0156->U+0157, U+0157, U+0158->U+0159, U+0159,U+015A->U+015B, U+015B, U+015C->U+015D, U+015D, U+015E->U+015F, U+015F, U+0160->U+0161,U+0161, U+0162->U+0163, U+0163, U+0164->U+0165, U+0165, U+0166->U+0167, U+0167,U+0168->U+0169, U+0169, U+016A->U+016B, U+016B, U+016C->U+016D, U+016D, U+016E->U+016F,U+016F, U+0170->U+0171, U+0171, U+0172->U+0173, U+0173, U+0174->U+0175, U+0175,U+0176->U+0177, U+0177, U+0178->U+00FF, U+00FF, U+0179->U+017A, U+017A, U+017B->U+017C,U+017C, U+017D->U+017E, U+017E, U+0410..U+042F->U+0430..U+044F, U+0430..U+044F,U+05D0..U+05EA, U+0531..U+0556->U+0561..U+0586, U+0561..U+0587, U+0621..U+063A, U+01B9,U+01BF, U+0640..U+064A, U+0660..U+0669, U+066E, U+066F, U+0671..U+06D3, U+06F0..U+06FF,U+0904..U+0939, U+0958..U+095F, U+0960..U+0963, U+0966..U+096F, U+097B..U+097F,U+0985..U+09B9, U+09CE, U+09DC..U+09E3, U+09E6..U+09EF, U+0A05..U+0A39, U+0A59..U+0A5E,U+0A66..U+0A6F, U+0A85..U+0AB9, U+0AE0..U+0AE3, U+0AE6..U+0AEF, U+0B05..U+0B39,U+0B5C..U+0B61, U+0B66..U+0B6F, U+0B71, U+0B85..U+0BB9, U+0BE6..U+0BF2, U+0C05..U+0C39,U+0C66..U+0C6F, U+0C85..U+0CB9, U+0CDE..U+0CE3, U+0CE6..U+0CEF, U+0D05..U+0D39, U+0D60,U+0D61, U+0D66..U+0D6F, U+0D85..U+0DC6, U+1900..U+1938, U+1946..U+194F, U+A800..U+A805,U+A807..U+A822, U+0386->U+03B1, U+03AC->U+03B1, U+0388->U+03B5, U+03AD->U+03B5,U+0389->U+03B7, U+03AE->U+03B7, U+038A->U+03B9, U+0390->U+03B9, U+03AA->U+03B9,U+03AF->U+03B9, U+03CA->U+03B9, U+038C->U+03BF, U+03CC->U+03BF, U+038E->U+03C5,U+03AB->U+03C5, U+03B0->U+03C5, U+03CB->U+03C5, U+03CD->U+03C5, U+038F->U+03C9,U+03CE->U+03C9, U+03C2->U+03C3, U+0391..U+03A1->U+03B1..U+03C1,U+03A3..U+03A9->U+03C3..U+03C9, U+03B1..U+03C1, U+03C3..U+03C9, U+0E01..U+0E2E,U+0E30..U+0E3A, U+0E40..U+0E45, U+0E47, U+0E50..U+0E59
	ngram_len = 1  
	ngram_chars = U+4E00..U+9FBF, U+3400..U+4DBF, U+20000..U+2A6DF, U+F900..U+FAFF,U+2F800..U+2FA1F, U+2E80..U+2EFF, U+2F00..U+2FDF, U+3100..U+312F, U+31A0..U+31BF,U+3040..U+309F, U+30A0..U+30FF,U+31F0..U+31FF, U+AC00..U+D7AF, U+1100..U+11FF,U+3130..U+318F, U+A000..U+A48F, U+A490..U+A4CF  

}
common
{
}
indexer
{
	mem_limit		= 128M
}
searchd
{

	#php
	listen          = 9312

	#mysql
    listen          = 9306:mysql41

	log			= /usr/local/sphinx/var/log/searchd.log
	query_log		= /usr/local/sphinx/var/log/query.log
	query_log_format = sphinxql
	read_timeout		= 5
	client_timeout		= 300
	max_children		= 30
	persistent_connections_limit	= 30
	pid_file		= /usr/local/sphinx/var/log/searchd.pid
	seamless_rotate		= 1
	preopen_indexes		= 1
	unlink_old		= 1
	mva_updates_pool	= 1M
	max_packet_size		= 8M
	max_filters		= 256
	max_filter_values	= 4096
	max_batch_queries	= 32
	workers			= threads # for RT to work
}
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
# /usr/local/sphinx/bin/indexer --config /usr/local/sphinx/etc/sphinx-test.conf --all  
# /usr/local/sphinx/bin/searchd --config /usr/local/sphinx/etc/sphinx-test.conf  
```
- 2,重启php-fpm
```
# /etc/init.d/php-fpm restart  
```

### 五，测试sphinx全文检索

- 1,命令行的测试

```
  # mysql -h127.0.0.1  -P9306
  MySQL [(none)]> show tables;
  +-------+-------+
  | Index | Type  |
  +-------+-------+
  | users | local |
  +-------+-------+
  1 row in set (0.00 sec)

  MySQL [(none)]>
  MySQL [(none)]>
  MySQL [(none)]> desc users;
  +----------+-----------+
  | Field    | Type      |
  +----------+-----------+
  | id       | bigint    |
  | username | field     |
  | post     | field     |
  | summary  | field     |
  | userid   | uint      |
  | username | string    |
  | addtime  | timestamp |
  | post     | string    |
  +----------+-----------+
  8 rows in set (0.00 sec)
```
{% qnimg 20160814142332937.jpg %}


```

MySQL [(none)]> select * FROM users WHERE match('洪荒之力');
+------+--------+----------+------------+--------------+
| id   | userid | username | addtime    | post         |
+------+--------+----------+------------+--------------+
|   20 |      4 | 王五     | 1375287894 | 洪荒之力     |
+------+--------+----------+------------+--------------+
```
{% qnimg 20160814142346640.jgp %}


- 2,利用php 扩展

```
<?php
 header("Content-type: text/html; charset=utf-8");
 $b_time = microtime(true);
 $key = "洪荒之力";
 $index = "users";
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
 echo '<p>输入：'.$key.'</p>'."\r\n";
 echo '<p>分词：'.$words.'</p>'."\r\n";
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
 exit;
?>

```
结果如下：

```
<p>输入：洪荒之力</p>
<p>分词：(洪荒)|(之)|(力)</p>
Array
(
    [error] =>
    [warning] =>
    [status] => 0
    [fields] => Array
        (
            [0] => username
            [1] => post
            [2] => summary
        )

    [attrs] => Array
        (
            [userid] => 1
            [username] => 7
            [addtime] => 2
            [post] => 7
        )

    [matches] => Array
        (
            [0] => Array
                (
                    [id] => 20
                    [weight] => 4500
                    [attrs] => Array
                        (
                            [userid] => 4
                            [username] => 王五
                            [addtime] => 1375345494
                            [post] => 洪荒之力
                        )

                )

            [1] => Array
                (
                    [id] => 19
                    [weight] => 3451
                    [attrs] => Array
                        (
                            [userid] => 3
                            [username] => 李四
                            [addtime] => 1438460694
                            [post] => 洪荒之
                        )

                )

            [2] => Array
                (
                    [id] => 18
                    [weight] => 2436
                    [attrs] => Array
                        (
                            [userid] => 2
                            [username] => zhangsi
                            [addtime] => 1408472694
                            [post] => 中国人
                        )

                )

        )

    [total] => 3
    [total_found] => 3
    [time] => 0
    [words] => Array
        (
            [洪] => Array
                (
                    [docs] => 4
                    [hits] => 4
                )

            [荒] => Array
                (
                    [docs] => 3
                    [hits] => 3
                )

            [之] => Array
                (
                    [docs] => 2
                    [hits] => 2
                )

            [力] => Array
                (
                    [docs] => 1
                    [hits] => 1
                )

        )

)

```

查询query.log
{% qnimg 20160814143027377.jpg %}



>参考：
http://blog.csdn.net/clevercode/article/details/52204124
http://www.cnblogs.com/chenpingzhao/p/4712345.html
http://www.ibm.com/developerworks/cn/opensource/os-sphinx/
http://ourmysql.com/archives/965
http://www.cnblogs.com/yjf512/p/3581869.html
http://blog.51yip.com/mysql/1658.html
