---
title: web框架01
date: 2019-03-23 13:38:59
tags: 
    - web框架
    - 从零开始
---
1.java web框架
2.maven
3.Servlet3.0
4.IDEA、git
<!--more-->

前言：
> 

## web项目
工程目录
```
src
    |-main
        |-java
        |-resources
    |-test
        |-java
    |-webapp
        |-WEB-INF
            |-web.xml
pom.xml
```

### pom.xml
```xml
<modelVersion>1.0.0</modelVersion>

<groupId>org.lxin</groupId>
<artifactId>xin</artifactId>
<version>1.0.1</version>

<packaging>war</packaging>

<!-- uncoding -->
<properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
</properties>

<!-- plugin -->
<build>
    <plugins>
        <!-- JDK -->
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-compiler-plugin</artifactId>
            <version>3.3</version>
            <configuration>
                <source>1.8</source>
                <target>1.8</target>
            </configuration>
        </plugin>
        <!-- unit test -->
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-surefire-plugin</artifactId>
            <version>2.18.1</version>
            <configuration>
                <skipTests>true</skipTests>
            </configuration>
        </plugin>
    </plugins>
</build>
```
### Servelt3.0
```java
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServelt;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@WebServlet("/hello")
public class HelloXin extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) 
    throws ServeltException, IOException {
        DateFormat df = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
        String currentTime = df.format(new Date());
        req.setAttribute("currentTime", currentTime);
        req.getRequestDispatcher("/WEB-INF/jsp/xin.jsp").forward(req, resp);
    }
}
```

```jsp
<%@ page pageEncoding="UTF-8" %>
<html>
    <head>
        <title>hello,xin</title>
    </head>
    <body>
        <h1>Xin</h1>
        <h2>current Time: ${currentTime}</h2>
    </body>
</html>
```