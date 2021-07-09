---
title: 实现网页页面跳转的几种方法(meta标签、js实现、php实现)
date: 2018-01-25 13:35:37
tags:
---
### 1、meta标签实现 只需在head里加上下面这一句就行了，在当前页面停留0.1秒后跳转到目标页面
```
<meta http-equiv="refresh" content="0.1; url=http://baidu.cn/">
```
### 2、Javascript实现
- 方法一：这个方法比较常用
```
window.location.href = "http://baidu.cn/";
```

- 方法二：
```
self.location = "http://baidu.cn/";
```

- 方法三：
```
top.location = "http://baidu.cn/";
```
- 方法四：
只对IE系列浏览器有效，实用性不大
```
window.navigate("http://baidu.cn/");
```

### 3、php实现
```
<?php
    header("Location: http://baidu.cn/");
?>
```
