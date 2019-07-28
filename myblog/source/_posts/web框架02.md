---
title: web框架02
date: 2019-03-23 14:25:46
tags: 
    - web框架
    - 从零开始
---
1.Balsamiq Mockups
2.UIDesginer
3.RESTful
4.MySQL
5.Navicat
6.MVC 
<!--more-->

### 单元测试
```xml
<!-- JUnit -->
<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.11</version>
    <scope>test</scope>
</dependency>   

<!-- Apache Commons Lang -->
<dependency>
    <groupId>org.apache.commons</groupId>
    <artifactId>commons-lang3</artifactId>
    <version>3.3.2</version>
</dependency>   
<!-- Apache Commons Collections -->
<dependency>
    <groupId>org.apache.commons</groupId>
    <artifactId>commons-collections4</artifactId>
    <version>4.0</version>
</dependency>   
```
编写单元测试类
```java
@Test
Assert.assertEquals(...)
Assert.assertNotNull(...)
Assert.assertTrue(...)
```

资源文件读取类
```java
public static Properties loadProps(String fileName) {
    Properties props = null;
    InputStream is = null;
    try {
        is = Thread.currentThread().getContextClassLoader().getResourceAsStream(fileName);
        if (is == null) {
            throw new FileNotFoundException(fileName + " file is not found");
        }
        props = new Properties();
        props.load(is);
    } catch(IOException e) {
        LOGGER.error("load properties file failure", e);
    } finally {
        if (is != null) {
            try {
                is.close();
            } catch (IOException e) {
                LOGGER.error("close input stream failure", e);
            }
        }
    }
    return props;
}
```

字符转换工具类
```java
try {
    doubleValue = Double.parseDouble(strValue);
} catch (NumberFormatException e) {
    doubleValue = defaultValue;
}
```

读取常量
```java
private static final String USERNAME;
Properties conf = PropsUtil.loadProps("config.properties");
USERNAME = conf.getProperty("jdbc.username");

try {
    Class.forName(DRIVER);
} catch (ClassNotFoundException e) {
    LOGGER.error("can not load jdbc driver", e);
}
```

JDBC
```java
try {
    List<User> list = new ArrayList<>();
    String sql = "select * from t_user";
    Connection conn = DriverManager.getConnection(URL, USERNAME, PASSWORD);
    PreparedStatement stmt = conn.prepareStatement(sql);
    ResultSet rs = stmt.executeQuery();
    while (rs.next) {
        // to-do
    }
    return list;
} catch (SQLException e) {
    LOGGER.error("execute sql failure", e);
} finally {
    if (conn != null) {
        try {
            conn.close();
        } catch(SQLException e) {
            LOGGER.error("close connection failure", e);
        }
    }
}
```

