---
title: json解析错误
id: 269
categories:
  - 未分类
date: 2016-01-05 21:18:06
tags:
---

         switch (json_last_error()) {
             case JSON_ERROR_NONE:
                 echo ' - No errors';
                 break;
             case JSON_ERROR_DEPTH:
                 echo ' - Maximum stack depth exceeded';
                 break;
             case JSON_ERROR_STATE_MISMATCH:
                 echo ' - Underflow or the modes mismatch';
                 break;
             case JSON_ERROR_CTRL_CHAR:
                 echo ' - Unexpected control character found';
                 break;
             case JSON_ERROR_SYNTAX:
                 echo ' - Syntax error, malformed JSON';
                 break;
             case JSON_ERROR_UTF8:
                 echo ' - Malformed UTF-8 characters, possibly incorrectly encoded';
                 break;
             default:
                 echo ' - Unknown error';
                 break;
         }


## 我碰到的错误

    echo mb_detect_encoding($passengers);
    $json = str_replace('&amp;quot;', '"', $passengers);
