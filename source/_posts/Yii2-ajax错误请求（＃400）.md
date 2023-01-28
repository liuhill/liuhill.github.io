---
title: Yii2 ajax错误请求（＃400）
date: 2022-05-05 10:22:14
categories: 
    - 技术
tags:
    - yii2
    - 400
    - csrf
---

yii ajax post 请求出现400错误,如下：
> Bad Request (#400): Unable to verify your data submission.

或者
> 错误请求（＃400）：无法验证您的数据

整个页面所有ajax请求都出现错误，在ajax请求的header里面包含了_csrf信息,由于使用了`yii2-sortable-grid-view-widget`控件，所以无法通过ajax添加csrf来处理。

在网上总结了一下方法，将所有方法列出如下：

### 1、禁用csrf验证 
在controller里面添加：
```
public $enableCsrfValidation = false;
```
或者
```
public function init(){
     $this->enableCsrfValidation = false;
}
```

禁止某一个action使用crsf验证
```
 public function beforeAction($action) {
        $this->enableCsrfValidation = ($action->id !== "save-order"); // <-- here
        return parent::beforeAction($action);
    }
```

### 2、在ajax添加`_csrf`参数
```
var csrfToken = $('meta[name="csrf-token"]').attr("content");
$.ajax({
         url: 'request',
         type: 'post',
         dataType: 'json',
         data: {param1: param1, _csrf : csrfToken},
});
```

```
  $.ajax({
    url: '$urlSave',
    type: 'post',
    data: {payload: payload, _csrf: yii.getCsrfToken()},        
    dataType: 'json',
  }).success(function(response) {
      //处理
  });
```

### 3、在全局里面添加csrf数据
我采用这种方式，所有的ajax都会更新csrf，包括第三方控件。
```
<script>
    $.ajaxSetup({
        data: <?= yiihelpersJson::encode([
            yii::$app->request->csrfParam => yii::$app->request->csrfToken,
        ]) ?>
    });
</script>
```
