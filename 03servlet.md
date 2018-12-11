# Servlet

## 创建servlet

* 在web项目里面创建 创建包  在里面在创建 servlet

* 创建类继承   httpServlet

* 配置  webroot - webinf - web.xml

## web.xml

tomcat的 web app配置文件

* 2016

servlet
servlet-maping

* 2017

servlet name 配置的是名称
servlet <-> url  将java类转化成 网页

tomcat 可以将 java 变成  网页

[localshot:8080/Myweb/servlet/addServlet](localshot:8080/Myweb/servlet/addServlet)

### servlet doGet、doPost

获取客户端请求的回调函数

#### doGet

request.getParameter()  
收到的参数 都是字符串，没传的参数都是 null

//返回到前端
out.write()  
out.flush()
out.close()

## jsp

jsp页面可以编写  java html css js的综合页面

<%@ page language="java" import="java.util.*" pageEncoding="utf-8"%>

<%
    String path = request.getContextPath();
    String basePath = request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+path+"/";
%>

"<%=basePath%>"  记录的是  webRoot 绝对路径

后端渲染  

java中   /  代表根目录

## 后端渲染流程

1. jsp页面post请求

    ```jsp
    <%@ page language="java" import="java.util.*" pageEncoding="utf-8"%>
    <%
    String path = request.getContextPath();
    String basePath = request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+path+"/";
    //basePath 记录了WebRoot的绝对路径
    %>
    <!--以html为模板 加上第一个 <%@%> 代码段 jsp格式的文件就是jsp  添加 <%%> 可以 编写 java代码   下面就按照html进行编写-->
    <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
    <html>
        <head>
            <meta charset="utf-8">
            <title>post提交用户名密码</title>
        </head>
        <body>
            <form action="<%=basePath%>servlet/LogIn" method="POST">
            <!--这里   <%=basePath%>  通过java获取了webRoot的绝对路径-->
                <input type="text" name="userName">
                <input type="text" name="passWord">
                <input type="submit" value="提交">
            </form>
        </body>
    </html>
    ```

2. servlet程序接收参数、跳转

    ```java
    package com.weibin.servlet;
    import java.io.IOException;
    import java.io.PrintWriter;
    import javax.servlet.ServletException;
    import javax.servlet.http.HttpServlet;
    import javax.servlet.http.HttpServletRequest;
    import javax.servlet.http.HttpServletResponse;
    public class LogIn extends HttpServlet {
        public void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        }
        public void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
            doGet(request,response);
            String userName = request.getParameter("userName");
            //request.getParameter() 获取前端传递的参数
            String passWord = request.getParameter("passWord");
            //操作一下
            String result = "账号：" + userName + " 密码：" + passWord;
            //传值
            request.setAttribute("result", result);
            //传对象
            request.getRequestDispatcher("../result.jsp").forward(request, response);;
        }
    }
    ```
3. jsp接收返回参数

    ```jsp
    <%@ page language="java" import="java.util.*" pageEncoding="utf-8"%>
    <%
        String path = request.getContextPath();
        String basePath = request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+path+"/";
    %>
    <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
    <html>
        <head>
            <meta charset="utf-8">
            <title>世界太美好</title>
        </head>
        <body>
            <p>result ： ${requestScope.result}</p>
            <!--通过 el表达式   ${}来读取数据，由于它具有严格的作用域限制，所以 通过requestScope来调用-->
        </body>
    </html>

    ```