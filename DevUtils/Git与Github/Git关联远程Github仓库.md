 ##Git关联远程Github仓库
####1.配置Git,设置用户信息
```
git config --global user.name "jack"
git config --global user.email "your_email@example.com"
注：git config命令的--global参数，用了这个参数，表示你这台机器上所有的
   Git仓库都会使用这个配置，当然也可以对某个仓库指定不同的用户名和Email地址
   配置了用户信息再提交代码后其他人可以知道是谁提交的
   git config -l 查看用户配置信息
```
####2.在本机创建SSH Key
```
ssh-keygen -t rsa -C "your_email@example.com"
注：在gitbash下执行此命令后会在C:\Users\username\路径下会生成一个.ssh的目录，
   里面有id_rsa和id_rsa.pub两个文件
```
####3.将本机生成的密钥添加到Github
```
在Github用户设置中的SSH and GPG key中新建一个SSH Key，Title最好填一个能标识
你这台电脑的标识符(万一你有多台电脑需要向这个Github仓库上push呢)，在Key文本框里
粘贴id_rsa.pub文件的内容(注意：注意，拷贝时切莫在其中插入多余的换行符、空格等，
否则在公钥认证过程因为服务器端和客户端公钥不匹配而导致认证失败)
```
####4.验证是否配置成功
```
ssh -T git@github.com
当执行以上命令后会出现
The authenticity of host 'github.com (207.97.227.239)' can't be established.
RSA key fingerprint is 16:27:ac:a5:76:28:2d:36:63:1b:56:4d:eb:df:a6:48.
Are you sure you want to continue connecting (yes/no)?
直接输入yes然后回车来连接服务器
```

