# Linux权限管理
## 对r w x权限的理解
### 对文件而言：
- r：可以查看文件内容（cat/more/head/tail/less）
- w：可以修改文件内容（vim）
- x：可以执行文件（脚本/命令）

### 对目录而言：
- r：可以列出目录中的内容（ls）
- w：可以在目录中创建，删除文件（touch/mkdir/rmdir/rm）
- x：可以进入目录（cd）

**谁可以改变文件的权限：** <br/>
- 文件的所有者
- 管理员root

**谁可以改变文件的所有者** <br/>
- 管理员root

## 权限管理
**只有文件的所有者或root用户才能更改文件权限。**
- chmod [ugoa+-=rwx] [文件名或目录] <br/>
ugoa: 所有者/所属组/其他人/所有用户 <br/>
+-=: 增加权限/减少权限/指定权限 <br/>
![](http://i.imgur.com/PqxHlS0.png)

- 用数字表示权限
r: 4, w: 2, x: 1 <br/>
![](http://i.imgur.com/dIBkIoJ.png)

- 递归修改（同时目录下所有文件或目录的权限）
chmod -R

- 改变文件的所有者/所属组
**只有管理员root才能执行成功。** <br/>
```
chown [用户] [文件或目录]
chgrp [用户组] [文件或目录]
```