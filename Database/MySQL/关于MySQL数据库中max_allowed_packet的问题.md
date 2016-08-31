## 关于MySQL数据库中max_allowed_packet的问题

### max_allowed_packet是什么？
max_allowed_packet是指MySQL服务器端和客户端传送单个数据包允许的大小，这个数字至少应该大于客户程序将要处理的最大BLOB块的长度(网络数据传输是一个包一个包的传输)。

- 查看MySQL默认max_allowed_packet的大小
```
mysql> show variables like '%max_allowed_packet%';
+--------------------------+------------+
| Variable_name            | Value      |
+--------------------------+------------+
| max_allowed_packet       | 1048576    |
| slave_max_allowed_packet | 1073741824 |
+--------------------------+------------+
2 rows in set (0.00 sec)
```
从以上结果可以看出 max_allowed_packet = 1048576（字节），即：1M

### 如何配置max_allowed_packet？
- 在my.ini文件中添加 max_allowed_packet=4M
```
该文件在MySQL安装目录的根目录，这里以4M为例
```

- 重启MySQL服务（必须重启才能生效）

- 查看修改后的结果
```
mysql> show variables like '%max_allowed_packet%';
ERROR 2006 (HY000): MySQL server has gone away
No connection. Trying to reconnect...
Connection id:    1
Current database: *** NONE ***
+--------------------------+------------+
| Variable_name            | Value      |
+--------------------------+------------+
| max_allowed_packet       | 4194304    |
| slave_max_allowed_packet | 1073741824 |
+--------------------------+------------+
2 rows in set (1.01 sec)
```