---
title: ffmpeg 多个音频合并 截取 拆分
date: 2018-01-29 13:27:09
tags: ffmpeg 合并 截取 拆分
---
### 多个mp3文件合并成一个mp3文件
一种方法是连接到一起
```
ffmpeg64.exe -i "concat:123.mp3|124.mp3" -acodec copy output.mp3
```
解释：
- -i代表输入参数
-  contact:123.mp3|124.mp3代表着需要连接到一起的音频文件
-  -acodec copy output.mp3 重新编码并复制到新文件中

### 另一种方法是混合到一起
```
ffmpeg64.exe -i 124.mp3 -i 123.mp3 -filter_complex amix=inputs=2:duration=first:dropout_transition=2 -f mp3 remix.mp3
```
解释：
- -i代表输入参数
- -filter_complex ffmpeg滤镜功能，非常强大，详细请查看文档
- amix是混合多个音频到单个音频输出
- inputs=2代表是2个音频文件，如果更多则代表对应数字
- duration 确定最终输出文件的长度
- longest(最长)|shortest（最短）|first（第一个文件）
- dropout_transition
The transition time, in seconds, for volume renormalization when an input stream ends. The default value is 2 seconds.
- -f mp3  输出文件格式

### 音频文件截取指定时间部分
```
ffmpeg64.exe -i 124.mp3 -vn -acodec copy -ss 00:00:00 -t 00:01:32 output.mp3
```
解释：
- -i代表输入参数
- -acodec copy output.mp3 重新编码并复制到新文件中
- -ss 开始截取的时间点
- -t 截取音频时间长度

音频文件格式转换
```
ffmpeg64.exe -i null.ape -ar 44100 -ac 2 -ab 16k -vol 50 -f mp3 null.mp3
```
解释：
- -i代表输入参数
- -acodec aac（音频编码用AAC）
- -ar 设置音频采样频率
- -ac  设置音频通道数
- -ab 设定声音比特率
- -vol <百分比> 设定音量
