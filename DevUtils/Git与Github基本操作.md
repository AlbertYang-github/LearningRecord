# Git与Github基本操作
### Git的基本命令
* 初始化新的本地仓库
```
git init
注意：初始化之后当前目录下会出现一个名为.git的目录，所有Git需要的数据和资源都存放在这个目录中
```
* 将文件添加到缓存（提交到本地仓库之前的操作）
```
//filename是需要添加的文件名
git add [filename]
//添加当前目录下的所有文件及子目录下的所有文件
git add .
注意：如果要提交文件已经在缓存中，但是修改过了，还是需要再次执行以上命令
```
* 将缓存中的文件提交到本地仓库
```
git commit -m "commit information"
git commit -am "commit information"
注意：如果文件已经被添加到缓存中，只是被修改过了，那么就可以直接使用第二条命令直接提交（相当于执行两条命令：git add [filename]和git commit -m "commit information"）
```

* 查看当前工作目录和缓存中文件的状态
```
git status
```