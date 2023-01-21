---
title: "[서블릿/JSP(뉴렉처)] 54 ~ 62강, 69강"
excerpt: "서블릿/JSP 공부"

categories:
  - Learning-Java
tags:
  - [Servlet, JSP]

toc: true
toc_sticky: true
toc_label: "페이지 주요 목차"

date: 2023-01-19
last_modified_at: 2023-01-20
---

<br>

# [유튜브 강의 주소](https://www.youtube.com/watch?v=drCj2k50j_k&list=PLq8wAnVUcTFVOtENMsujSgtv2TOsMy8zd)

<br>

# 참고사항

- 61강 이후부터는 핵심적인 개념이 있으면 기록하고, 아니면 넘기려고 한다.
- 나는 스프링의 뼈대가 되는 개념과 이론을 원해서 servlet과 jsp 강의를 보았다. 이걸로 개발할 건 아니기 때문에 그렇다.

# 보는 방법

- 강의가 화면 자료에 내용 설명이 적고, 적당히 설명해서 구체적으로 메모 힘들다. 따라서 키워드 위주나 내용 간단 요약 느낌으로 기록했다.
- 강의 내용 복습은 메모한 것뿐만 아니라 강의 영상도 봐라. 이는 상기용/테스트용 정도로만 활용해라.
- 여기서는 다음을 기록.
  - 강의 목차와 키워드나 내용 요약

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

<br><br>

# 강의 57 - 목록 페이지도 MVC model2로 수정하기

- 전에 했던 것과 비슷. 복습용 느낌
- NoticeListController.java 생성
  - Notice 게시글이니까 list에 담아서 전달

```java
@WebServlet("/notice/list")
public class NoticeListController extends HttpServlet {
	@Override
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		List<Notice> list=new ArrayList<>();

		String url = "jdbc:oracle:thin:@localhost:1521/xepdb1";
		String sql = "select * from notice";

		try {
			Class.forName("oracle.jdbc.driver.OracleDriver");
			Connection con = DriverManager.getConnection(url, "newlec", "1234");
			Statement st = con.createStatement();
			ResultSet rs = st.executeQuery(sql);

			while(rs.next()) {
				int id=rs.getInt("id");
				String title=rs.getString("title");
				String writerId=rs.getString("writer_id");
				Date regdate=rs.getDate("regdate");
				String hit=rs.getString("hit");
				String files=rs.getString("files");
				String content=rs.getString("content");

				Notice notice=new Notice(id,title,writerId,regdate,hit,files,content);
				list.add(notice);
			}


			rs.close();
			st.close();
			con.close();
		} catch (ClassNotFoundException e) {
			e.printStackTrace();
		} catch (SQLException e) {
			e.printStackTrace();
		}

		request.setAttribute("list", list);
		request
		.getRequestDispatcher("/notice/list.jsp")
		.forward(request, response);
	}
}
```

- list.jsp 수정
  - `request.getAttribute("list")`해서 request로 전달받은 list를 다시 지역변수에 담는 이유는 list의 각 notice를 꺼내서 쓰는 걸 반복해야 하기 때문이다.
  - `pageContext.setAttribute("n", n);`하는 이유: EL에서는 자바의 지역변수는 쓸 수 없다. page, request, session, application 저장소에 있는 것만 꺼내올 수 있다.

```jsp
{%raw%}
<%
	List<Notice> list=(List<Notice>) request.getAttribute("list");
	for(Notice n:list) {
		pageContext.setAttribute("n", n);
%>
<tr>
	<td>${n.id }</td>
	<td class="title indent text-align-left"><a href="detail?id=${n.id }">${n.title}</a></td>
	<td>${n.writerId }</td>
	<td>${n.regdate }</td>
	<td>${n.hit }</td>
</tr>
<%} %>
{%endraw%}
```

<br><br>

# 강의 58 - View 페이지 은닉하기

