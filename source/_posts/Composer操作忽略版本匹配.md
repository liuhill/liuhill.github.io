---
title: Composer操作忽略版本匹配
date: 2024-06-02 16:19:42
categories:
tags:
---
使用composer安装组件提示错误:
```
/www # composer require --prefer-dist powerkernel/yii2-photoswipe "*"
./composer.json has been updated
Running composer update powerkernel/yii2-photoswipe
Loading composer repositories with package information
Updating dependencies
Your requirements could not be resolved to an installable set of packages.

  Problem 1
    - symfony/browser-kit is locked to version v4.2.4 and an update of this package was not requested.
    - symfony/browser-kit v4.2.4 requires php ^7.1.3 -> your php version (8.2.19) does not satisfy that requirement.
  Problem 2
    - codeception/module-yii2 is locked to version 1.1.5 and an update of this package was not requested.
    - codeception/module-yii2 1.1.5 requires php >=5.6.0 <=8.1 | ~8.1.0 -> your php version (8.2.19) does not satisfy that requirement.
  Problem 3
    - symfony/console v4.2.12 requires php ^7.1.3 -> your php version (8.2.19) does not satisfy that requirement.
    - codeception/codeception 4.2.2 requires symfony/console >=2.7 <6.0 -> satisfiable by symfony/console[v4.2.12].
    - codeception/codeception is locked to version 4.2.2 and an update of this package was not requested.


Installation failed, reverting ./composer.json and ./composer.lock to their original content.
```

修复方法，添加参数`--ignore-platform-reqs`:

```
/www/www.heynet.com.cn # composer require --ignore-platform-reqs  --prefer-dist powerkernel/yii2-photoswipe "*"
./composer.json has been updated
Running composer update powerkernel/yii2-photoswipe
Loading composer repositories with package information
Updating dependencies
Lock file operations: 2 installs, 0 updates, 0 removals
  - Locking bower-asset/photoswipe (v4.1.3)
  - Locking powerkernel/yii2-photoswipe (1.1.5)
Writing lock file
Installing dependencies from lock file (including require-dev)
Package operations: 2 installs, 0 updates, 0 removals
  - Downloading bower-asset/photoswipe (v4.1.3)
  - Downloading powerkernel/yii2-photoswipe (1.1.5)
  - Installing bower-asset/photoswipe (v4.1.3): Extracting archive
  - Installing powerkernel/yii2-photoswipe (1.1.5): Extracting archive
Package overtrue/wechat is abandoned, you should avoid using it. Use w7corp/easywechat instead.
Package swiftmailer/swiftmailer is abandoned, you should avoid using it. Use symfony/mailer instead.
Package phpunit/php-token-stream is abandoned, you should avoid using it. No replacement was suggested.
Generating autoload files
65 packages you are using are looking for funding.
Use the `composer fund` command to find out more!
No security vulnerability advisories found.
```


其他命令参考：

```
composer install --ignore-platform-reqs
```
或者
```
composer update --ignore-platform-reqs
```
