---
title: IE提交ajax提交304
id: 157
categories:
  - 技术文章
date: 2014-12-30 11:32:19
tags:
---

增加cache

    $.ajax({  
        cache : false,  
        url : "sendCode.html",  
        data : {phone : value}  
    });  
    