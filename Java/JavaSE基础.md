# JavaSE基础
## 基本数据类型
### 占用存储空间的大小
> boolean 没有明确指定, 只能取true或false <br/>
> char 16 bits <br/>
> byte 8 bits <br/>
> short 16 bits <br/>
> int 32 bits <br/>
> long 64 bits <br/>
> float 32 bits <br/>
> double 64 bits <br/>

**整数默认类型：int** <br/>
**小数默认类型：double**

### 类型转换
* 将浮点数转换为整型时，截取小数点之间的值，并非四舍五入；可以使用java.lang.Math中的round()方法进行四舍五入

* 如果对char、byte或short做算数或位运算时，在运算之前都会自动转换为int，最终结果也是int类型

* 表达式中最大的数据类型决定表达式最终结果的数据类型