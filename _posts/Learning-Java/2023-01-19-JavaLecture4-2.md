---
title: "[서블릿/JSP(뉴렉처)] 54 ~ "
excerpt: "서블릿/JSP 공부"

categories:
  - Learning-Java
tags:
  - [Servlet, JSP]

toc: true
toc_sticky: true
toc_label: "페이지 주요 목차"

date: 2023-01-19
last_modified_at: 2023-01-19
---

<br>

# [유튜브 강의 주소](https://www.youtube.com/watch?v=drCj2k50j_k&list=PLq8wAnVUcTFVOtENMsujSgtv2TOsMy8zd)

<br>

# 보는 방법

- 강의가 화면 자료에 내용 설명이 적고, 적당히 설명해서 구체적으로 메모 힘들다. 따라서 키워드 위주나 내용 간단 요약 느낌으로 기록했다.
- 강의 내용 복습은 메모한 것뿐만 아니라 강의 영상도 봐라. 이는 상기용/테스트용 정도로만 활용해라.
- 여기서는 다음을 기록.
  - 강의 목차와 키워드나 내용 요약
  - (가끔씩) 강의를 보고 생긴 의문에 대해 구글링해서 찾은 내용

<br><br>

# 강의 54 - 자세한 페이지 MVC model1으로 변경하기

- detail.jsp에 MVC model1 적용

```jsp
<%@page import="java.util.Date"%>
<%@page import="java.sql.PreparedStatement"%>
<%@page import="java.sql.ResultSet"%>
<%@page import="java.sql.DriverManager"%>
<%@page import="java.sql.Connection"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>

<%
int id=Integer.parseInt(request.getParameter("id"));

String url = "jdbc:oracle:thin:@localhost:1521/xepdb1";
String sql = "select * from notice where id=?";

Class.forName("oracle.jdbc.driver.OracleDriver");
Connection con = DriverManager.getConnection(url, "newlec", "1234");
PreparedStatement st = con.prepareStatement(sql);
st.setInt(1, id);

ResultSet rs = st.executeQuery();
rs.next();

String title=rs.getString("title");
String writerId=rs.getString("writer_id");
Date regdate=rs.getDate("regdate");
String hit=rs.getString("hit");
String files=rs.getString("files");
String content=rs.getString("content");

rs.close();
st.close();
con.close();
%>

<!-- 중략 -->

<div class="margin-top first">
	<h3 class="hidden">공지사항 내용</h3>
	<table class="table">
		<tbody>
			<tr>
				<th>제목</th>
				<td class="text-align-left text-indent text-strong text-orange" colspan="3"><%=title %></td>
			</tr>
			<tr>
				<th>작성일</th>
				<td class="text-align-left text-indent" colspan="3"><%=regdate %></td>
			</tr>
			<tr>
				<th>작성자</th>
				<td><%=writerId %></td>
				<th>조회수</th>
				<td><%=hit %></td>
			</tr>
			<tr>
				<th>첨부파일</th>
				<td colspan="3"><%=files %></td>
			</tr>
			<tr class="content">
				<td colspan="4"><%=content %></td>
			</tr>
		</tbody>
	</table>
</div>

<!-- 중략 -->
```

<br><br>

# 강의 55 - MVC model2 방식으로 변경하기

- MVC model2: 뷰와 컨트롤러를 물리적으로 나누는 방식

  - 적용 이유
    - 뷰와 컨트롤러 개별적으로 유지관리 가능. 또 재사용성 증가
    - 실행 면에서도 도움. 요청이 오면 jsp는 서블릿으로 변환되고 컴파일 과정도 거치게 된다. 따라서 컨트롤러(자바 코드) 부분이 뷰와 같이 있으면 두 코드를 합쳐서 요청이 올 때마다 서블릿으로 바꾸고 컴파일해야 한다. 서로 분리하면 컨트롤러 부분을 미리 컴파일할 수 있기에, 실행 성능에서도 유리하다.

- NoticeDetailController.java
  - 기존의 detail.jsp의 코드 블록 내용을 복붙한 게 바탕이므로 추가로 넣은 것만 주석 처리 안 함.

