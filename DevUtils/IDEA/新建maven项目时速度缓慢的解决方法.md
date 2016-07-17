##新建Maven项目时速度缓慢的解决方法
* 原因
IDEA根据maven archetype的本质，其实是执行mvn archetype:generate命令，该命令执行时，需要指定一个archetype-catalog.xml文件。
该命令的参数-DarchetypeCatalog，可选值为：remote，internal  ，local等，用来指定archetype-catalog.xml文件从哪里获取。
默认为remote，即从http://repo1.maven.org/maven2/archetype-catalog.xml路径下载archetype-catalog.xml文件。
http://repo1.maven.org/maven2/archetype-catalog.xml 文件约为3-4M，下载速度很慢，导致创建过程卡住。

* 解决方法
在下面界面添加一个属性 archetypeCatalog = internal
![](http://i.imgur.com/ljaZYJm.png)