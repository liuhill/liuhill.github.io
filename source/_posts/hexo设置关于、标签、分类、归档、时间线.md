---
title: hexo设置关于、标签、分类、归档、时间线
date: 2022-04-14 15:43:08
categories: 
    - 技术
tags:
    - hexo
    - 配置
---
### 1、添加 关于页面
使用：`hexo new page "about"` 新建一个 `关于我` 页面。
主题的 `_config.yml` 文件中的 `menu` 中进行匹配
不同主题 `_config.yml`文件有区别
```
menu:
  home: /      //主页
  categories: /categories //分类
  archives: /archives   //归档
  tags: /tags   //标签
  about: /about   //关于                  （添加此行即可）
```
或 
```   
menu:
  - page: home
    directory: .      //主页
    icon: fa-home
  - page: archive
    directory: archives/    //归档
    icon: fa-archive
  - page: about
    directory: about/    //关于
    icon: fa-user
  - page: rss
    directory: atom.xml    //rss订阅
    icon: fa-rss
```
编辑 about 关于页面 md文件 部署就能看到

### 2、添加 标签页面
使用： `hexo new page tags` 新建一个 标签 页面。
主题的 `_config.yml` 文件中的 `menu` 中进行匹配

```
menu:
  home: /      //主页
  categories: /categories //分类
  archives: /archives   //归档
  tags: /tags   //标签                  （添加此行即可）
  about: /about   //关于
```
底下代码是一篇包含 标签 文章的例子：

```
title: 标签测试
tags:
  - Testing                   （这个就是文章的标签了）
  - Another Tag               （这个就是文章的标签了）
---
```
### 3、添加 分类页面
使用： `hexo new page categories` 新建一个 分类 页面。
主题的 `_config.yml` 文件中的 `menu` 中进行匹配

```
menu:
  home: /      //主页
  categories: /categories //分类        （添加此行即可）
  archives: /archives   //归档
  tags: /tags   //标签                  
  about: /about   //关于
```
底下代码是一篇包含 分类 文章的例子：

```
title: 分类测试
categories:
- hexo                       （这个就是文章的分类了）
---
```
### 4、添加 归档页面
使用： `hexo new page archives` 新建一个 归档 页面。

主题的 _config.yml 文件中的 menu 中进行匹配

```
menu:
  home: /      //主页
  categories: /categories //分类        
  archives: /archives   //归档             （添加此行即可）
  tags: /tags   //标签                  
  about: /about   //关于
```

### 5、添加 时间线页面
使用： `hexo new page timeline` 新建一个 归档 页面。

主题的 `_config.yml` 文件中的 `menu` 中进行匹配

```
menu:
  home: /      //主页
  categories: /categories //分类        
  archives: /archives   //归档             （添加此行即可）
  tags: /tags   //标签    
-page: history   (时间线)
     directory: timeline/
      icon: fa-history              
  about: /about   //关于
```

修改timeline对应的列表
```
timeline:
  - num: 1
    word: 2018/08/16-Start 
  - num: 2
    word: writing
  - num: 3
    word: More
```
### 6、添加 自定义页面
使用： `hexo new page "guestbook"` 新建一个 自定义 页面。
主题的 `_config.yml` 文件中的 `menu` 中进行匹配

```
menu:
  home: /      //主页
  categories: /categories //分类        
  archives: /archives   //归档   
  tags: /tags   //标签                  
  about: /about   //关于
  guestbook: /guestbook    //自定义             （添加此行即可）
```

> 作者：hiekay
链接：https://www.jianshu.com/p/ebbbc8edcc24
来源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
