---
title: Linux 刪除 Delete all root mail
id: 284
categories:
  - 未分类
date: 2016-01-21 10:16:47
tags:
---

有時候系統會提示收到郵件，通常是一些系統錯誤的通知，看完確認沒問題就可以刪了。

通知長得像這樣：
You have new mail in /var/spool/mail/root
刪除的方法很簡單，用以下兩種指令擇一即可。

cp /dev/null /var/spool/mail/root

> /var/spool/mail/root
>   這樣就會清空root的mail。