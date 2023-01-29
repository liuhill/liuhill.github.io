---
title: 'php try/catch a warning '
date: 2017-01-06 13:11:14
tags:
    - php
    - 调试
categories:
  - 技术
---
## 方案一，输出到系统错误

One possibility is to set your own error handler before the call and restore the previous error handler later with restore_error_handler().
```
set_error_handler(function() { /* ignore errors */ });
dns_get_record();
restore_error_handler();
```
You could build on this idea and write a re-usable error handler that logs the errors for you.

```
set_error_handler([$logger, 'onSilencedError']);
dns_get_record();
restore_error_handler();
Turning errors into exceptions
```
You can use `set_error_handler()` and the `ErrorException` class to turn all php errors into exceptions.

```
set_error_handler(function($errno, $errstr, $errfile, $errline, array $errcontext) {
    // error was suppressed with the @-operator
    if (0 === error_reporting()) {
        return false;
    }

    throw new ErrorException($errstr, 0, $errno, $errfile, $errline);
});

try {
    dns_get_record();
} catch (ErrorException $e) {
    // ...
}
```

The important thing to note when using your own error handler is that it will bypass the `error_reporting` setting and pass all errors (notices, warnings, etc.) to your error handler. You can set a second argument on `set_error_handler()` to define which error types you want to receive, or access the current setting using ... = `error_reporting()` inside the error handler.

Suppressing the warning

Another possibility is to suppress the call with the @ operator and check the return value of dns_get_record() afterwards. But I'd advise against this as errors/warnings are triggered to be handled, not to be suppressed.


## 方案二： 输出到自定义函数
```
set_error_handler(function ($errno, $errstr) {
    // do something
    echo "\r\n～～～～～找到错误 :-D～～～～～\r\n";
    echo "错误信息:($errno)$errstr \r\n";
    debug_print_backtrace(DEBUG_BACKTRACE_IGNORE_ARGS);
    echo "\r\n";
}, E_WARNING);
$dir_handle = opendir("$dir/111");  //要catch的函数
restore_error_handler();
```
