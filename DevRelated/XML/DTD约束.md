* 编写DTD约束文件（book.dtd）
例如：
```
<?xml version="1.0" encoding="UTF-8"?>
<!ELEMENT 书架 (书)>
<!ELEMENT 书 (书名,作者,售价,简介)>
<!ELEMENT 书名 (#PCDATA)>
<!ELEMENT 作者 (#PCDATA)>
<!ELEMENT 售价 (#PCDATA)>
<!ELEMENT 简介 (#PCDATA)>
```
注：(#PCDATA)代表简单元素（只包含可解析的字符数据）, 若括号内包含元素的名称，则该元素是复杂元素(包含子标签)；(书名,作者,售价,简介)代表可以包含这四个元素，(书名|作者|售价|简介)代表只能包含这四中的其中一个。

* 编写受约束的XML文件（book.xml）
例如：
```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE 书架 SYSTEM "book.dtd">
<书架>
	<书>
		<书名>Java</书名>
		<作者>杨欢</作者>
		<售价>100</售价>
		<简介>快速入门</简介>
	</书>
</书架>
```
注：<!DOCTYPE 书架 SYSTEM "book.dtd">代表引入book.dtd约束文件；"book.dtd"代表DTD文件与XML文件在相同的路径下，开发中应注意路径。
	 

