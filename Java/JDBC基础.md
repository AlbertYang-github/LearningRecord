# JDBC基础（操作MySQL）
## 连接数据库
- **导入JDBC包：**
在Java代码中，用import语句导入需要的包

- **注册JDBC驱动程序：**
这一步会导致JVM加载所需要的驱动程序到内存中执行

- **制定数据库URL：**
创建正确的地址指向你想要连接的数据库

- **创建连接对象：**
调用DriverManager对象的getConnection()方法来建立数据库连接

### 注册驱动的两种方式:
- 方式一
```
Class.forName("com.mysql.jdbc.Driver");
```
- 方式二
```
DriverManager.registerDriver(new Driver());
```

### 制定数据库URL
MySQL的URL格式：jdbc:mysql://hostname：port/databaseName （端口号默认为3306）

### 创建连接对象
```
DriverManager.getConnection();
```
> getConnection(String url)
> getConnection(String url, Properties info)
> getConnection(String url, String user, String password)

### 示例代码
```
//加载驱动
Class.forName("com.mysql.jdbc.Driver");
//连接数据库所需的信息
Properties info = new Properties();
String url = "jdbc:mysql://hostname：port/databaseName";
info.put("user", "user");
info.put("password", "password");
//创建连接对象
Connection conn = DriverManager.getConnection(url, info);
```

### 关闭连接
```
conn.close();
```

## Statement对象
一旦我们获得了数据库的连接，我们就可以和数据库进行交互。JDBC的Statement，CallableStatement和PreparedStatement接口定义的方法和属性，可以让你发送 SQL 命令到数据库，并从你的数据库接收数据。

- Statement
```
//SQL语句
String SQL = "···";
stmt = conn.createStatement();
//stmt.execute(SQL);
//stmt.executeUpdate(SQL);
//stmt.executeQuery(SQL);
```

- PreparedStatement (防止SQL注入)
```
String SQL = "Update Employees SET age = ? WHERE id = ?";
pstmt = conn.prepareStatement(SQL);
```
PreparedStatement中有setXXX()方法可以设置？处的值

- 关闭Statement对象