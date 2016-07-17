##Maven基础内容
###安装
* 首先在官网下载Maven文件（只需要下载Binary文件即可）
下载地址：[http://maven.apache.org/download.cgi](http://maven.apache.org/download.cgi)
* 配置环境变量（Windows）
MAVEN_HOME = 解压之后的路径（如：D:\BuildTools\apache-maven-3.3.9）
path = %MAVEN_HOME%\bin
* 验证是否成功
在命令行输入: mvn -v

###POM
POM 代表工程对象模型（Project Object Model），它是使用 Maven 工作时的基本组建，是一个 xml 文件。它被放在工程根目录下，文件命名为 pom.xml，每个工程应该只有一个POM文件。

POM文件的根元素为project，其中包含三个必须的元素:groupId,artifactId,version
modelVersion: POM模型的版本
groupId: 这个项目所属的组织，通常都是组织的域名的反写形式
artifactId: 这个项目产生的制品的名称，比如生成的JAR包或者WAR包的名称
version: 项目的版本
packaging: 项目如何打包，默认值为jar

###构建生命周期
<table>
<tr>
<th>阶段</th>
<th>处理</th>
<th>描述</th>
</tr>
<tbody>
<tr>
<td>prepare-resources</td>
<td>资源拷贝</td>
<td>本阶段可以自定义需要拷贝的资源</td>
</tr>
<tr>
<td>compile</td>
<td>编译</td>
<td>本阶段完成源代码编译</td>
</tr>
<tr>
<td>package</td>
<td>打包</td>
<td>本阶段根据 pom.xml 中描述的打包配置创建 JAR / WAR 包</td>
</tr>
<tr>
<td>install</td>
<td>安装</td>
<td>本阶段在本地 / 远程仓库中安装工程包</td>
</tr>
</tbody>
</table>