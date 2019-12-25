---
title: Java 进阶之链接数据库
categories:
- java
tags:
- java 进阶
- java
---

![](/images/java.jpg)

最常用到的 **Java** 之连接数据库，详细解析，保证你看了之候明明白白。

<!--more-->

# （超详细的） Java 进阶之连接数据库

**Java** 连接之前我们需要准备好连接数据库的 **jar** 包，里边封装了我们需要的连接数据库的工具和类，都给我们封装好了，连接数据库就 **so** **eary** 了。



## 连接数据库步骤

### 准备

**① 下载：**在网上下载连接数据库的 **jar** 包，或者到我的公众号：**一个不甘平凡的码农**，回复：“ **数据库jar** ” 就可以下载了。

**② 放到新建 libs 文件夹下：**下载完之后我们将 **jar** 包放到我们创建的 **java** 项目的根目录下新建文件夹 **libs** 下。

**③  加载 jar 包：**虽然我们把连接数据库的 jar 包放到我们项目了，但是我们还没有加载 jar 包，也就是说代码中无法调用，我们要做的是加载 jar 包。

在项目中点击 **jar** 包鼠标右键，选择  **Build Path → configure Build Path** 出现如下页面：

![](/images/加载jar.png)

**④** 选择上边的第三个标签 **Librarise**，选择 **Add External JARs...**  选择需要加载的 **jar**，就会出现在左边栏中：

**⑤** 然后选择第四个标签 **Order and Export** 在我们 **jar** 包前打钩然后保存就可以了。

![](/images/选择jar.png)

### 写代码连接数据库

新建一个j ava 类 DBOP.java。

```java
/**
* 功能：连接数据库
**/
public class DBOP {
    //加载驱动的变量
	String driver="com.mysql.jdbc.Driver";
    //url 包括数据库的 ip（这里用到的是本机就是localhost/127.0.0.1 ） 以及 3306端口，DBName 为你的数据库的名字
	String url="jdbc:mysql://localhost:3306/DBName?useSSL=true";
    //数据库的用户名
	String user="root";
    //数据库的密码
	String password="root";
  
    //功能：连接数据库并执行Sql语句之后关闭数据库
    //参数：HashMap<String,Object>>：map 类型变量，sql ：sql语句
	public ArrayList<HashMap<String,Object>> query(String sql) throws ClassNotFoundException, SQLException{
    //定义查询结果返回的数据在 ResultSet 集合
	ResultSet resultSet=null;
    //定义 connection 对象
	Connection connection=null;
    //定义执行 sql 语句的对象
	Statement statement=null;
    //创建一个List对象，里边的类型为HashMap数据类型
	ArrayList<HashMap<String,Object>>list=new ArrayList<>();
        //加载驱动
		Class.forName(driver);
        //接下来的连接数据库操作要在try...catch 语句中执行 
		try {
            //开始尝试连接数据库（传入url（包含ip、端口、数据库名等信息），数据库用户名和密码），连接结果返回一个 Connection 对象
			connection =(Connection) DriverManager.getConnection(url,user,password);
            //通过 connection 对象的 createStatement 方法创建一个能够执行 sql 语句的 Statement 对象
			statement=(Statement) connection.createStatement();
            //通过 statement 对象执行 sql 语句将执行结果返回一个 ResultSet 对象集合
			resultSet=statement.executeQuery(sql);
            //通过 ResultSet 对象可获取到 ResultSetMetaData 对象集合(目的是可以获取到表中的字段名)
			ResultSetMetaData resultSetMetaData=(ResultSetMetaData) resultSet.getMetaData();
		   // 遍历我们的查询结果集合 resultSet，我们将查询到的每条结果封装成 map（表字段名:对应的值）放到 List 中去。
            while (resultSet.next()) {
                //如果查询结果的 resultSet 集合中还有数据就执行 while 循环
                //创建一个 map 对象
				HashMap<String, Object>map = new HashMap<>();
                //通过for循环，将查询结果中的对应数据库表的字段名和查询值封装到map中
				for(int i=0;i<resultSetMetaData.getColumnCount();i++) {
                    //通过resultSetMetaData.getColumnName(i) 可获取到对应字段名
					map.put(resultSetMetaData.getColumnName(i), resultSet.getObject(i));
				}
                //将每个 map 对象添加到 list 当中
					list.add(map);			
			}
		} catch (Exception e) {
			// 连接数据库中途出错时就会执行 catch 语句块
				e.printStackTrace();
		}finally {
            //无论连接数据库出错还是成功，程序在最后总会执行 finally 语句（我们就在里边执行关闭数据库的操作）
            //关闭 resultSet 对象集合
			if(resultSet!=null){
				resultSet.close();
			}
            //关闭 statement 执行 sql 语句的对象
			if(statement!=null){
				statement.close();
			}
            //最后关闭数据库连接
			if(connection!=null){
				connection.close();
			}
		}
        //将结果集返回一个List对象，方便我们取出查询结果数据
		return list;
	}
}
```

尊重他人劳动成果，转载请说明出处：http://luxiangqiang.xn--6qq986b3xl/





















