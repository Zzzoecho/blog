# JDBC

## 概念

> JDBC (Java Database Connectivity) , SUN 公司为了统一对数据库的操作, 定义的一套规范

- JDBC API: 一组标准的接口和类.
> 驱动: 接口的实现, 每个数据库厂商都需要提供自己的驱动

### 使用 jdbc 连接数据库

```java
public class MySQLDemo {
    // 指定驱动名和数据库链接
    static final String JDBC_DRIVER = "com.mysql.cj.jdbc.Driver";
    static final String DB_URL = "jdbc:mysql://10.1.4.11:3306/carl";

    // 指定登录用户和密码
    static final String USER = "zcw_db_user";
    static final String PASS = "PZ4tEcNVrLhcPxUt";

    public static void main(String[] args) {
        Connection conn = null;
        Statement stmt = null;
        try{
            // 1 注册 JDBC 驱动
            Class.forName(JDBC_DRIVER);

            // 2 建立链接
            conn = DriverManager.getConnection(DB_URL,USER,PASS);

            // 3 执行查询
            System.out.println("实例化Statement对象...");
            stmt = conn.createStatement();
            String sql;
            sql = "SELECT id, name, age FROM users";
            ResultSet rs = stmt.executeQuery(sql);

            // 如果有数据，rs.next()返回true
            while(rs.next()){
                System.out.println(rs.getString("name")+" 年龄："+rs.getInt("age"));
            }

            // 完成后关闭
            rs.close();
            stmt.close();
            conn.close();
        }catch(SQLException se){
            // 处理 JDBC 错误
            se.printStackTrace();
        }catch(Exception e){
            // 处理 Class.forName 错误
            e.printStackTrace();
        }finally{
            // 关闭资源
            try{
                if(stmt!=null) stmt.close();
            }catch(SQLException se2){
            }// 什么都不做
            try{
                if(conn!=null) conn.close();
            }catch(SQLException se){
                se.printStackTrace();
            }
        }
        System.out.println("Goodbye!");
    }
}
```

1. 指定驱动和数据库链接
2. 指定登录用户名和密码
3. 注册驱动
4. 打开数据库链接
5. 执行语句