- 뷰는 더이상 사용자가 직접 요청해서는 안되는 페이지다. 컨트롤러로 출력할 일이 있을 때만, 컨트롤러가 선택적으로 실행시킬 녀석임
  - WEB-INF 폴더는 외부에 서비스되지 않는 파일들(예로 설정 파일, 라이브러리, 코드 파일)이 여기에 있다.
  - 이 예제에서 admin, member, notice, student 폴더는 뷰 페이지를 가지고 있는 폴더다. 사용자가 직접 요청하는 것도 아니고, 컨트롤러에 의해서 선택적으로 사용될 파일들(동적 파일들)이 모여있다. 그래서 이러한 폴더들을 WEB-INF 폴더 속 view라는 폴더를 만들어서 보관해두자
  - 그럼 각 컨트롤러 파일에서 다음 부분을 수정하자.
    - NoticeListController 속 `getRequestDispatcher("/WEB-INF/view/notice/list.jsp")`로 수정.
    - 또 NoticeDetailController 속 `getRequestDispatcher("/WEB-INF/view/notice/detail.jsp")`로 수정

<br><br>

# 강의 59 - View(list.jsp)에서 반복문 제거하기

## jstl 관련 오류 문제 해결

- 강의대로 jstl 1.2버전 다운 받으니 문제였다.
- [참고 사이트](https://stackoverflow.com/questions/4928271/how-to-install-jstl-the-absolute-uri-http-java-sun-com-jstl-core-cannot-be-r)
- 23년 1월 20일의 나는 톰켓 10.0버전을 쓰고 있다. 따라서 그에 맞게 jstl 2.0버전을 다운받았다.
- 그 후 문제없이 작동했다.

## 깅의 내용

- view에서 사용하는 제어구조
  - 자바의 반복문을 이용한 제어구조에서 태그를 이용한 제어구조로
- JSTL 다운로드
  - 위의 태그를 사용할 수 있도록 하는 라이브러리 다운
  - 다운 후 WEB-INF 속 lib 폴더에 넣기
  - 무슨 의미인지 모르겠지만 태그 라이브러리를 쓰기 위해서 다음 설정을 해야 함.
    - `<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>`
      - "prefix(접두사) c라는 녀석을 찍어보면 우리가 쓸 수 있는 태그들이 보일 겁니다."
    - `<c:forEach var="n" items="${list}">`
      - items라는 속성: 저장소에 담겨있는 녀석을 뽑아서 items에 담으면, 반복할 때마다 items에서 하나하나씩 꺼내서 반복됨
      - var: items에서 꺼낸 녀석을 담는 녀석. n이라는 키워드로 pageContext에 담아주고, forEach가 반복해서 실행함.

```jsp
<c:forEach var="n" items="${list}">
<tr>
	<td>${n.id }</td>
	<td class="title indent text-align-left"><a href="detail?id=${n.id }">${n.title}</a></td>
	<td>${n.writerId }</td>
	<td>${n.regdate }</td>
	<td>${n.hit }</td>
</tr>
</c:forEach>
```

<br><br>

# 강의 60 - Tag 라이브러리와 JSTL

- jstl(java standard tag library)
  - 5가지 범주의 태그 라이브러리 지원
    - core: 제어의 행위를 담당하는 태그 라이브러리
    - formating: 날짜, 숫자 등을 출력할 때 formating
    - functions: 문자열 조작이 필요할 때 사용하는 함수 모아놓음
    - sql, xml
      - 이 두 개는 쓰지 않는게 바람직함. mvc 나오기 전에 뷰단에 로직들 처리할 때 썼음. 코드의 구성이 깨짐
      - sql문, xml 사용할 수 있게 해줌
- jstl core
  - `<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>`
  - 접두사와 uri를 사용하는 이유: jasper가 jstl 태그를 이해할 수 있어야 하기 때문에
    - 접두사 c를 붙이면 이는 곧 uri가 제공하는 거라고 설명하게 됨. 따라서 uri통해서 이 태그 의미를 이해할 수 있음
- 태그 라이브러리 원리
  - 태그 라이브러리 만들 때는 TagSupport를 상속받아서 클래스를 만들면 됨
  - 또한 tag library descriptor, 즉 태그 라이브러리를 만들고 쓰기 위해서 필요한 설명자(.tld 파일)가 필요
    - uri: 태그를 구분해주는 식별자(이름이 관리되고 있는 도메인 이름은 중복되지 않음) 역할이다. 만들려고 하는 태그 이름은 다른 사람의 태그와 중복될 수 있기 때문이다.
  - 그럼 태그 쓸 때 중복 피하도록 uri를 붙이자니 너무 기니까 접두사를 정해서 태그 앞에 붙임.
  - 그럼 jasper가 이것이 태그 라이브러러임을 알게 됨.

<br><br>

# 강의 61 - 중간 정리

- 서블릿은 자바 웹 프로그램을 만들 때 쓴다. 웹에서 입력(request)과 출력(response)을 적절히 설정한 후 웹 문서를 출력했다.
- html 문서를 출력하다보니 문제가 많아서 문서 기반(jsp)의 코드블록을 사용했다.
- 하지만 이 또한 코드가 스파게티처럼 되는 문제가 있어서 MVC 패턴이 도입되었다.
- mvc 패턴을 쓸 때 뷰에서 코드 블록이 꼭 필요한 경우, EL과 JSTL을 사용한다.

<br><br>

# 강의 62 - forEach의 속성 사용하기

- begin과 end
  - 반복되는 forEach에서 범위를 한정할 수 있음
  - forEach로 반복해서 출력할 때, 출력되는 순서에 따라 0,1,2...의 인덱스가 있다고 보면 된다.
  - 이 때 `<c:forEach var="n" items="${list}" begin="2" end="3"> `처럼 사용하면 2~3인덱스 내용만 출력된다.
- varStatus
  - forEach가 반복하면서 index를 사용하는데, 이런 index와 같은 상태값을 얻을 수 있음
  - 상태값을 알기 위한 변수명만 설정하면 된다.
  - `<c:forEach var="n" items="${list}" varStatus="st">`처럼 쓰면 된다.
  - st가 가지고 있는 상태값(사용은 `${st.index} 느낌`)
    - current(현재 사용 중인 객체(var="n"에서 n))
    - index: 0부터 시작되서 반복할 때마다 느는 값
    - count: 반복되고 있는 횟수
    - first: 현재 반복됙 있는게 첫 번째 아이템이면 true, 아니면 false를 반환
    - last: 현재 반복됙 있는게 마지막 아이템이면 true, 아니면 false를 반환
    - begin, end: 우리가 설정한 begin, end 값
    - step: 만약 이걸 2로 놓으면 2씩 증가해서 반복되게 됨

<br><br>

# 강의 69 - 기업형으로 레이어를 나누는 이유와 설명

- 서블릿만으로 웹 서비스 만듦
- -> 서블릿을 분리(컨트롤러/뷰)
  - 컨트롤러(servlet): 요청을 처리하고 출력할 데이터인 모델을 만듦
  - 뷰(jsp): 모델을 받아서 출력만 함
- -> 컨트롤러에서 업무 서비스를 분리
  - 컨트롤러: 요청 처리하고, 모델은 넘겨받아서 뷰한테 줌
  - 업무 서비스: 요청 받은 후 모델을 만들어서 줌
  - 미숙한 개발자는 컨트롤러를, 숙련된 개발자는 업무 서비스를
- -> 업무 서비스에서 DAO(데이터 서비스)를 분리
  - 업무 서비스: entity(개념화된 데이터)를 넘겨받아서 model을 만듦
  - DAO: entity(개념화된 데이터)를 만듦
