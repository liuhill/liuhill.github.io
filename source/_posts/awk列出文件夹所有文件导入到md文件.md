---
title: awk列出文件夹所有文件导入到md文件
date: 2016-12-12 10:58:44
tags:
    - 命令行
    - awk
categories:
  - 技术
---
通过命令列出文件夹并输出到指定文件

    ll |grep json | awk '{print "## "NR"." ,$9}' > README.md

说明：
- `ll` 列出夹所有文件
- `grep json` 仅显示包含 **json** 的文件名
- awk '{print "## "NR"." ,$9}'
    - awk 格式化grep的结果
    - 打印输出
    - `## ` 每一天前面添加字符串`## `
    - NR"." 格式化输出行号。如 `1.`,`2.`等
    - `,` 连接字符串
    - `$9` 结果的第9列数据
    - `> README.md` 结果输出到README.md 文件

实例：

```
localhost:IFE8.0 json.json liuligang$ ll
total 944
-rw-r--r--@ 1 liuligang  staff    510 12 12 10:39 README.md
-rw-r--r--@ 1 liuligang  staff  21499 11 28 10:40 airshop.json
-rw-r--r--@ 1 liuligang  staff  36993 11 28 12:17 en_culture.json
-rw-r--r--@ 1 liuligang  staff  17351 11 28 12:18 en_gourmet.json
-rw-r--r--@ 1 liuligang  staff   6548 11 28 12:18 en_magazines.json
-rw-r--r--@ 1 liuligang  staff  25975 11 28 12:17 en_movies.json
-rw-r--r--@ 1 liuligang  staff  21805 11 28 12:16 en_recommend.json
-rw-r--r--@ 1 liuligang  staff  29110 11 28 12:18 en_travel.json
-rw-r--r--@ 1 liuligang  staff   1082 11 28 11:43 english_menu.json
-rw-r--r--@ 1 liuligang  staff  28534 11 28 11:48 ent_cars.json
-rw-r--r--@ 1 liuligang  staff  29919 11 28 11:46 ent_film.json
-rw-r--r--@ 1 liuligang  staff  27993 11 28 11:47 ent_finance.json
-rw-r--r--@ 1 liuligang  staff  26844 11 28 11:49 ent_travel.json
-rw-r--r--@ 1 liuligang  staff   5409 11 28 11:49 ent_video.json
-rw-r--r--@ 1 liuligang  staff   1331 11 28 11:48 entertainment_menu.json
-rw-r--r--  1 liuligang  staff    350 12 12 10:21 file.list
-rw-r--r--@ 1 liuligang  staff   7247 11 28 10:39 game.json
-rw-r--r--@ 1 liuligang  staff  36349 11 28 11:49 gossip.json
-rw-r--r--@ 1 liuligang  staff   8555 11 28 10:37 home.json
-rw-r--r--@ 1 liuligang  staff  31678 11 28 11:50 lifestyle.json
-rw-r--r--@ 1 liuligang  staff   1673 11 28 10:48 mainMenu.json
-rw-r--r--@ 1 liuligang  staff  43716 11 28 11:50 recommend.json
-rw-r--r--@ 1 liuligang  staff   8423 11 28 10:41 topic2.json
-rw-r--r--@ 1 liuligang  staff   7047 11 28 10:40 topic4.json

```

输出README.md文件如下
```
## 1. airshop.json
## 2. en_culture.json
## 3. en_gourmet.json
## 4. en_magazines.json
## 5. en_movies.json
## 6. en_recommend.json
## 7. en_travel.json
## 8. english_menu.json
## 9. ent_cars.json
## 10. ent_film.json
## 11. ent_finance.json
## 12. ent_travel.json
## 13. ent_video.json
## 14. entertainment_menu.json
## 15. game.json
## 16. gossip.json
## 17. home.json
## 18. lifestyle.json
## 19. mainMenu.json
## 20. recommend.json
## 21. topic2.json
## 22. topic4.json
```
