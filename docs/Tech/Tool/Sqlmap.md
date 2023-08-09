---
title: Sqlmap使用笔记
---

# Sqlmap

记录一下sqlmap的（非常基础的）知识啥的

<!-- more -->

~~sqlmap从入门到入狱~~



## 一、sqlmap简介

> SQLMap 是一个开源的渗透测试工具，可以用来进行自动化检测，利用 SQL  注入漏洞，获取数据库服务器的权限。它具有功能强大的检测引擎，针对各种不同类型数据库的渗透测试的功能选项，包括获取数据库中存储的数据，访问操作系统文件甚至可以通过外带数据连接的方式执行操作系统命令。



sqlmap支持的五种不同的注入方式：

> Boolean-based blind：sqlmap 替换或附加到 HTTP 请求中的受影响参数，包含 SELECT 子语句的语法有效的 SQL 语句字符串，或用户要检索其输出的任何其他 SQL 语句。对于每个 HTTP 响应，通过在 HTTP 响应标头 / 正文与原始请求之间进行比较，该工具可以逐字符推断注入的语句的输出。或者，用户可以提供字符串或正则表达式以匹配 True 页面。在 sqlmap 中实现的用于执行此技术的二等分算法能够获取最多七个 HTTP 请求的输出每个字符。如果输出不在明文纯字符集内，则 sqlmap 将采用更大范围的算法来检测输出。
>
> Time-based blind：sqlmap 替换或附加到 HTTP 请求中的受影响参数，该语法有效的 SQL 语句字符串包含查询，该查询将使后端 DBMS 保留一定的秒数。对于每个 HTTP 响应，通过在 HTTP 响应时间和原始请求之间进行比较，该工具可以逐字符推断注入的语句的输出。像基于布尔的技术一样，应用二等分算法。
>
> Error-based：sqlmap 替换或附加到受影响的参数特定于数据库的错误消息引发语句，并解析 HTTP 响应标头和正文，以查找包含已注入的预定义字符链和其中的子查询语句输出的 DBMS 错误消息。仅当 Web 应用程序已配置为公开后端数据库管理系统错误消息时，此技术才有效。
>
> UNION query-based：sqlmap 将一个语法有效的 SQL 语句附加到受影响的参数，该语句以开头 UNION ALL SELECT。当 Web 应用程序页面 SELECT 在 for 循环或类似情况下直接传递语句的输出时，此技术有效，以便查询输出的每一行都打印在页面内容上。sqlmap 还能够利用部分（单项）UNION 查询 SQL 注入漏洞，该漏洞在语句的输出未在 for 构造中循环而仅显示查询输出的第一项时发生。联合查询分为内联（inner join）、左联（left outer join）、右联（right outer join）和全联（full outer join）
>
> Stacked queries：sqlmap 测试 Web 应用程序是否支持堆叠查询，然后在支持的情况下将 HTTP 请求后的分号（;）附加到受影响的参数上，后跟 SQL 语句以被执行。此技术对于运行除 SELECT（例如）数据定义或数据操作语句以外的 SQL 语句很有用，这可能导致文件系统的读写访问和操作系统命令的执行，具体取决于底层的后端数据库管理系统和会话用户特权。



软件可直接通过[官网](https://sqlmap.org/)下载安装

[github项目地址](https://github.com/sqlmapproject/sqlmap)



## 二、使用方式

### 指定目标

```shell
$ python3 sqlmap.py -u "url"	#指定目标网址 url的最后应当有参数 例如questionid=0
```

可能会问是否需要提高测试等级 比较推荐使用 3 等级进行测试。

也可以使用 --level 指定测试等级

### 列出库

```shell
$ python3 sqlmap.py -u "url" --dbs
```

### 列出表

```shell
$ python3 sqlmap.py -u "url" -D 库名 --tables
```

## 列出字段

```shell
$ python3 sqlmap.py -u "url" -D 库名 -T 表名 --columns
```

咕咕咕

## 参考资料

[SQLMap 从入门到入狱详细指南](https://gitbook.cn/books/5ba8393639ea516190a9b8f8/index.html)

[SqlMap中文版使用教程](https://www.wangan.com/docs/1060)

