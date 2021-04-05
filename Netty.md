# 目录

[TOC]

# 内容

## 1. 知乎-“Netty编解码、Netty粘包拆包、心跳机制”

https://zhuanlan.zhihu.com/p/265968511

## 2. TCP粘包拆包问题的四种解决方案

（1）在数据的末尾添加特殊的符号标识数据包的边界。通常会加\n、\r、\t或者其他的符号。

> 学习 HTTP、FTP 等，使用回车换行符号

（2）在数据的头部声明数据的长度，按长度获取数据。

> 将消息分为 head 和 body，head 中包含 body 长度的字段，一般 head 的第一个字段使用 int 值来表示 body 长度

（3）规定报文的长度，不足则补空位。读取时按规定好的长度来读取。比如 100 字节，如果不够就补空格。

（4）使用更复杂的应用层协议。

## 3. Netty是如何解决粘包问题的？

详见：https://zhuanlan.zhihu.com/p/265968511

（1）解决方式一---使用LineBasedFrameDecoder

见链接

（2）解决方式二---使用自定义长度帧解码器

见链接

（3）解决方式三---使用Google Protobuf编解码器

## 4. Netty心跳检测机制

详见：https://zhuanlan.zhihu.com/p/265968511