---
title: Yii2 advanced add module
date: 2018-06-07 11:31:14
tags:
    - yii2
categories:
  - 技术
---
# Yii2 advanced add module
*在Yii2高级版添加新的模块*

### 0 下载Yii2框架，配置环境
https://github.com/yiisoft/yii2-app-advanced/blob/master/docs/guide/start-installation.md

### 1 先在项目的根目录下复制一份 backend 为 api
```
cp backend/ api -r
```

### 2 拷贝 api 环境
```
cp -a environments/dev/frontend environments/dev/api
cp -a environments/prod/frontend environments/prod/api
```

### 3 修改 environments/index.php 文件之后的代码（主要是添加了一些 api 相关的代码）
```
return [
    'Development' => [
        'path' => 'dev',
        'setWritable' => [
            'backend/runtime',
            'backend/web/assets',
            'frontend/runtime',
            'frontend/web/assets',
            'api/runtime',
            'api/web/assets',
        ],
        'setExecutable' => [
            'yii',
        ],
        'setCookieValidationKey' => [
            'backend/config/main-local.php',
            'frontend/config/main-local.php',
            'api/config/main-local.php',
        ],
    ],
    'Production' => [
        'path' => 'prod',
        'setWritable' => [
            'backend/runtime',
            'backend/web/assets',
            'frontend/runtime',
            'frontend/web/assets',
            'api/runtime',
            'api/web/assets',
        ],
        'setExecutable' => [
            'yii',
        ],
        'setCookieValidationKey' => [
            'backend/config/main-local.php',
            'frontend/config/main-local.php',
            'api/config/main-local.php',
        ],
    ],
];
```

### 4 然后再执行初始化命令
```
php init
```

### 5 然后记得去 `common/config/bootstrap.php` 最后一行添加如下代码：
```
Yii::setAlias('api', dirname(dirname(__DIR__)) . '/api');
```

### 6 修改一下配置文件 `api/config/main.php`
```
return [
    'id' => 'app-api',
    // ...
    'controllerNamespace' => 'api\controllers',
]
```

### 7 api 里面的控制器等有命名空间的文件

### 8
