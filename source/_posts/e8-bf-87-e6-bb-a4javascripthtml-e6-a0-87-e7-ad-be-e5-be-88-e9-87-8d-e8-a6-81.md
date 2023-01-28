---
title: '过滤javascript,html标签很重要'
id: 8
categories:
  - 技术
date: 2014-08-21 14:27:44
tags:
  - javascript
  - 过滤
---

<div class="title" style="color: #000000;">

# 
    过滤javascript,html标签很重要

</div>

<div class="meta" style="font-style: italic; color: #000000;">
  张映 发表于 2010-09-26 分类目录： [php](http://blog.51yip.com/category/php "查看 php 的全部文章")
</div>

<div class="entry" style="color: #000000;">
  用户输入的东西是不可信认的，例如，用户注册，用户评论等，这样的数据，你不光要做好防sql的注入，还要防止JS的注入,html的注入。   **一，javascript注入的危害** 举个简单的例子，我在一个网站留言了，并且这个网站没有对JS进行过滤,我在留言中加入以下内容 <div class="dp-highlighter">
    <div class="bar">
      <div class="tools" style="color: silver;">
        [查看](http://blog.51yip.com/php/1031.html#)[复制](http://blog.51yip.com/php/1031.html#)[打印](http://blog.51yip.com/php/1031.html#)[?](http://blog.51yip.com/php/1031.html#)
      </div>
    </div>

1.  <span style="color: black;"><script language=javascript>  </span>
2.  <span style="color: black;"> <span class="keyword" style="font-weight: bold; color: #006699;">for</span>(i=0;i>=0;i++){  </span>
3.  <span style="color: black;"> alert(<span class="string" style="color: blue;">'我弹'</span>);  </span>
4.  <span style="color: black;"> }  </span>
5.  <span style="color: black;"> </script>  </span>
  </div> 上面的代码虽然简单，可是可以无限循环，并且会一直弹东西出来，让人感觉很不爽，直到浏览器没有响应为止。浏览您网站的人，第一反应肯定是这个网站有病毒。 解决办法 

  <div class="dp-highlighter">
    <div class="bar">
      <div class="tools" style="color: silver;">
        [查看](http://blog.51yip.com/php/1031.html#)[复制](http://blog.51yip.com/php/1031.html#)[打印](http://blog.51yip.com/php/1031.html#)[?](http://blog.51yip.com/php/1031.html#)
      </div>
    </div>

1.  <span style="color: black;"><span class="vars">$comment</span> = preg_replace(<span class="string" style="color: blue;">"/<[^><]*script[^><]*>/i"</span>,<span class="string" style="color: blue;">''</span>,<span class="vars">$comment</span>);  </span>
  </div> 把里面的javascript标签去掉就行了。 

  **二，html注入的危害** 1，容易引起页面错乱，对用户输入html标签不做处理的话，在读取的时候，很有可能就会破坏页面的布局。 2，影响seo，做seo的人都知道，pr高的网址，如果有链接，链到你的网站的话，可以加大自己网站的权重，这也是为什么有那么多人喜欢在高pr网站灌水的原因了。如果你没有对html标签进行处理的话，我输入以下内容 <div class="dp-highlighter">
    <div class="bar">
      <div class="tools" style="color: silver;">
        [查看](http://blog.51yip.com/php/1031.html#)[复制](http://blog.51yip.com/php/1031.html#)[打印](http://blog.51yip.com/php/1031.html#)[?](http://blog.51yip.com/php/1031.html#)
      </div>
    </div>

1.  <span style="color: black;"><span class="tag" style="font-weight: bold; color: #006699;"><</span><span class="tag-name" style="font-weight: bold; color: #006699;">a</span> <span class="attribute" style="color: red;">href</span>=<span class="attribute-value" style="color: blue;">"http://XXX.com"</span> <span class="attribute" style="color: red;">style</span>=<span class="attribute-value" style="color: blue;">"display:none;"</span><span class="tag" style="font-weight: bold; color: #006699;">></span>XXX.COM<span class="tag" style="font-weight: bold; color: #006699;"></</span><span class="tag-name" style="font-weight: bold; color: #006699;">a</span><span class="tag" style="font-weight: bold; color: #006699;">></span>  </span>
  </div> XXX.COM是个不河蟹网站,政府肯定会河蟹的,如果你的网站有链接到这样的网址,很有可能导致,你网站权重降低. 危害肯定不止这二个,因此要对这些html标签进行处理 

  <div class="dp-highlighter">
    <div class="bar">
      <div class="tools" style="color: silver;">
        [查看](http://blog.51yip.com/php/1031.html#)[复制](http://blog.51yip.com/php/1031.html#)[打印](http://blog.51yip.com/php/1031.html#)[?](http://blog.51yip.com/php/1031.html#)
      </div>
    </div>

1.  <span style="color: black;"><span class="vars">$comment</span> = preg_replace(<span class="string" style="color: blue;">"/<[\/\!]*?[^<>]*?>/si"</span>,<span class="string" style="color: blue;">''</span>,<span class="vars">$comment</span>);  </span>
  </div> 过滤的方法有好多，也可以直接把<>这样的符号转义掉，或者直接删除掉都是可以的。   

  **转载请注明 作者:海底苍鹰 地址:[http://blog.51yip.com/php/1031.html](http://blog.51yip.com/php/1031.html)**
</div>