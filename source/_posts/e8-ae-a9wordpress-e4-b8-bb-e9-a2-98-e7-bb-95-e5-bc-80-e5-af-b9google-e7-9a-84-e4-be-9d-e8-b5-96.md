---
title: 让wordpress主题绕开对google的依赖
id: 10
categories:
  - 技术
date: 2014-08-21 14:37:28
tags:
  - wordpress
  - Google依赖
---

终于有个借着写技术博客顺便吐槽GFW的机会了。每当网速很快，却偏偏不能谷歌搜索，好不容易搜到个跟问题十分匹配的网站，结果点进去菊花转了几百圈页面还是加载不出来，这显然是件让人很蛋疼的事情。虽然我学会了安装和配置翻墙神器goagent，但我仍然为很多很多很多不会翻墙和不愿意或者没钱买vpn的人叫一下委屈，其实会弄goagent这种的除了少数的一些个搞技术的一般人也弄不来。还有，即使是弄goagent，一，它也是很费时的，配置麻烦，偶尔还要更新程序，二，最近装新系统，准备下载goagent和浏览器代理插件的时候发现，goagent的程序托管在google仓库，proxyswitch要去google应用商店下，但问题是尼玛要访问google的东西你得先翻墙啊~为了能翻墙得先翻墙才行，悖论啊~ 呃。。好了，吐槽结束。该说正事儿了。对了，不知道GFW的去谷歌吧，哦不对，还是去百度吧，谷歌怕你上不去呢。 事情是这样的，基于wordpress的网站里面的主题会用到google的资源，比如我的[blog站](http://sudodev.cn/)用的twentytwelve主题就引用了谷歌的字体，里面就有个这样的链接:[http://fonts.googleapis.com/css?family=Open+Sans:400italic,700italic,400,700&amp;subset=latin,latin-ext](http://fonts.googleapis.com/css?family=Open+Sans:400italic,700italic,400,700&subset=latin,latin-ext) 由于身在天朝的原因，你在访问我的blog的时候就会由于浏览器请求不到这个资源而一直在转菊花（loading加载），运气好只能显示部分图片信息，字看不见，而且是永远看不见那种，除非你像我一样翻墙了。好，既然是这样，那我们把它要请求的东西都下载到服务器上，把链接换成内部链接就好了。here we go:

<div class="md-section-divider">
</div>

## 1.离线资源 

*   把这个网站对应的css下载下来，给它取个名字，比如我叫他googlefonts.css。
*   打开那个css文件，你会发现里面还有一些url是指向google的，分别都下载下来放在一起。比如我把css文件和里面的四个woff的url对应的字体文件都下下来放到了一个叫fonts的文件夹。
*   修改那个googlefonts.css文件中指向google的url。由于你把离线下来的那些woff文件放在该css同一个目录，所以你把/前面的所有内容都去掉，留个文件名即可
*   把fonts文件夹放在对应主题的目录里。比如我放在wp-content/twentytwelve/css/下面

<div class="md-section-divider">
</div>

## 2.修改引用url ,原来主题上的url是指向google的，现在要将它改成指向内部的css文件，即前面的googlefonts.css文件。那么去哪里改呢？我是在主题目录里执行

`grep googleapis . -r`找到的，是在wp-content/twentytwelve/functions.php文件里。在

<div class="md-section-divider">
</div>

1.  `<span class="pln" style="color: #000000;">$font_url </span><span class="pun" style="color: #666600;">=</span><span class="pln" style="color: #000000;"> add_query_arg</span><span class="pun" style="color: #666600;">(</span><span class="pln" style="color: #000000;"> $query_args</span><span class="pun" style="color: #666600;">,</span><span class="str" style="color: #008800;">"$protocol://fonts.googleapis.com/css"</span><span class="pun" style="color: #666600;">);</span>`

后面加两句

<div class="md-section-divider">
</div>

1.  `<span class="pln" style="color: #000000;">$filePath </span><span class="pun" style="color: #666600;">=</span><span class="pln" style="color: #000000;"> dirname</span><span class="pun" style="color: #666600;">(</span><span class="pln" style="color: #000000;">__FILE__</span><span class="pun" style="color: #666600;">).</span><span class="str" style="color: #008800;">"/css/fonts/googlefonts.css"</span><span class="pun" style="color: #666600;">;</span>`
2.  `<span class="pln" style="color: #000000;">$font_url </span><span class="pun" style="color: #666600;">=</span><span class="str" style="color: #008800;">"/"</span><span class="pun" style="color: #666600;">.</span><span class="pln" style="color: #000000;">substr</span><span class="pun" style="color: #666600;">(</span><span class="pln" style="color: #000000;">$filePath</span><span class="pun" style="color: #666600;">,</span><span class="pln" style="color: #000000;"> strpos</span><span class="pun" style="color: #666600;">(</span><span class="pln" style="color: #000000;">$filePath</span><span class="pun" style="color: #666600;">,</span><span class="str" style="color: #008800;">"wp-content"</span><span class="pun" style="color: #666600;">));</span>`

好了，网站已经彻底摆脱了对google的依赖，马麻再也不用担心访问我的网站只能看到转菊花了~ ：）

> 你可能会眨巴着你一双无辜的水汪汪的大眼睛，问：你连谷歌都不上去又怎么离线它的css资源呢，这不也是悖论吗？我微风中头一甩，待额头上整齐的头发潇洒平安的降落在头的一侧的时候，答：当然是先把墙给翻了再做这件事喽，傻瓜~

<div class="md-section-divider">
</div>

## 另：网站中添加的谷歌analytics的js代码不会对网页加载产生影响。因为它是动态写上去，等网站发起对ga.js的请求的时候，网页中该请求的东西都请求完了。所以不会影响，只是有墙的情况下，由于ga.js请求不到，会影响google analytics对你网站流量监控的准确性。     <转载于：http://sudodev.cn/articles/354.html>
