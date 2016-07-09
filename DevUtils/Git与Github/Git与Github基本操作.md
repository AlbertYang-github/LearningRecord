#Git与Github基本操作
###Git的基本命令
* 初始化新的本地仓库
```
git init
注：初始化之后当前目录下会出现一个名为.git的目录，所有Git需要的数据和资源都存放在这个目录中
```
* 将文件添加到缓存（提交到本地仓库之前的操作）
```
//filename是需要添加的文件名
git add [filename]
//添加当前目录下的所有文件及子目录下的所有文件
git add .
注：如果要提交文件已经在缓存中，但是修改过了，还是需要再次执行以上命令
```
* 取消已缓存的文件
```
git reset HEAD [filename]
```
* 将缓存中的文件提交到本地仓库
```
git commit -m "commit information"
git commit -am "commit information"
注：如果文件已经被添加到缓存中，只是被修改过了，那么就可以直接使用第二条命令直接提交
	（相当于执行两条命令：git add [filename]和git commit -m "commit information"）
```
* 修改上次提交
```
git commit --amend -m "commit information"
```
* 查看当前工作目录和缓存中文件的状态
```
git status
```
* 克隆远程仓库到本地
```
git clone [url] [name]
注：url是远程仓库的地址；name是克隆之后本地仓库的名字，如果不指定，和远程仓库的名字相同
```
* 查看已经添加的远程仓库
```
git remote -v
注：如果添加过，就会显示 [远程仓库的别名] [对应的url]
```
* 添加远程仓库
```
git remote add [another_name] [url]
```
* 修改已添加的远程仓库的别名
```
git remote rename [old] [new]
```
* 删除已添加的远程仓库
```
git remote remove [another_name]
```

* 从远程仓库抓取数据
```
git fetch [another_name]
注：此命令仅仅只是从远程仓库抓取本地仓库没有的数据，并没有merge功能
```
* 创建分支
```
git branch [branch_name]
```
* 切换分支
```
git checkout [branch_name]
```
* 删除分支
```
git branch -d [branch_name]
```
* 合并分支
```
git merge [branch_name]
git merge [remote_name/branch_name]
注：branch_name是需要被合并到主分支的分支名
	如果需要被合并的分支在远程仓库中则应该写为第二个命令(这种情况主要用于fetch之后的合并)
	用 (远程仓库名)/(分支名) 这样的形式表示远程分支
	在此之前需要切换回主分支(master)	
```
* 将本地分支推送到远程仓库
```
git push [remote_name] [branch_name]
注：git push [远程仓库名] [需要被推送的本地分支名]
```
**参考资料**：
> [Git官方文档](https://git-scm.com/docs)
> [Pro Git](http://iissnan.com/progit/)