```java
package com.newlecture.web.controller;
// import부분 생략

@WebServlet("/notice/detail")
public class NoticeDetailController extends HttpServlet{
	@Override
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// int id=Integer.parseInt(request.getParameter("id"));

		// String url = "jdbc:oracle:thin:@localhost:1521/xepdb1";
		// String sql = "select * from notice where id=?";

		try {
			// Class.forName("oracle.jdbc.driver.OracleDriver");
			// Connection con = DriverManager.getConnection(url, "newlec", "1234");
			// PreparedStatement st = con.prepareStatement(sql);
			// st.setInt(1, id);

			// ResultSet rs = st.executeQuery();
			// rs.next();

			// String title=rs.getString("title");
			// String writerId=rs.getString("writer_id");
			// Date regdate=rs.getDate("regdate");
			// String hit=rs.getString("hit");
			// String files=rs.getString("files");
			// String content=rs.getString("content");

			request.setAttribute("title", title);
			request.setAttribute("writerId", writerId);
			request.setAttribute("regdate", regdate);
			request.setAttribute("hit", hit);
			request.setAttribute("files", files);
			request.setAttribute("content", content);

			// rs.close();
			// st.close();
			// con.close();
		} catch (ClassNotFoundException e) {
			e.printStackTrace();
		} catch (SQLException e) {
			e.printStackTrace();
		}

		request
		.getRequestDispatcher("/notice/detail.jsp")
		.forward(request, response);
	}
}
```

- detail.jsp
  - `<%=request.getAttribute("writerId")%>`거만 수정했다.

```jsp
<div class="margin-top first">
	<h3 class="hidden">공지사항 내용</h3>
	<table class="table">
		<tbody>
			<tr>
				<th>제목</th>
				<td class="text-align-left text-indent text-strong text-orange" colspan="3"><%=request.getAttribute("title")%></td>
			</tr>
			<tr>
				<th>작성일</th>
				<td class="text-align-left text-indent" colspan="3"><%=request.getAttribute("regdate")%></td>
			</tr>
			<tr>
				<th>작성자</th>
				<td><%=request.getAttribute("writerId")%></td>
				<th>조회수</th>
				<td><%=request.getAttribute("hit")%></td>
			</tr>
			<tr>
				<th>첨부파일</th>
				<td colspan="3"><%=request.getAttribute("files")%></td>
			</tr>
			<tr class="content">
				<td colspan="4"><%=request.getAttribute("content")%></td>
			</tr>
		</tbody>
	</table>
</div>
```

- list.jsp
  - `<td class="title indent text-align-left"><a href="detail?id=<%=rs.getInt("id") %>"><%=rs.getString("title") %></a></td>`로 수정. 원래는 href="detail.jsp?~~"였다.

<br><br>

# 강의 56 - Model 데이터를 구조화하기

- 엔티티
  - 아래 데이터들은 개념적으로 공지사항으로 묶을 수 있음.
  - 개념화된 데이터(개념적으로 말할 수 있는 데이터 집합).
  - 구조적인 데이터: 아래 데이터들을 묶어서 계층을 만들었다고 해서
  - 아래 데이터를 다음 한 줄로 수정: `request.setAttribute("notice", notice);`

```java
request.setAttribute("title", title);
request.setAttribute("writerId", writerId);
request.setAttribute("regdate", regdate);
request.setAttribute("hit", hit);
request.setAttribute("files", files);
request.setAttribute("content", content);
```

- Notice.java 생성(엔티티)

```java
package com.newlecture.web.entity;

import java.util.Date;

public class Notice {
	private int id;
	private String title;
	private String writerId;
	private Date regdate;
	private String hit;
	private String files;
	private String content;

	public Notice() {
		// TODO Auto-generated constructor stub
	}

	public Notice(int id, String title, String writerId, Date regdate, String hit, String files, String content) {
		this.id = id;
		this.title = title;
		this.writerId = writerId;
		this.regdate = regdate;
		this.hit = hit;
		this.files = files;
		this.content = content;
	}

	// getter와 setter  생략

	@Override
	public String toString() {
		return "Notice [id=" + id + ", title=" + title + ", writerId=" + writerId + ", regdate=" + regdate + ", hit="
				+ hit + ", files=" + files + ", content=" + content + "]";
	}

}
```

- NoticeDetailController.java 수정
  - 주석 부분을 대체

```java
Notice notice=new Notice(id,title,writerId,regdate,hit,files,content);
request.setAttribute("n", notice);

// request.setAttribute("title", title);
// request.setAttribute("writerId", writerId);
// request.setAttribute("regdate", regdate);
// request.setAttribute("hit", hit);
// request.setAttribute("files", files);
// request.setAttribute("content", content);
```

- detail.jsp 수정
  - 다 ${n.title}느낌으로 수정

```jsp
<tbody>
<tr>
	<th>제목</th>
	<td class="text-align-left text-indent text-strong text-orange" colspan="3">${n.title}</td>
</tr>
<tr>
	<th>작성일</th>
	<td class="text-align-left text-indent" colspan="3">${n.regdate}</td>
</tr>
<tr>
	<th>작성자</th>
	<td>${n.writerId}</td>
	<th>조회수</th>
	<td>${n.hit}</td>
</tr>
<tr>
	<th>첨부파일</th>
	<td colspan="3">${n.files}</td>
</tr>
<tr class="content">
	<td colspan="4">${n.content}</td>
</tr>
</tbody>
```
