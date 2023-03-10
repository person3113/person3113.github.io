---
title: "[서블릿/JSP(뉴렉처)] 1 ~ 62강, 69강"
excerpt: "서블릿/JSP 공부"

categories:
  - Learning-Java
tags:
  - [Servlet, JSP]

toc: true
toc_sticky: true
toc_label: "페이지 주요 목차"

date: 2023-01-16
last_modified_at: 2023-02-01
---

<br>

# [유튜브 강의 주소](https://www.youtube.com/watch?v=drCj2k50j_k&list=PLq8wAnVUcTFVOtENMsujSgtv2TOsMy8zd)

<br>

# 보는 방법

- 강의가 화면 자료에 내용 설명이 적고, 적당히 설명해서 구체적으로 메모 힘들다. 따라서 키워드 위주나 내용 간단 요약 느낌으로 기록했다.
- 강의 내용 복습은 메모한 것뿐만 아니라 강의 영상도 봐라. 이는 상기용/테스트용 정도로만 활용해라.
- 여기서는 다음을 기록.
  - 강의 목차와 키워드나 내용 요약

<br>

# 참고사항

- 61강 이후부터는 핵심적인 개념이 있으면 기록하고, 아니면 넘기려고 한다.
- 나는 스프링의 뼈대가 되는 개념과 이론을 원해서 servlet과 jsp 강의를 보았다. 이걸로 개발할 건 아니기 때문에 그렇다.

<br><br>

# 강의 01 - 학습 안내

- 서블릿으로 자바 웹 프로그램 만들고, 서블릿의 html 코드 출력 문제를 도와주는 jsp, jsp 코드를 정리하는 jsp mvc.

<br><br>

# 강의 02 - 웹 서버 프로그램이란

- CS(클라이언트/서버) 프로그램의 문제점: 클라이언트 프로그램 업데이트의 어려움(재설치)
- 웹 프로그래밍이라는 것은 웹을 이용해서 cs 프로그램을 만든다는 것.
- 웹은 클라이언트 프로그램 따로 없이 브라우저 하나만 있으면 가능. 웹 서버에서 문서 받아서 보여주는 역할.
- 기존에 만들어진 정적 페이지를 전달하는 웹 서버의 환경을 조금 바꿔서, 동적으로 페이지 만들 수 있는 환경을 추가. 그래서 웹 서버 쪽에는 서버 프로그램을 얹을 수 있는 환경을 만들게 됨.
- 자바스크립트 등장 이후 페이지를 요청하는 것이 아니라, 데이터 요청으로 바뀌게 됨. 그 후 브리우저 단에다가 프로그램 만드는 것처럼 자바스크립트 이용해서 만들게 됐음. 즉 브라우저 기반으로 쿨라이언트 프로그램 만들게 되었음. 따라서 이를 프론트엔드로, 서버 단은 백엔드로 구분

<br><br>

# 강의 03 - 웹 서버 프로그램과 Servlet

- 웹 기반의 클라이언트/서버(cs) 프로그램
  - 클라이언트가 웹 문서를 요청하면 웹 서버는 요청한 웹 문서를 찾은 후 돌려줄 것임.
  - 하지만 클라이언트가 회원 목록을 요청하면, 회원 목록이라는 것이 미리 문서로서 만들어질 수 없다. 계속 회원의 정보는 바뀔 수 있기 때문이다.
  - 따라서 이 때는 웹 문서가 아니라 목록을 만들어 내기 위한 코드가 있음. 그럼 웹 서버는 이러한 요청을 수반할 수 있는 코드를 실행해서 db에서 목록을 문서화해서 돌려줘야 함. 따라서 이 코드를 실행할 환경이 따로 필요함
  - 즉 사용자가 요구하는 내용이 동적인 문서를 요구한다면, 웹 서버에다가 추가적으로 관련 코드를 실행할 수 있는 환경이 필요한데, 이 환경을 WAS(웹 어플리케이션 서버)라고 한다.
  - was에서 코드 실행되고 목록 받아서 클라이언트에 돌려주는 형식으로 cs 프로그램을 만들게 되는 것
  - 이 때 was에서 실행되는 코드를 server app(서버 어플리케이션 / 동적으로 문서를 만들기 위한 코드)이라고 한다.
- 웹 서버 응용 프로그램(server app,서버 어플리케이션)을 servlet 단위로 만듦.
  - 클라이언트가 수정/삭제/회원 목록 등을 요청하면, 각 요청을 수반할 수 있는 서버 어플리케이션이 읽혀질 것임.
  - 이 때 서버 어플리케이션은 조각나 있는데(수정/삭제 등 각 요청에 맞는 서버 앱으로 파편화되어 있음), 이를 server application let(서버 어플리케이션 조각)으로 표현할 때 이를 줄여서 servlet이라고 하지 않았을까 추측한다고 말함.
  - 즉 서버 어플리케이션을 서블릿 단위로 만듦

<br><br>

# 강의 04 - 톰캣 9 설치하기 #1 of 3

- 23년 1월 16일인 현재 나는 톰켓 10을 다운받았다.
- 보면 window.zip, window service installer 두 가지를 설치할 수 있지만, 차이점은 다음과 같다.
  - window.zip: 학습용, 개발용으로 적합하다.
  - window service installer: 컴퓨터는 부팅되자마자 실행되는 서비스들이 있음. 이걸로 설치하면 톰켓이 서비스 목록에 등록됨. 따라서 서비스 목적으로 적합함.
  - 따라서 현재는 학습용으로 다운받으므로 window.zip으로 설치
- 톰켓 파일 속 bin 폴더에 startup.bat을 실행시키면 콘솔창이 떴다 바로 꺼질 수 있는데, 이는 두 가지 원인일 수 있다.
  - 먼저 환경변수에 JAVA_HOME이라는 변수가 없어서 그럴 수 있다. 톰켓의 경우 jdk가 필요한데, jdk가 어느 디렉토리에 설치되었는지를 알기 위해 JAVA_HOME 변수에 jdk의 디렉토리 경로를 등록해야 한다.
  - 두 번째는 다른 톰켓이 실행 중이여서 포트 번호 충돌로 안 될 수 있다.
- 다시 startup.bat을 실행한 후 localhost:8080에 접속하면 톰켓 페이지가 나온다.

<br><br>

# 강의 05 - 톰캣 9 설치하기 #2 of 3 - 웹문서 추가해보기

- 톰켓은 was이기도 하고, 웹 서버라고 볼 수도 있다. 이 강의에선 톰켓을 웹 서버로서 사용해보자.
- 문서를 보관하고 있는 곳을 홈 디렉토리라고 한다. 만약 내가 test.txt 파일을 홈 디렉토리에 두고, localhost:8080/test.txt를 치면(test.txt파일을 요청하면) test.txt 파일이 제공됨.
- 홈 디렉토리는 apache-tomcat-10.0.27\webapps\ROOT 폴더이다.

<br><br>

# 강의 06 - 톰캣 9 설치하기 #3 of 3 - Context 사이트 추가하기

- 개요
  - 홈 디렉토리에 폴더가 많아지고 각 폴더를 분업해서 맡고 있다면, Context 사이트를 고려할 수 있다.
  - 컨텍스트 사이트는 가상 사이트라고도 얘기하기도 한다. 기존 웹 사이트 폴더가 네이버라고 하자. 그럼 뉴스라는 폴더를 기존 네이버라는 폴더에서 땐 후에, 전혀 다른 폴더에서 웹 사이트를 개발한다. 하지만 뉴스는 기존 웹 사이트의 하위 디렉토리에서 돌아가는 것처럼 보여지는 방식으로 서비스하는 것을 컨텍스트 사이트라고 한다.
- 실습

  - 아래 내용은 이해를 위한 거지 실효성이 있는 건 아님. 왜냐면 server.xml파일을 수정해서 컨텍스 사이트를 추가하면, 그 후 다시 서버를 껐다 켜야해서 큰 문제다. 따라서 각 앱마다 메타인포라는 곳에서 따로 컨텍스트를 마련할 수 있는데, 이는 수업 범위가 아니라서 다루지 않는다.
  - conf(config) 폴더에서 server.xml이 있다. 그럼 그 안에 host 태그 안에 다음처럼 넣으면 된다.
  - 아래 코드를 이해하면, test란 폴더는 기존 폴더에 없지만, docBase에 지정한 디렉토리를 test라는 가상 디렉토리와 연결해서 서비스가 될 수 있도록 해라는 뜻.

  ```xml
  <Host name="localhost" appBase="webapps">
    <Context path="test" docBase="C:\어쩌구저쩌구\apache-tomcat-10.0.27\webapps\test" privileged="true">
  </Host>
  ```

<br><br>

# 강의 07 - 처음으로 서블릿 프로그램 만들어보기

- 왜 웹 서버 응용프로그램(서버 어플리케이션)을 servlet이라고 명칭할까?
  - 서버 어플리케이션은 기능별로 코드가 나눠져 있고, 클라이언트가 요청하는 것에 해당하는 코드만 실행됨. 즉 필요에 따라서 로드될 수 있도록 조각나 있는 서버 어플리케이션을 servlet이라고 함
- 서블릿 코드 작성
  - 모든 서블릿 클래스들은 was에 의해서 실행됨. was는 약속되어있는 인터페이스나 추상 클래스를 구현하거나 상속받은 서블릿 클래스를 참조하게 됨. 이 때 기존의 main() 역할이 servlet에서는 service()라고 보면 됨.

```java
import javax.servlet.*;
import javax.servlet.http.*;
import java.io.*;

public class Test extends HttpServlet{
    public void service(HttpServletRequest request, HttpServletResponse response) throws IOExceptionm ServletException{
        // 코드
    }
}
```

<br><br>

# 강의 08 - 서블릿 객체 생성과 실행 방법

- 톰켓을 통해서 서블릿 코드가 실행되도록 코드 배치
  - apache-tomcat-10.0.27\webapps\ROOT\WEB-INF 안 classes 폴더 속에 서블릿 코드를 배치한다.
  - 이 때 서블릿 코드가 어떤 패키지 속에 있다면 그 패키지명대로 classes 폴더 속에 넣어야 함. 예를 들어 패키지 이름이 com.newlec이라면 classes 폴더 속에 com 폴더 만들고, 그 안에 newlec 폴더 만든 후에, 다시 그 안에다 서블릿 클래스 파일을 둬야 함.
- 서블릿이 실행되는 방식
  - WEB-INF는 클라이언트가 요청할 수 없고, 오직 서버 쪽에서만 사용할 수 잇는 폴더다.
  - 따라서 서블릿 내용은 톰켓(웹서버+was)만 알면 됨. 서블릿은 톰켓이 실행할 테니 클라이언트는 어떤 거를 원하는지만 말해라는 느낌.
  - 즉 클라이언트가 특정 URL로 요청하면, 그 url에 매핑된 서블릿 코드를 찾아서 실행 후 돌려줌.
- 서블릿 코드를 url과 매핑하기

  - WEB-INF에 web.xml 파일이 있다. 그 안에 다음 코드를 적당히 넣으면 된다.
  - com.newlec.test의 뜻: test라는 클래스의 패키지명(com.newlec)까지 포함해서 작성
  - localhost:8080/testUrl로 접속하면, 일단 톰켓(여기선 웹서버)가 testUrl이라는 파일을 홈 디렉토리에서 찾아봄. 파일이 없으면 다시 톰켓(여기선 was)이 자신의 매핑 정보를 뒤져서 그에 매핑되는 test 클래스(서블릿 코드)가 실행됨

  ```xml
  <servlet>
      <servlet-name>test</servlet-name>
      <servlet-class>com.newlec.test</servlet-class>
  </servlet>

  <servlet-mapping>
      <servlet-name>test</servlet-name>
      <url-pattern>/testUrl</url-pattern>
  </servlet-mapping>
  ```

<br><br>

# 강의 09 - 서블릿(Servlet) 문자열 출력

- 자바 웹 프로그래밍: 여기서 웹은 UI이다. 사용자 입력을 받거나 출력 결과를 보여주는 역할이 UI다. 이 때 ui는 기존에는 콘솔, 윈도우 등을 떠올릴 수 있다. 하지만 자바 웹 프로그래밍에서는 ui로 웹을 사용하는 프로그래밍이라는 뜻이다.
- response를 이용한 출력방법 1
  - `OutputStream os= response.getOutputStream();`: 자바는 입출력을 스트림을 통해 수행한다. 여기서는 네트워크 스트림을 쓴다.
  - `PrintStream out = new PrintStream(os, true);`
    - OutputStream 출력하려고 보니, 바이너리나 바이트를 출력할 게 아니고 문자열을 출력할 거다. 따라서 이를 더 쉽게 쓸 수 있도록 하는 PrintStream로 OutputStream를 매핑해서 쓰는 것이 편하다.
    - PrintStream은 기본적인 출력 스트림을 가지고 좀 더 쉽게 문자열을 출력할 수 있도록 print계열의 함수들을 제공해 주고 있는 객체다.
    - 일반적으로 네트워크로 출력되는 스트림은 출력 버퍼가 일정 단위(윈도우는 기본적으로 8kb)가 채워지면 보낸다. 따라서 print했을 때 버퍼에 넣지 말고 바로 플러쉬해라는(출력해라는) 옵션이 true에 해당한다.
  - `out.println("Hello Servlet!");`: 기존의 System.out.println의 경우 콘솔창에 출력되는 거고, 아래 코드의 println은 클라이언트에게 전달되는 거다.

```java
import javax.servlet.*;
import javax.servlet.http.*;
import java.io.*;

public class Test extends HttpServlet{
    public void service(HttpServletRequest request, HttpServletResponse response) throws IOExceptionm ServletException{
        OutputStream os = response.getOutputStream();
        PrintStream out = new PrintStream(os, true);
        out.println("Hello Servlet!");
    }
}
```

- response를 이용한 출력방법 2
  - 근데 단지 문자열을 다룰거면 PrintWriter를 쓴다.
  - 근데 전에는 PrintStream이었는데, 여기서는 PrintWriter를 쓰는가? 자바에 stream 계열과 writer 계열이 있는데, 문자열을 쓸거고 그게 다국어라면 무조건 PrintWriter를 쓰는게 기본이다. 우리는 영어가 아니라 한국어니까 writer 계열을 쓴다. 즉 다국어 문자열을 사용할 수 있다는 점

```java
import javax.servlet.*;
import javax.servlet.http.*;
import java.io.*;

public class Test extends HttpServlet{
    public void service(HttpServletRequest request, HttpServletResponse response) throws IOExceptionm ServletException{
        PrintWriter out = response.getWriter();
        out.println("Hello Servlet!");
    }
}
```

<br><br>

# 강의 10 - 웹 개발을 위한 이클립스 IDE 준비하기

## vsCode

- 난 vsCode로 쭉 할 거기 때문에 강의 내용과 별개로 몇개 기록하겠다.
- 톰켓 서버를 vsCode에서 사용하기 위한 익스텐션은 Community Server Connectors다. 몇년 전 블로그를 보면 Tomcat for Java 익스텐션을 다운받아서 사용하였다. 하지만 이는 deprecated 상태라 다운 받을 수 없게 되었다.
- 익스텐션 다운 후 explorer 탭 안에서 java projects 탭 밑에 servers 탭이 생긴 것을 알 수 있다. 그럼 거기서 새 서버를 추가할 수 있다.
- [참고 사이트](https://code.visualstudio.com/docs/java/java-tomcat-jetty)니까 다시 볼 필요 있을 때 참고할 것

## eclipse

- vsCode로 해당 강의를 실습하려 했으나, servlet 관련해서 마땅한 도구가 없어서 무리라고 판단하고 이클립스로 진행하려고 한다.
- Eclipse IDE for Enterprise Java and Web Developers를 다운받는다.
- dynamic web project를 생성한다.

<br><br>

# 강의 11 - 이클립스를 이용한 서블릿 프로그래밍

## 이클립스 프로젝트, 서버 관련 오류 해결 기록

- html 파일을 run on server할 때 목록이 안 보여서 한동안 진행하지 못했다. 그리고 이것저것 찾아보면서 조작해도 안 되서 막막한 참에 다시 해보니까 갑자기 된다?
  - 차이를 떠올려보면, 프로젝트 생성할 때 Target runtime에서 new runtime버튼 클릭해서 설정할 때 밑에 있는 "create a new local server"라는 체크박스에 체크한 후 나머진 동일하게 진행했다.
  - 그 후 다시 시도해 보니까 run on server에서 목록이 보이긴 했다. 그런데 보니까 run on server에서 choose an existing server를 선택할 수 있게 된 것이다. 단 manually define a new server쪽은 선택 안 되는 건 동일한 것 같다.
- 근데 신난 마음에 실행하려고 하니까 갑자기 vsCode에 test.html 파일이 뜬 것이다... 왜지 싶어서 고생한 결과, 알고보니, window에 web browser 탭이 있는데, 그 안에서 기본 설정은 1. default system ~~인 거였다. 이를 chrome로 변경 후 다시 시도해 보니 정상적으로 실행되었다.
  - 그 이유를 추측하건데, 내 컴퓨터가 기본적으로 html 파일을 열 때 vsCode로 열기로 설정해서 그렇지 않을까 추측해본다.

## 강의 내용

- 버전이 업데이트 됨에 따라서 강의와 다르게 작업해야 할 부분들이 있습니다!

  - Java Resources ->src/main/java, WebContent->webapps 로 기본 설정이 변경 됨. (MONE님이 발견 하심)
  - 프로젝트 이름에 우클릭 해야 하며 source folder는 src/main/java이다. (강의: src 폴더에 우클릭)
  - 우클릭 후 New>Servlet으로 해야한다. (강의: New>Class)
  - Nana Servlet을 호출하는 url 변경 시 web.xml에서 url-pattern 태그를 수정한다.

- 여기서 홈 디렉토리는 webapp이다.
- 내가 만든 프로젝트 폴더가 루트 프로젝트가 되도록 설정하기.
  - 프로젝트 우클릭->properties->web project settings->context root명을 `/`로 바꾼다.
- 서블릿 클래스를 매핑되야 한다. 매핑하기 위해서는 web.xml을 통해 매핑해야 한다. apache-tomcat-10.0.27\webapps\ROOT\WEB-INF 속 web.xml을 복사해서 새로 만든 프로젝트 속 WEB-INF로 붙여넣은 후, 현재 서블릿 클래스 전체 이름(예- com.newlecture.web.test)으로 수정한다.
  - 서블릿 코드 수정하면, 컴파일하고, 배포하고, 톰켓 서버 재시작하고해야 하는 일련의 과정을 이클립스에서 간단히 할 수 있다.

<br><br>

# 강의 12 - 어노테이션을 이용한 URL 매핑

- 어노테이션을 이용한 URL 매핑
  - web.xml에서 url 매핑할 필요 없다. 어노테이션이 좋은 이유는 여러 사람이 분업해서 만들때 각자가 분리한 상태에서 수행할 수 있기 때문이다. 왜냐면 각 사람마다 web.xml을 손대려 할 거니까 불편하지.
  - 어노테이션으로 url 매핑하려면, web.xml 속 metadata-complete를 "false"로 수정해야 한다.
  - metadata-complete이 true라면 모든 메타데이터(설정)이 web.xml에 있다는 소리다. 그럼 web.xml 파일만 뒤지고 url 매핑 시도할 것이다. 따라서 이를 false로 해야 한다.

```xml
<web-app xmlns="https://jakarta.ee/xml/ns/jakartaee"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="https://jakarta.ee/xml/ns/jakartaee
                      https://jakarta.ee/xml/ns/jakartaee/web-app_5_0.xsd"
  version="5.0"
  metadata-complete="false">

  <!-- <servlet>
      <servlet-name>test</servlet-name>
      <servlet-class>com.newlecture.web.test</servlet-class>
  </servlet>

  <servlet-mapping>
      <servlet-name>test</servlet-name>
      <url-pattern>/testUrl</url-pattern>
  </servlet-mapping> -->
```

```java
package com.newlecture.web;

import jakarta.servlet.*;
import jakarta.servlet.annotation.WebServlet;
import jakarta.servlet.http.*;
import java.io.*;

@WebServlet("/testUrl")
public class test extends HttpServlet {
	@Override
	protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		PrintWriter out = response.getWriter();
        out.println("Hello servlet");
	}
}
```

<br><br>

# 강의 13 - 서블릿 출력 형식을 지정해야 하는 이유

- 브라우저에 컨텐츠 형식을 알려주지 않은 경우 각자 자의적인 해석을 한다.
  - 크롬은 이 때 텍스트로 해석해서, `<br>` 태그가 그대로 출력된다.
  - 반면 엣지의 경우 이를 웹 문서(html)로 해석해서 `<br>` 태그가 출력되지 않고, 줄내림이 된 것을 알 수 있다.

```java
@WebServlet("/testUrl")
public class test extends HttpServlet {
	@Override
	protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		PrintWriter out = response.getWriter();

		for(int i=0;i<10;i++)
			out.println(i+" Hello servlet<br>");
	}
}
```

<br><br>

# 강의 14 - 한글과 콘텐츠 형식 출력하기

- 한글이 깨지는 이유
  - 서버에서 한글을 지원하지 않는 문자코드로 인코딩한 경우: 기본적으로 웹 서버가 클라이언트에게 보내는 단위가 ISO-8859-1 방식을 이용함. 따라서 한글은 2byte인데, 1byte씩 끊어서 클라이언트에게 전달함.
  - 서버에서는 UTF-8로 인코딩해서 보냈지만, 브라우저가 다른 코드로 잘못 해석한 경우: 클라이언트가 utf-8이 아닌 다른 인코딩 방식을 썼을 경우
- 코드 이해
  - `response.setCharacterEncoding("UTF-8");`: 서버가 utf-8 형식으로 보낸다는 뜻
  - `response.setContentType("text/html; charset=UTF-8");`: 클라이언트에게 컨텐트 타입이 어떤지 알려줌. http 응답 헤더에 컨턴트 타입을 명시해서 넣어줌

```java
@WebServlet("/testUrl")
public class test extends HttpServlet {
	@Override
	protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		response.setCharacterEncoding("UTF-8");
		response.setContentType("text/html; charset=UTF-8");
		PrintWriter out = response.getWriter();

		for(int i=0;i<10;i++)
			out.println(i+": 안녕, servlet<br>");
	}
}
```

<br><br>

# 강의 15 - GET 요청과 쿼리스트링

- GET 요청
  - 무엇을 달라고 하는 요청에 추가로 옵션이 붙을 수 있다.
  - 클라이언트는 기본적으로는 웹 문서를 요청한다(localhost:8080/hello). 하지만 localhost:8080/hello?cnt=3으로 hello라는 문서를 요청하면, cnt=3이라는 조건에 맞는 웹 문서를 서버가 돌려줘야 한다(강의에서는 hello라는 문자를 100번 반복해서 출력했는데, cnt=3이라고 하면 3번만 반복해서 출력해라고 뜻). 마치 헴버거를 주문하면서 추가로 양파는 빼달라는 옵션을 요청하는 것이다. -이 때 "?cnt=3"을 쿼리스트링이라고 한다.
- 코드 이해
  - requset(입력도구) 활용.
  - `request.getParameter("cnt")`: getParameter()는 "?cnt=3"이라는 쿼리스트링에서 키워드(여기선 cnt)를 읽는 함수. 그럼 이 때 cnt라는 키워드는 서버와 클라이언트가 서로 합의된 키워드를 써야 한다.

```java
@WebServlet("/testUrl")
public class test extends HttpServlet {
	@Override
	protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		response.setCharacterEncoding("UTF-8");
		response.setContentType("text/html; charset=UTF-8");
		PrintWriter out = response.getWriter();

		int cnt=Integer.parseInt(request.getParameter("cnt"));

		for(int i=0;i<cnt;i++)
			out.println(i+": 안녕, servlet<br>");
	}
}
```

<br><br>

# 강의 16 - 기본값 사용하기

- 쿼리스트링을 request.getParameter("cnt")할 때 전달되는 cnt값은?
  - "/hello?cnt=3" => "3"
  - "/hello?cnt=" => ""
  - "/hello?" => null
  - "/hello" => null
- 입력 값에 기본 값을 사용하기
  - `int cnt=10;`: 예제에서는 cnt의 기본값을 10으로 두었다.
  - `if(cnt_!=null&&!cnt_.equals("cnt"))`: getParameter()로 받은 값이 빈 문자열("")이나 null일 수 있음을 위에서 언급했다. 따라서 이런 경우일 땐 if문이 실행 안 되고 기본값인 10이 될 것이고, 실제 cnt 값이 들어올 경우는 해당 값을 받는 코드

```java
@WebServlet("/testUrl")
public class test extends HttpServlet {
	@Override
	protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		response.setCharacterEncoding("UTF-8");
		response.setContentType("text/html; charset=UTF-8");
		PrintWriter out = response.getWriter();

		int cnt=10; // 기본값
		String cnt_=request.getParameter("cnt");
		if(cnt_!=null&&!cnt_.equals(""))
			cnt=Integer.parseInt(cnt_);

		for(int i=0;i<cnt;i++)
			out.println(i+": 안녕, servlet<br>");
	}
}
```

- 사용자는 위의 cnt 값을 다음처럼 전달.
  - `<a href="testUrl">`: 인사하기(기본값) 링크를 누르면 localhost:8080/testUrl로 이동
  - `<a href="testUrl?cnt=3">`: 인사하기(기본값) 링크를 누르면 localhost:8080/testUrl?cnt=3"로 이동

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <title>Insert title here</title>
  </head>
  <body>
    환영합니다.<br />
    <a href="testUrl">인사하기(기본값)</a><br />
    <a href="testUrl?cnt=3">인사하기(3번)</a><br />
  </body>
</html>
```

<br><br>

# 강의 17 - 사용자 입력을 통한 GET 요청

- 반복횟수를 사용자로부터 입력받으려면 입력폼을 준비해야 한다.
  - `<form action="hello">`: 사용자가 채우는(입력하는) 양식을 뜻함. action에다가 서블릿 매핑 주소를 씀. 그럼 submit 버튼 누르면 요청이 /hello에게 요청이 감. 즉 브라우저는 action명을 보고 url을 작성하게 된다.
  - `<input type="text" name="cnt" />`: 여기에 입력된 값이 있는지 확인 후 있으면, name에 해당하는 키와 입력받은 값으로 쿼리스트링을 만들어 줌.
  - `<input type="submit" value="출력" />`: 요청을 하게 되는 submit 버튼

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <title>Insert title here</title>
  </head>
  <body>
    <div>
      <form action="hello">
        <div>
          <label>"안녕하세요"를 몇 번 듣고 싶으신가요?</label>
        </div>
        <div>
          <input type="text" name="cnt" />
          <input type="submit" value="출력" />
        </div>
      </form>
    </div>
  </body>
</html>
```

<br><br>

# 강의 18 - 입력할 내용이 많은 경우는 POST 요청

- POST 요청의 일반적인 방식
  - get 요청으로 바로 끝나는 경우도 있지만, 입력할 내용이 많을 경우 일정한 양식을 통해서 전달받는 경우(post요청)도 있다.
  - 즉 get요청과 post요청을 나눠서, 처음에는 입력폼을 받기 위한 get요청을 하고, 그걸 가지고 내용을 채워서 post함.
- reg.html파일을 추가로 만든다.
  - `<form action="notice-reg" method="post">`
    - `<form action="notice-reg">`
      - 기본적으로 method="post"가 없는 경우 get방식으로 내용을 전달한다. 이 경우 입력받은 값은 쿼리스트링으로 전달된다. 쿼리스트링은 기본적으로 웹 문서를 요청할 때 추가로 요청하는 사항을 뜻한다. 따라서 그렇게 많이 입력받을 준비가 덜 되어있다.
      - 쿼리스트링으로 붙어서 url이 길어지는데, url은 길이에 제한이 있어 내용이 잘릴 수 있다. 또한 아이디나 패스워드를 입력했을 때 그 내용이 쿼리스트링으로 붙으므로 보안 상의 문제도 있다.
    - `method="post"`: 입력받을 내용이 많기 때문에 url을 통해 전달하지 않고, 요청 바디에 붙어서 전달됨(쿼리스트링으로 전달되지 않음). 요청 바디의 경우 크기가 제한이 없어서 많은 내용을 받을 수 있다.
- 그럼 noticeReg.java 파일도 추가로 만든다.
  - `@WebServlet("/notice-reg")`: /notice-reg를 담당하는 servlet 코드를 만든다.

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <title>Insert title here</title>
  </head>
  <body>
    <div>
      <form action="notice-reg" method="post">
        <div><label>제목:</label><input name="title" type="text" /></div>
        <div>
          <label>내용:</label><br />
          <textarea name="content"></textarea>
        </div>
        <div>
          <input type="submit" value="등록" />
        </div>
      </form>
    </div>
  </body>
</html>
```

```java
@WebServlet("/notice-reg")
public class NoticeReg extends HttpServlet {
	@Override
	protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		response.setCharacterEncoding("UTF-8");
		response.setContentType("text/html; charset=UTF-8");
		PrintWriter out = response.getWriter();

		String title=request.getParameter("title");
		String content=request.getParameter("content");

		out.println(title+"<br>");
		out.println(content);
	}
}
```

<br><br>

# 강의 19 - 한글 입력 문제

- 한글이 전달되는 것을 서버에서 받지 못하는 문제
  - 브라우저에서 utf-8로 post하면 2byte가 한 문자로 여겨져서 전송됨.
  - 하지만 톰켓(웹 서버)의 기본적인 인코딩 방식은 ISO-8859-1로, 1byte를 1문자로 읽는다.
  - 따라서 읽을 때 utf-8로 읽어라고 설정해야 한다(`request.setCharacterEncoding("UTF-8");`)
  - 톰켓의 server.xml에서 인코딩 방식을 바꿀 수 있지만, 톰켓 서버에는 여러 개의 사이트가 있을 수 있다. 따라서 나만 위해서 인코딩 방식을 바꾸면 다른 데서 문제가 될 수 있어서, 기본적으로 서블릿을 통해 설정한다.

```java
@WebServlet("/notice-reg")
public class NoticeReg extends HttpServlet {
	@Override
	protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    request.setCharacterEncoding("UTF-8");
		response.setCharacterEncoding("UTF-8");
		response.setContentType("text/html; charset=UTF-8");
		PrintWriter out = response.getWriter();

		String title=request.getParameter("title");
		String content=request.getParameter("content");

		out.println(title+"<br>");
		out.println(content);
	}
}
```

<br><br>

# 강의 20 - 서블릿 필터(Servlet Filter)

- 필터
  - 요청이 들어오면 was는 서블릿을 실행 후 결과를 돌려줌. 이 때 서블릿을 실행하게 되면 메모리에 존재하게 되는데, 이 공간을 서블릿 컨테이너라고 한다.
  - 요청이 들어오면 서블릿을 실행하려 하는데, 그 중간에 가로채는 게 필터다. 가로채서 먼저 필터가 실행되고, 그 다음에 서블릿이 실행된다(모든 서블릿이 공통적으로 가져야 하는 코드). 혹은 서블릿이 실행할지 말지를 결정할 수도 있다(인증과 권한). 또한 서블릿이 실행된 후에도 필터가 실행될 수 있다.
- jakarta.servlet.Filter를 구현하는 CharacterEncodingFilter 클래스를 새로 만든다.
  - `chain.doFilter(request, response);`
    - 필터는 서블릿을 실행할지 말지를 결정할 수 있는데, 이는 FilterChain가 한다.`chain.doFilter(request, response);`의 경우 요청이 오면 그냥 흐름을 넘겨서 다음 필터 혹은 서블릿이 실행되도록 한다.
    - 이 메소드를 위아래를 기준으로 다음과 같이 실행된다. 이 메서드보다 위에 있는 코드는 요청이 올 때 바로 실행됨. 그 후 doFilter() 메소드 만나면 다음 필터나 서블릿으로 넘기고, 실행된 결과가 오면 아래에 있는 코드가 실행됨.

```java
package com.newlecture.web.filter;

import java.io.IOException;

import jakarta.servlet.Filter;
import jakarta.servlet.FilterChain;
import jakarta.servlet.ServletException;
import jakarta.servlet.ServletRequest;
import jakarta.servlet.ServletResponse;

public class CharacterEncodingFilter implements Filter {

	@Override
	public void doFilter(ServletRequest request,
			ServletResponse response,
			FilterChain chain)
			throws IOException, ServletException {
		// 위에 있는 코드
		request.setCharacterEncoding("UTF-8");
		chain.doFilter(request, response);
    // 아래에 있는 코드
	}

}
```

- web.xml에서 필터 설정을 한다(어노테이션 방식도 당연히 있음)

```xml
<filter>
  	<filter-name>characterEncodingFilter</filter-name>
  	<filter-class>com.newlecture.web.filter.CharacterEncodingFilter</filter-class>
</filter>
<filter-mapping>
  <filter-name>characterEncodingFilter</filter-name>
  <url-pattern>/*</url-pattern>
</filter-mapping>
```

- 어노테이션으로 필터 설정: `@WebFilter("/*")`는 모든 url에 대해서 필터를 적용하도록 하겠다는 뜻

```java
@WebFilter("/*")
public class CharacterEncodingFilter implements Filter {
      // 구현 코드
}
```

<br><br>

# 강의 21 - 학습과제(사용자 입력을 통한 계산 요청)

- 강의 22에 풀이 있으니 기록은 생략

<br><br>

# 강의 22 - 과제풀이(사용자 입력을 통한 계산 요청)

- 그냥 지금까지 배운 내용 요약한 느낌

- add.html

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <title>Insert title here</title>
  </head>
  <body>
    <form action="add" method="post">
      <div>
        <label>x: </label>
        <input name="x" type="text" />
      </div>
      <div>
        <label>y: </label>
        <input name="y" type="text" />
      </div>
      <div>
        <input type="submit" value="덧셈" />
      </div>
      <div>결과: 0</div>
    </form>
  </body>
</html>
```

- Add.java
  - `request.setCharacterEncoding("UTF-8");`는 필터에서 처리하고 있음

```java
package com.newlecture.web;

import jakarta.servlet.ServletException;
import jakarta.servlet.annotation.WebServlet;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;
import java.io.IOException;

@WebServlet("/add")
public class Add extends HttpServlet {

	protected void service(HttpServletRequest request,
			HttpServletResponse response) throws ServletException, IOException {
		response.setCharacterEncoding("UTF-8");
		response.setContentType("text/html; charset=UTF-8");

		String x_=request.getParameter("x");
		String y_=request.getParameter("y");

		int x=0; int y=0;

		if(!x_.equals("")) x=Integer.parseInt(x_);
		if(!y_.equals("")) y=Integer.parseInt(y_);

		int result=x+y;
		response.getWriter().printf("result is %d\n", result);
	}

}
```

<br><br>

# 강의 23 - 여러 개의 Submit 버튼 사용하기

- 덧셈 버튼뿐 아니라 뺄셈 버튼을 만들면, 이 둘을 어떻게 구분하지?

  - submit 버튼에도 name 속성을 넣으면 됨.
  - 그럼 value값은 "덧셈" 혹은 "뺄셈"이고 버튼을 누르면, http를 봤을 때 form data 안에 "x=12&y=5&operator=%EB%BA%84%EC%85%88"처럼 전송된다. getParameter("operator")해서 값("덧셈" 혹은 "뺄셈")을 받은 후 조건문 사용하면 구분해서 동작 가능

- calc.html

```html
<form action="calc" method="post">
  <div>
    <label>x: </label>
    <input name="x" type="text" />
  </div>
  <div>
    <label>y: </label>
    <input name="y" type="text" />
  </div>
  <div>
    <input type="submit" name="operator" value="덧셈" />
    <input type="submit" name="operator" value="뺄셈" />
  </div>
  <div>결과: 0</div>
</form>
```

- Calc.java

```java
@WebServlet("/calc")
public class Calc extends HttpServlet {

	protected void service(HttpServletRequest request,
			HttpServletResponse response) throws ServletException, IOException {
		response.setCharacterEncoding("UTF-8");
		response.setContentType("text/html; charset=UTF-8");

		String x_=request.getParameter("x");
		String y_=request.getParameter("y");
		String operator=request.getParameter("operator");

		int x=0; int y=0;
		if(!x_.equals("")) x=Integer.parseInt(x_);
		if(!y_.equals("")) y=Integer.parseInt(y_);

		int result=0;
		if(operator.equals("덧셈"))
			result=x+y;
		else
			result=x-y;

		response.getWriter().printf("result is %d\n", result);
	}

}
```

<br><br>

# 강의 24 - 입력 데이터 배열로 받기

- 가변적인 길이, 개수를 여러 개 보낼 때는 같은 이름으로 다 보낼 수 있다. 같은 이름으로 맞추면 배열로 전달된다.
- add2.html

```html
<form action="add2" method="post">
  <div>
    <input name="num" type="text" />
    <input name="num" type="text" />
    <input name="num" type="text" />
    <input name="num" type="text" />
  </div>
  <div>
    <input type="submit" value="덧셈" />
  </div>
  <div>결과: 0</div>
</form>
```

- `String[] num_=request.getParameterValues("num");`: 배열로 받으니까 getParameter()가 아니라 getParameterValues()로 받기
- Add2.java

```java
@WebServlet("/add2")
public class Add2 extends HttpServlet {

	protected void service(HttpServletRequest request,
			HttpServletResponse response) throws ServletException, IOException {
		response.setCharacterEncoding("UTF-8");
		response.setContentType("text/html; charset=UTF-8");

		String[] num_=request.getParameterValues("num");

		int result=0;
		for(int i=0;i<num_.length;i++) {
			int num=Integer.parseInt(num_[i]);
			result+=num;
		}

		response.getWriter().printf("result is %d\n", result);
	}

}
```

<br><br>

# 강의 25 - 상태 유지를 필요로 하는 경우와 구현의 어려움

- 사용자로부터 두 개의 값을 하나씩 개별적으로 입력받는 상황
  - 우리가 클라이언트에서 버튼을 누르게 되면, 서버 프로그램은 잠깐 실행되었다가 종료됨. 그럼 전달받은 값을 어딘가에 기록해야 한다. 그래야 다시 입력받아 서블릿이 실행되면 기록한 값을 가지고 처리할 수 있음
- 상태 유지를 위한 5가지 방법
  - application, session, cookie: 값을 담아놓을 수 있는 곳
  - hidden input, querystring: 나중에 다룰 예정
- 예제 실습은 다음 강의에서 자세히 다뤄서 생략

<br><br>

# 강의 26 - Application 객체와 그것을 사용한 상태 값 저장

- 서블릿을 사용할 때 데이터를 이어갈 수 있는 저장소가 필요한데, 이를 서블릿 컨텍스트(context)라고 한다. 즉 서블릿 간에 문맥(컨텍스트)를 이어갈 수 있는 공간(상태 저장 공간)이라는 뜻. 이곳을 어플리케이션 저장소라고 말하기도 한다.

- `ServletContext application=request.getServletContext();`: 어플리케션 저장소를 생성
- application에는 두 가지를 저장(v, operator)
  - 예시: `application.setAttribute("value", v);`
  - 키하고 값을 넣는 함수. 맵 컬렉션이라고 생각하면 됨.
- 그 다음 전달받은 연산자가 +,-,= 중에 뭔지 구분. =이 오면 계산하고, +,-가 오면 값을 저장하고.
- =의 경우 앞에서 저장한 값과 지금 읽은 값 두 가지가 필요하다.
  - 앞에서 저장한 값과 연산자는 어플리케이션 저장소에서 가져와야 한다.
  - `int x=(Integer)application.getAttribute("value");`: setAttribute()로 저장한 키워드를 사용해서 들고옴. 반환 타입은 오브젝트라서 (Integer)로 형변환함.
- 먼저 값을 3을 입력하고 +버튼 누름 -> 해당 정보가 전달되면 서블릿은 값과 연산자를 어플리케이션 저장소에 저장함 -> 이 때 저장만 하고 출력하는 게 없어서 새하얀 화면이 될 것이다. -> 그럼 뒤로가기 버튼 누르고, 이젠 값 2와 =버튼을 누른다. -> 앞에 저장한 값들을 가져오고 계산한 결과가 화면에 출력된다.

- calc2.html

```html
<form action="calc2" method="post">
  <div>
    <label>입력: </label>
    <input name="v" type="text" />
  </div>
  <div>
    <input type="submit" name="operator" value="+" />
    <input type="submit" name="operator" value="-" />
    <input type="submit" name="operator" value="=" />
  </div>
  <div>결과: 0</div>
</form>
```

- Calc2.java

```java
@WebServlet("/calc2")
public class Calc2 extends HttpServlet {

	protected void service(HttpServletRequest request,
			HttpServletResponse response) throws ServletException, IOException {
		response.setCharacterEncoding("UTF-8");
		response.setContentType("text/html; charset=UTF-8");
		ServletContext application=request.getServletContext();

		String v_=request.getParameter("v");
		String op=request.getParameter("operator");

		int v=0;
		if(!v_.equals("")) v=Integer.parseInt(v_);

		// 계산
		if(op.equals("=")) {
			int x=(Integer)application.getAttribute("value");
			int y=v;
			String operator=(String)application.getAttribute("op");

			int result=0;
			if(operator.equals("+"))
				result=x+y;
			else
				result=x-y;

			response.getWriter().printf("result is %d\n", result);
		}
		// 값을 저장
		else {
			application.setAttribute("value", v);
			application.setAttribute("op", op);
		}

	}

}
```

<br><br>

# 강의 27 - Session 객체로 상태 값 저장하기(그리고 Application 객체와의 차이점)

- 일단 강의 26에서 사용한 코드에서 Application 객체에서 Session 객체로 바꿔본 후 차이점을 살펴보도록 하자.
- 강의 26에서 사용한 Calc2.java에서 `HttpSession session= request.getSession();`로 세션을 생성한 후, application.get/setAttribute()부분에서 application을 session으로 전부 고치기만 하고 실행. 결과는 전과 똑같음.

- 차이점: Application 객체는 어플리케이션 전역에서 쓸 수 있다. 반면 Session 객체는 범주 내에서 쓸 수 있다. 이 때 세션은 현재 접속한 사용자를 뜻한다. 그럼 접속자마다 공간이 달라질 수 있다.
- 다른 브라우저를 쓰면 다른 세션으로 인식한다.
- 반면 기존에 크롬을 쓰고, 지금도 크롬을 새로 실행해서 접속하면 같은 세션으로 인식된다.
  - 왜냐면 크롬 브라우저를 여러 개 띄워도, 여러 프로세스가 동작하는 게 아니라, 하나의 프로세스에 창을 쓰레드라는 개념(프로세스의 흐름을 나눠 갖는 쓰레드라는 개념)으로 띄우게 된다.
  - 따라서 프로세스가 가지고 있는 자원을 쓰레드들은 같이 공유하기 때문에, 웹 서버에 요청할 경우 같은 프로세스가 요청한 걸로 인식됨. 그래서 같은 세션(사용자)으로 인식하게 된다.

<br><br>

# 강의 28 - WAS가 현재사용자(Session)을 구분하는 방식

- 세션 id(SID)가 was에 의해서 발부가 되고, 발급된 번호는 브라우저가 항상 가지고 다님. 그러다가 브라우저를 닫으면 사라짐.
  - was 안에 값을 두고(Application, Session) 나중에 다시 사용함.
  - 세션은 개인별 사물함(캐비닛) 느낌의 공간이라, 개인마다 번호가 있음.
  - 만약 사용자가 서버 상의 프로그램을 실행해라는 요청이 온다면, 서블릿이 실행될 것이다.
  - 이 때 요청이 처음 오는 것이라면, 새로운 사용자가 되고 이 때는 이 사용자를 위한 세션은 존재하지 않음(Application 공간의 경우는 모든 요청에 대해 저장 가능). 세션은 사용자가 세션 아이디를 가지고 있어야 세션에 값을 저장할 수 있음.
  - 요청 처리를 마친 후 새 사용자에게 아이디를 부여해 주고 브라우저는 이를 갖고 있음. 그 후 다시 브라우저가 요청하면 이 땐 아이디를 가지고 있으니 세션 사용 가능.
- 세션 키 확인
  - f12로 개발자 도구 열고, 네트워크 탭을 연다. 그 후 버튼을 누르고 http 헤더 정보를 보면, Cookie: JSESSIONID=9405B93930922EAF56AFCC941B86628C같은 항목이 있다. 다른 크롬 창을 열고 다시 반복해도 세션 아이디는 같아서 같은 세션으로 인식됨.
- 세션 객체가 갖고 있는 메서드
  - void setAttribute(String name, Object value): 지정된 이름으로 객체를 설정
  - Object getAttribute(String name): 지정한 이름의 객체를 반환
  - void invalidate(): 세션에서 사용되는 객체를 바로 해제(저장소를 비울 때)
  - void setMaxInactiveInterval(int interval)
    - 세션 타임아웃을 정수(초)로 설정
    - 사용자가 요청했지만, 그 다음에는 요청이 안 올 수 있음. 따라서 시간을 두는데, 기본 타임아웃은 30분임.
    - 만약 타임아웃된 후 전의 아이디를 갖고 있는 요청이 다시 오면, 이는 새 유저로 인식됨. 즉 해당 아이디는 유효하지 않은 아이디가 되서 해당 세션 공간을 메모리에서 수거됨.
  - boolean isNew(): 세션이 새로 생성되었는지를 확인
  - Long getCreationTime(): 세션이 시작된 시간을 반환. 1970년 1월 1일을 시작으로 하는 밀리초
  - Long getLastAccessedTime(): 마지막 요청 시간. 1970년 1월 1일을 시작으로 하는 밀리초

<br><br>

# 강의 29 - Cookie를 이용해 상태값 유지하기

- 누구나 쓸 수 있는 저장공간인 Application과, 자기만 쓸 수 있는 공간인 Session이 was 안에 있음을 배웠음.
- 꼭 서버에다가 값을 보관하지 않고, 상태값을 가지고 다닐 수 있음(브라우저가 상태값을 갖고 들고 다님).
- 클라이언트에서 저장할 수 있는 공간은 Cookie 저장소가 있음.
- 클라이언트가 값을 가지고 갈 수 있는데, 이 때 값의 종류는 세 가지가 있음

  - 브라우저가 알아서 담아주는 헤더 정보, 사용자 데이터(내가 보내는 데이터), 쿠기
  - 헤더 정보는 getHeader()로, 사용자 데이터는 getParameter()로, 쿠키는 getCookies()로 꺼내고 씀.
  - 서버가 쿠키를 만들어서 브라우저가 보관하게 하고 싶으면 addCookie()를 씀

- 쿠키는 문자열만 저장될 수 있음. 따라서 객체 생성할 때 `new Cookie("value", String.valueOf(v));`에서 String.valueOf(v)처럼 v가 정수면 이를 문자열로 바꿔서 생성해야 함.
  - jakarta.servlet.http.Cookie 사용
  - "value"는 키고, String.valueOf(v)는 값이다.
- `response.addCookie(valueCookie);`처럼 쿠키를 보낸다. 쿠키가 어떻게 전달되냐면 response header에 심어진 형태로 전달됨.
- 브라우저에서 쿠키를 읽는 걸 허용하지 않으면, 이를 통해 상태값을 유지하는 데 쓸 수 없음
- `Cookie[] cookies=request.getCookies();`: getCookies로 쿠키를 읽을 수 있다. 쿠키는 여러 개일 수 있으므로 배열로 반환됨.

- Calc2.java

```java
@WebServlet("/calc2")
public class Calc2 extends HttpServlet {

	protected void service(HttpServletRequest request,
			HttpServletResponse response) throws ServletException, IOException {
		response.setCharacterEncoding("UTF-8");
		response.setContentType("text/html; charset=UTF-8");

		ServletContext application=request.getServletContext();
		HttpSession session= request.getSession();
		Cookie[] cookies=request.getCookies();

		String v_=request.getParameter("v");
		String op=request.getParameter("operator");

		int v=0;
		if(!v_.equals("")) v=Integer.parseInt(v_);

		// 계산
		if(op.equals("=")) {
			//int x=(Integer)application.getAttribute("value");
			//int x=(Integer)session.getAttribute("value");
			int x=0;
			for(Cookie c:cookies) {
				if(c.getName().equals("value")) {
					x=Integer.parseInt(c.getValue());
					break;
				}
			}

			int y=v;
			//String operator=(String)application.getAttribute("op");
			//String operator=(String)session.getAttribute("op");
			String operator="";
			for(Cookie c:cookies) {
				if(c.getName().equals("op")) {
					operator=c.getValue();
					break;
				}
			}

			int result=0;
			if(operator.equals("+"))
				result=x+y;
			else
				result=x-y;

			response.getWriter().printf("result is %d\n", result);
		}
		// 값을 저장
		else {
			//application.setAttribute("value", v);
			//application.setAttribute("op", op);

			//session.setAttribute("value", v);
			//session.setAttribute("op", op);

			Cookie valueCookie=new Cookie("value", String.valueOf(v));
			Cookie opCookie=new Cookie("op", op);
			response.addCookie(valueCookie);
			response.addCookie(opCookie);
		}
	}
}
```

<br><br>

# 강의 30 - Cookie의 path 옵션

- 쿠키를 사용할 때 해당 url과 관련된 서블릿에게만 값이 전달될 수 있도록 하기
  - `valueCookie.setPath("/");`: 모든 페이지를 요청할 때마다 클라이언트가 valueCookie를 가져오도록 설정
  - `valueCookie.setPath("/notice/");`:notice가 포함된 하위 url을 요청할 경우에만 valueCookie를 가져오도록 설정
  - `valueCookie.setPath("/calc2");`: calc2에서만 사용되는 거라면 이렇게 설정.

```java
Cookie valueCookie=new Cookie("value", String.valueOf(v));
Cookie opCookie=new Cookie("op", op);
valueCookie.setPath("/");
opCookie.setPath("/");
response.addCookie(valueCookie);
response.addCookie(opCookie);
```

<br><br>

# 강의 31 - Cookie의 maxAge 옵션

- 기본적인 경우 브라우저가 닫히면 쿠키도 없어진다. 하지만 브라우저가 닫혀도 내가 원하는 기간을 설정하면 기간 내에 그 값을 유지할 수 있다.
- 브라우저가 쿠기를 저장하는 공간: 기본적으로 쿠키는 브라우저의 메모리에 있다가, 기간 설정이 되면 외부 파일로 저장됨.
- `valueCookie.setMaxAge(24*60*60);`: 해당 쿠키는 `24*60*60`초 후에 만료되도록 설정. 이 때 기본 단위는 초이기 때문에 60분(1시간)을 하고 싶으면 `60*60`, 24시간(하루)로 설정하고 싶으면 `24*60*60`

<br><br>

# 강의 32 - Application/Session/Cookie 정리

- 상태 저장을 위한 저장소 특징
  - application
    - 사용범위: 전역 범위에서 사용하는 저장 공간
    - 생명주기: was가 시작해서 종료할 때까지
    - 저장위치: was 서버의 메모리
  - session
    - 사용범위: 세션 범위(특정 사용자)에서 사용하는 저장 공간
    - 생명주기: 세션이 시작해서 종료할 때까지
    - 저장위치: was 서버의 메모리
  - cookie
    - 사용범위: 웹 브라우저별 지정한 path 범주(특정 url에 대해서만 사용 가능) 공간
    - 생명주기: 브라우저에 전달한 시간부터 만료시간까지
    - 저장위치: web browser의 메모리 또는 파일
  - 값을 저장하려고 하는데, 오래 저장하고 싶은 경우(예로 1년) 어디에 저장해야 하지?
    - 무조건 쿠키에 저장해야 함.
    - 세션은 왜 안됨? 기본적으로 세션 아이디는 쿠키로 전달됨. 따라서 브라우저 닫으면 쿠키 사라짐. 새로 브라우저 열면 그 사용자의 세션 아이디는 그 전과 같을까? 아니다. 또 다른 번호를 부여받고 또 다른 세션 공간을 차지하게 됨. 그럼 서버 자원 낭비.
  - 특정 url(예로 notice)와 관련된 값을 가지고 있고, notice 외에는 이를 쓰는 서블릿이 없다고 치자. 그럼 이를 application,session에 저장하는 것보다 쿠키를 쓰는게 바람직. 어쩌다 한 번 쓰는 값을 session에 넣으면 이것도 서버 자원 낭비

<br><br>

# 강의 33 - 서버에서 페이지 전환해주기(redirection)

- 페이지 전환(리다이렉트)
  - 값과 + 버튼을 눌러서 포스트하면 서블릿쪽에서 아무것도 돌려주는 것이 없어서 백지를 돌려주게 됨. 계속 계산 이어가려고 하면 뒤로가기를 눌러야 했다.
  - 이 때 서블릿 쪽에서 백지를 주지 않고 원하는 페이지로 대신 돌려줄 수 있음.
  - `response.sendRedirect("calc2.html");`:

<br><br>

# 강의 34 - 동적인 페이지(서버 페이지)의 필요성

- 동적인 문서
  - 동적인 페이지: 요청이 들어오면 데이터를 결합시켜서 만든 문서(요청이 있을 때 만들어지는 문서)를 동적인 문서라고 함. 또 서버 페이지라고 불리는 까닭도 서버에서 만들어지는 페이지라서 그럼
  - 좀 더 현실적인 계산기는 사용자가 입력한 내용을 서버에서 페이지를 만들 때 끼워 넣어야 함. 하지만 사용자가 어떤 숫자 버튼을 눌렀다면(post했다면), 서버는 그것을 쿠키든 세션이든 어딘가에 저장함. 그 후 다시 계산기 형식의 페이지를 보여주기 위해서 calc.html로 리다이렉트함.
  - 하지만 html은 정적 페이지라서 사용자가 입력한 내용을 끼워 넣을 수 없음. 따라서 동적인 페이지(서버 페이지)가 필요함. 즉 이미 만들어진 페이지를 보내는 것이 아니라, 요청이 오면 출력할 문서를 만들 때 입력한 숫자도 넣어서 출력.
  - 이를 위해 html만이 아니라 서블릿으로 된 동적인 문서를 만드는 프로그램을 만들어야 함
- 계산기 형식 html 만들기
  - 다음 강의에서 이를 서블릿으로 바꾸는 작업 할 거임
  - 일반 윈도우 계산기에서 좀 간략하게 해서 만들어서, 5행(tr) 4열(td)(결과를 표시하는 행까지 합하면 6행 4열)이다.
  - `<td colspan="4">0</td>`: 제일 위의 1행은 결과나 사용자가 입력한 문자를 표시하는 행이다. 이 때 colspan은 기본적으로 1로 설정되어 있음. 이 뜻은 열 1칸의 크기를 의미함. 이 때 4라고 설정하면 열의 크기를 4칸만큼 하겠다는 뜻
  - ÷ 만드는 법: ㄷ에 "한자"키 누르면 ÷ 나옴.

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <title>Insert title here</title>
    <style>
      input {
        width: 50px;
        height: 50px;
      }
      .output {
        height: 50px;
        background: #e9e9e9;
        font-size: 24px;
        font-weight: bold;
        text-align: right;
        /*상하는 0px, 좌우는 5px  */
        padding: 0px 5px;
      }
    </style>
  </head>
  <body>
    <form action="calc3" method="post">
      <table>
        <tr>
          <td class="output" colspan="4">0</td>
        </tr>
        <tr>
          <td><input type="submit" name="operator" value="CE" /></td>
          <td><input type="submit" name="operator" value="C" /></td>
          <td><input type="submit" name="operator" value="BS" /></td>
          <td><input type="submit" name="operator" value="÷" /></td>
        </tr>
        <tr>
          <td><input type="submit" name="value" value="7" /></td>
          <td><input type="submit" name="value" value="8" /></td>
          <td><input type="submit" name="value" value="9" /></td>
          <td><input type="submit" name="operator" value="X" /></td>
        </tr>
        <tr>
          <td><input type="submit" name="value" value="4" /></td>
          <td><input type="submit" name="value" value="5" /></td>
          <td><input type="submit" name="value" value="6" /></td>
          <td><input type="submit" name="operator" value="-" /></td>
        </tr>
        <tr>
          <td><input type="submit" name="value" value="1" /></td>
          <td><input type="submit" name="value" value="2" /></td>
          <td><input type="submit" name="value" value="3" /></td>
          <td><input type="submit" name="operator" value="+" /></td>
        </tr>
        <tr>
          <td></td>
          <td><input type="submit" name="value" value="0" /></td>
          <td><input type="submit" name="dot" value="." /></td>
          <td><input type="submit" name="operator" value="=" /></td>
        </tr>
      </table>
    </form>
  </body>
</html>
```

<br><br>

# 강의 35 - 처음이자 마지막으로 동적인 페이지 서블릿으로 직접 만들기

- `out.write()`: write()는 문자열 출력 전용함수

```java
package com.newlecture.web;

import java.io.IOException;
import java.io.PrintWriter;

import jakarta.servlet.ServletException;
import jakarta.servlet.annotation.WebServlet;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;

@WebServlet("/calcpage")
public class CalcPage extends HttpServlet {

	protected void service(HttpServletRequest request,
			HttpServletResponse response) throws ServletException, IOException {
		response.setCharacterEncoding("UTF-8");
		response.setContentType("text/html; charset=UTF-8");
		PrintWriter out=response.getWriter();


		out.write("<!DOCTYPE html>");

		out.write("<html>");
		out.write("<head>");
		out.write(" <meta charset=\"UTF-8\" />");
		out.write(" <title>Insert title here</title>");
		out.write(" <style>");
		out.write("	input{");
		out.write("		width:50px;");
		out.write("		height:50px;");
		out.write("	}");
		out.write("	.output{");
		out.write("		height:50px;");
		out.write("		background: #e9e9e9;");
		out.write("	font-size:24px;");
		out.write("	font-weight:bold;");
		out.write("	text-align:right;");
		out.write("	padding:0px 5px; ");
		out.write("}");
		out.write(" </style>");
		out.write("</head>");
		out.write("<body>");
		out.write(" <form action=\"calc3\" method=\"post\">");
		out.write("	<table>");
		out.write("	<tr>");
		out.printf("		<td class=\"output\" colspan=\"4\">%d</td>",3+4);
		out.write("	</tr>");
		out.write("	<tr>");
		out.write("		<td><input type=\"submit\"  name=\"operator\" value=\"CE\" /></td>");
		out.write("		<td><input type=\"submit\"  name=\"operator\" value=\"C\" /></td>");
		out.write("		<td><input type=\"submit\"  name=\"operator\" value=\"BS\" /></td>");
		out.write("		<td><input type=\"submit\"  name=\"operator\" value=\"÷\" /></td>");
		out.write("	</tr>");
		out.write("	<tr>");
		out.write("		<td><input type=\"submit\"  name=\"value\" value=\"7\" /></td>");
		out.write("		<td><input type=\"submit\"  name=\"value\" value=\"8\" /></td>");
		out.write("		<td><input type=\"submit\"  name=\"value\" value=\"9\" /></td>");
		out.write("		<td><input type=\"submit\"  name=\"operator\" value=\"X\" /></td>");
		out.write("	</tr>");
		out.write("	<tr>");
		out.write("		<td><input type=\"submit\"  name=\"value\" value=\"4\" /></td>");
		out.write("	<td><input type=\"submit\"  name=\"value\" value=\"5\" /></td>");
		out.write("	<td><input type=\"submit\"  name=\"value\" value=\"6\" /></td>");
		out.write("		<td><input type=\"submit\"  name=\"operator\" value=\"-\" /></td>");
		out.write("	</tr>");
		out.write("	<tr>");
		out.write("	<td><input type=\"submit\"  name=\"value\" value=\"1\" /></td>");
		out.write("	<td><input type=\"submit\"  name=\"value\" value=\"2\" /></td>");
		out.write("	<td><input type=\"submit\"  name=\"value\" value=\"3\" /></td>");
		out.write("		<td><input type=\"submit\"  name=\"operator\" value=\"+\" /></td>");
		out.write("	</tr>");
		out.write("	<tr>");
		out.write("		<td></td>");
		out.write("		<td><input type=\"submit\"  name=\"value\" value=\"0\" /></td>");
		out.write("		<td><input type=\"submit\"  name=\"dot\" value=\".\" /></td>");
		out.write("		<td><input type=\"submit\"  name=\"operator\" value=\"=\" /></td>");
		out.write("	</tr>");
		out.write("	</table>");
		out.write(" </form>");
		out.write("</body>");
		out.write("</html>");
	}

}
```

<br><br>

# 강의 36 - 계산기 서블릿 완성하기

- 목표: 버튼 눌러서 post하게 되면, 이를 누적해서 쿠키로 저장 후 리다이렉트한다. 그러면 ui에는 저장된 쿠키 정보를 출력하기
- CalcPage.java
  - exp: 이 문자열은 연산식(숫자와 연산자로 이루어진 식)을 표현. 이를 `out.printf("		<td class=\"output\" colspan=\"4\">%s</td>", exp);`게 출력
  - 기존에 value=\"÷\", value=\"X\"의 경우 실제적으로는 연산자가 `/, *`이므로 value=\"/\", `value=\"*\"`로 수정
- Calc3.java

  - `response.sendRedirect("calcpage");`: Calc3(@WebServlet("/calc3"))에서 CalcPage(@WebServlet("/calcpage"))으로 갈 때는 경로(/)가 같기 때문에 그냥 주소만(calcpage) 쓸 수 있음.
  - `String value=request.getParameter("value");`, `String operator=request.getParameter("operator");`, `String dot=request.getParameter("dot");`는 이중에 하나만 올 수 있다.
    - 만약에 숫자버튼 하나만 눌렀으면, value만 값이 오고 나머지는 다 null이다.
    - 따라서 `exp+=(value==null)?"":value;`, `exp+=(operator==null)?"":operator;`, `exp+=(dot==null)?"":dot;`가 뜻하는 것은 얻은 값이 null이 아닐 때만 붙인다는 소리다.
  - 자바 스크립트 엔진이 안되서 다음 댓글을 참고해서 진행했는데...도 안된다.

  ```
  스크립트 엔진 안되시는분들
  1. 프로젝트 우클릭 -> configure -> convert to maven project 클릭
  2. 생성된 pom.xml 파일에
    <dependencies>
      <dependency>
        <groupId>org.graalvm.js</groupId>
        <artifactId>js</artifactId>
        <version>19.2.0.1</version>
      </dependency>
      <dependency>
        <groupId>org.graalvm.js</groupId>
        <artifactId>js-scriptengine</artifactId>
        <version>19.2.0.1</version>
      </dependency>
    </dependencies>
  삽입 ( <build> 바로 위 )
  3. Calc3.java에 ScriptEngine engine = new ScriptEngineManager().getEngineByName("graal.js"); 삽입
  ```

- 코드

```java
package com.newlecture.web;

import java.io.IOException;

import javax.script.ScriptEngine;
import javax.script.ScriptEngineManager;
import javax.script.ScriptException;

import jakarta.servlet.ServletException;
import jakarta.servlet.annotation.WebServlet;
import jakarta.servlet.http.Cookie;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;

@WebServlet("/calc3")
public class Calc3 extends HttpServlet {

	protected void service(HttpServletRequest request,
			HttpServletResponse response) throws ServletException, IOException {
		Cookie[] cookies=request.getCookies();

		String value=request.getParameter("value");
		String operator=request.getParameter("operator");
		String dot=request.getParameter("dot");

		// 기존의 쿠키를 읽어와서, 그걸 가지고 덧붙이는 작업
		String exp="";
		if(cookies!=null)
			for(Cookie c:cookies) {
				if(c.getName().equals("exp")) {
					exp=c.getValue();
					break;
				}
			}

		if(operator!=null&&operator.equals("=")) {
			// 자바스크립트 엔진 에러난다.
//			ScriptEngineManager manager = new ScriptEngineManager();
//			ScriptEngine engine = manager.getEngineByName("graal.js");
//			try {
//			    exp=String.valueOf(engine.eval(exp));
//			} catch (ScriptException e) {
//			    System.err.println(e);
//			}
		} else {
			exp+=(value==null)?"":value;
			exp+=(operator==null)?"":operator;
			exp+=(dot==null)?"":dot;
		}

		Cookie expCookie=new Cookie("exp",exp);
		response.addCookie(expCookie);
		response.sendRedirect("calcpage");
		}
	}
```

<br><br>

# 강의 37 - 쿠키 삭제하기

- 계산기에서 클리어 버튼(C) 눌렀을 때 쿠키 삭제하기

  - `expCookie.setMaxAge(0);`: 클리어 버튼 누르면 쿠키의 만료시간을 0초 후로 설정하면 쿠키는 삭제됨

- Calc3.java

```java
@WebServlet("/calc3")
public class Calc3 extends HttpServlet {

	protected void service(HttpServletRequest request,
			HttpServletResponse response) throws ServletException, IOException {
		Cookie[] cookies=request.getCookies();

		String value=request.getParameter("value");
		String operator=request.getParameter("operator");
		String dot=request.getParameter("dot");

		// 기존의 쿠키를 읽어와서, 그걸 가지고 덧붙이는 작업
		String exp="";
		if(cookies!=null)
			for(Cookie c:cookies) {
				if(c.getName().equals("exp")) {
					exp=c.getValue();
					break;
				}
			}

		if(operator!=null&&operator.equals("=")) {
			// 안된다.
//			ScriptEngineManager manager = new ScriptEngineManager();
//			ScriptEngine engine = manager.getEngineByName("graal.js");
//			try {
//			    exp=String.valueOf(engine.eval(exp));
//			} catch (ScriptException e) {
//			    System.err.println(e);
//			}
		} else if(operator!=null&&operator.equals("C")) {
			exp="";
		}
		else {
			exp+=(value==null)?"":value;
			exp+=(operator==null)?"":operator;
			exp+=(dot==null)?"":dot;
		}

		Cookie expCookie=new Cookie("exp",exp);
		if(operator!=null&&operator.equals("C")) {
			expCookie.setMaxAge(0);
		}
		response.addCookie(expCookie);
		response.sendRedirect("calcpage");
		}
	}
```

<br><br>

# 강의 38 - GET과 POST에 특화된 서비스 함수

- `req.getMethod().equals("GET")`: form에 method값을 반환하는 함수가 getMethod(). 단 반환은 대문자로 하므로 주의
- 부모의 service함수(`super.service(req, res);`)

  - get요청이 오면 doGet()을, post요청일 경우 doPost()를 실행하게 되어 있음.
  - 만약 get요청이 오고 `super.service(req, res);` 실행되면 doGet()가 호출되는데, doGet()이 오버라이드되지 않았으면 오류 발생
  - 따라서 부모의 service를 오버라이드하고 조건문써서 get과 post를 다 여기서 처리할 건지, 아님 service 함수 오버라이드 하지 않고 get요청만 처리하고 싶으면 doGet()만 오버라이드해서 처리하는 식으로 할건지 고를 수 있음
  - get과 post 요청을 따로 처리할 거면 각각 doGet,doPost메소드를 오버라이드해서 사용. 혹은 get과 post 요청을 한번에 처리할 거면 service를 오버라이드해서 사용

- calculator.html

```html
<body>
  <form action="calculator" method="get">
    <input type="submit" value="요청" />
  </form>
</body>
```

- service를 오버라이드하고 조건문써서 get과 post를 다 여기서 처리할 건지

```java
@WebServlet("/calculator")
public class Calculator extends HttpServlet{
	@Override
	protected void service(HttpServletRequest req, HttpServletResponse res) throws ServletException, IOException {

		if(req.getMethod().equals("GET")) {
			System.out.println("GET 요청이 왔습니다");
		}else if(req.getMethod().equals("POST")) {
			System.out.println("POST 요청이 왔습니다");
		}
	}
}
```

- service 함수 오버라이드 하지 않고 get요청만 처리하고 싶으면 doGet()만 오버라이드해서 처리하는 식으로 할건지

```java
@WebServlet("/calculator")
public class Calculator extends HttpServlet{

	@Override
	protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		System.out.println("doGet 메소드가 호출되었습니다");
	}

	@Override
	protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		System.out.println("doPost 메소드가 호출되었습니다");
	}
}
```

<br><br>

# 강의 39 - 계산기 프로그램 하나의 서블릿으로 합치기

- 전에 만들었던 계산기 서블릿을 보면, /calcpage로 get요청만 하고 있고, /calc3로 post요청만 하고 있었다.
  - 그럼 만약 쿠키.setPath()해서 특정 url에서만 쿠키를 사용하도록 하고 싶다고 치자. 이 때 이 setPath()는 하나의 경로만 지정 가능하다. 또 아무것도 지정하지 않으면 루트경로가 된다.
  - 루트경로가 되면 쿠키를 사용할 필요가 없는 곳에서도 쿠키를 넣게 된다. 이는 싫으니까 경로를 설정하려고 보니 /calcpage로 설정하면 /calc3는 쿠키를 못 받으니까 문제다.
  - 따라서 url을 하나로 합치고 싶다.
- /calculator이라는 url로 합치자. 이 때 전에 만든 calc3.java는 doPost() 안에, calcpage.java는 doGet() 안에 넣고 약간만 수정하자

  - `<form action="calc3" method="post">"`에서 `action="calc3"` 삭제
    - 아래 코드처럼 합치면 action을 쓸 필요가 없다. 왜냐면 기존에는 get요청하는 페이지와 post를 담당하는 url이 달라서 "calc3"라고 지정한 거다.
    - 현재 페이지를 post요청하니까 안 넣어도 됨
  - `response.sendRedirect("calculator");`: 자기가 자기를 요청하는데, 이는 get요청을 의미함
  - `expCookie.setPath("/calculator");`: 자기자신(calculator)에서만 해당 쿠키를 사용할 수 있음

- 기존의 코드와 똑같은 건 주석 처리함.

```java
package com.newlecture.web;

import java.io.IOException;
import java.io.PrintWriter;

import jakarta.servlet.ServletException;
import jakarta.servlet.annotation.WebServlet;
import jakarta.servlet.http.Cookie;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;

@WebServlet("/calculator")
public class Calculator extends HttpServlet{

	@Override
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// Cookie[] cookies=request.getCookies();

		// String exp="0";
		// if(cookies!=null)
		// 	for(Cookie c:cookies) {
		// 		if(c.getName().equals("exp")) {
		// 			exp=c.getValue();
		// 			break;
		// 		}
		// 	}

		// response.setCharacterEncoding("UTF-8");
		// response.setContentType("text/html; charset=UTF-8");
		// PrintWriter out=response.getWriter();

		// out.write("<!DOCTYPE html>");

		// out.write("<html>");
		// out.write("<head>");
		// out.write(" <meta charset=\"UTF-8\" />");
		// out.write(" <title>Insert title here</title>");
		// out.write(" <style>");
		// out.write("	input{");
		// out.write("		width:50px;");
		// out.write("		height:50px;");
		// out.write("	}");
		// out.write("	.output{");
		// out.write("		height:50px;");
		// out.write("		background: #e9e9e9;");
		// out.write("	font-size:24px;");
		// out.write("	font-weight:bold;");
		// out.write("	text-align:right;");
		// out.write("	padding:0px 5px; ");
		// out.write("}");
		// out.write(" </style>");
		// out.write("</head>");
		// out.write("<body>");
		out.write(" <form method=\"post\">");
		// out.write("	<table>");
		// out.write("	<tr>");
		// out.printf("		<td class=\"output\" colspan=\"4\">%s</td>", exp);
		// out.write("	</tr>");
		// out.write("	<tr>");
		// out.write("		<td><input type=\"submit\"  name=\"operator\" value=\"CE\" /></td>");
		// out.write("		<td><input type=\"submit\"  name=\"operator\" value=\"C\" /></td>");
		// out.write("		<td><input type=\"submit\"  name=\"operator\" value=\"BS\" /></td>");
		// out.write("		<td><input type=\"submit\"  name=\"operator\" value=\"/\" /></td>");
		// out.write("	</tr>");
		// out.write("	<tr>");
		// out.write("		<td><input type=\"submit\"  name=\"value\" value=\"7\" /></td>");
		// out.write("		<td><input type=\"submit\"  name=\"value\" value=\"8\" /></td>");
		// out.write("		<td><input type=\"submit\"  name=\"value\" value=\"9\" /></td>");
		// out.write("		<td><input type=\"submit\"  name=\"operator\" value=\"*\" /></td>");
		// out.write("	</tr>");
		// out.write("	<tr>");
		// out.write("		<td><input type=\"submit\"  name=\"value\" value=\"4\" /></td>");
		// out.write("	<td><input type=\"submit\"  name=\"value\" value=\"5\" /></td>");
		// out.write("	<td><input type=\"submit\"  name=\"value\" value=\"6\" /></td>");
		// out.write("		<td><input type=\"submit\"  name=\"operator\" value=\"-\" /></td>");
		// out.write("	</tr>");
		// out.write("	<tr>");
		// out.write("	<td><input type=\"submit\"  name=\"value\" value=\"1\" /></td>");
		// out.write("	<td><input type=\"submit\"  name=\"value\" value=\"2\" /></td>");
		// out.write("	<td><input type=\"submit\"  name=\"value\" value=\"3\" /></td>");
		// out.write("		<td><input type=\"submit\"  name=\"operator\" value=\"+\" /></td>");
		// out.write("	</tr>");
		// out.write("	<tr>");
		// out.write("		<td></td>");
		// out.write("		<td><input type=\"submit\"  name=\"value\" value=\"0\" /></td>");
		// out.write("		<td><input type=\"submit\"  name=\"dot\" value=\".\" /></td>");
		// out.write("		<td><input type=\"submit\"  name=\"operator\" value=\"=\" /></td>");
		// out.write("	</tr>");
		// out.write("	</table>");
		// out.write(" </form>");
		// out.write("</body>");
		// out.write("</html>");
	}

	@Override
	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// Cookie[] cookies=request.getCookies();

		// String value=request.getParameter("value");
		// String operator=request.getParameter("operator");
		// String dot=request.getParameter("dot");

		// 기존의 쿠키를 읽어와서, 그걸 가지고 덧붙이는 작업
		// String exp="";
		// if(cookies!=null)
		// 	for(Cookie c:cookies) {
		// 		if(c.getName().equals("exp")) {
		// 			exp=c.getValue();
		// 			break;
		// 		}
		// 	}

		// if(operator!=null&&operator.equals("=")) {
		// 	안된다.
		// 	ScriptEngineManager manager = new ScriptEngineManager();
		// 	ScriptEngine engine = manager.getEngineByName("graal.js");
		// 	try {
		// 	    exp=String.valueOf(engine.eval(exp));
		// 	} catch (ScriptException e) {
		// 	    System.err.println(e);
		// 	}
		// } else if(operator!=null&&operator.equals("C")) {
		// 	exp="";
		// }
		// else {
		// 	exp+=(value==null)?"":value;
		// 	exp+=(operator==null)?"":operator;
		// 	exp+=(dot==null)?"":dot;
		// }

		// Cookie expCookie=new Cookie("exp",exp);
		// if(operator!=null&&operator.equals("C")) {
		// 	expCookie.setMaxAge(0);
		// }

		expCookie.setPath("/calculator");
		// response.addCookie(expCookie);
		response.sendRedirect("calculator");
	}
}
```

<br><br>

# 강의 40 - JSP 시작하기 (Jasper를 이용한 서블릿 프로그래밍)

- Jasper가 만들어주는 서블릿 출력 코드
  - html 코드에 out.write()를 일일히 다 붙여서 서블릿 코드로 바꾼 다음 출력했는데, 이를 Jasper가 대신 해줌.
  - Jasper에게 일을 시키려면 확장자만 .html에서 .jsp로 바꾸면 된다.
- Jasper를 이용한 코드 작성 방법
  - 기본적으로 Jasper에 적혀 있는 모든 내용은 다 출력해야 하는 걸로 알고 다 write() 안에 넣어버림
  - 하지만 이건 출력할 게 아니고 자바 코드를 넣기 위한 용도로 "<% 안에 자바 코드 %>"(코드블록)를 이용할 수 있다.
  - 만약 add.jsp에 코드를 쓰면 이는 Jasper가 add_jsp.java파일로 변환한다. 코드 블록을 쓰면 write() 안에 넣지 않고 쓴 그대로 넣는다.

<br><br>

# 강의 41 - JSP의 코드 블록

- 코드 블록: 예시로 add.jsp파일에서 다음처럼 코드 블록 활용하면, Jasper가 add_jsp.java파일로 변환할 때 어떻게 할까?
  - `<% y=x+3; %>`: 그대로 `y=x+3;`를 add_jsp.java 속 \_jspService함수에 넣음
  - `y의 값은: <% out.print(y)%>`: `out.write("y의 값은: "); out.print(y);`가 add_jsp.java 속 \_jspService함수에 심어짐
  - `y의 값은: <%=y%>`: `y의 값은: <% out.print(y)%>`와 같이 쓰기 불편하니 간단히 한 버전. 결과는 똑같이`out.write("y의 값은: "); out.print(y);`가 add_jsp.java 속 \_jspService함수에 심어짐
  - `<%! public int sum(int a,int b) {return a+b;} %>`: 일반적인 코드블록(<%%>)안에 느낌표를 앞에 하나 추가하면 이 코드는 add_jsp.java 속 \_jspService함수가 아니라, add_jsp 클래스의 멤버를 선언하는 곳에 심어짐
- 페이지 지시자
  - 페이지를 어떤 인코딩 방식을 이용할 것인지, 콘텐츠 타입은 무엇인지 지시하는 것
  - 이는 지시 블록(<%@ %>)를 이용한다.

<br><br>

# 강의 42 - JSP의 내장객체 간단히 알아보기

- JSP의 내장객체
  - Jasper가 만들어 놓은 객체를 가리키는 변수들을 내장 객체라고 한다.
  - 우리는 이런 변수가 있음을 알고 있어야 하고, 이를 적절히 활용해서 만들 수도 있어야 한다.
  - request, response: 입력, 출력 도구
  - applicaton, session 객체
  - pageContext
    - applicaton, session처럼 페이지 내에서 임시로 데이터를 저장할 수 있는(setAttribute, getAttribute 갖고 있는) 변수
    - ServletContext는 전역에서 사용하는 거라면, PageContext는 파일 내부에서만 쓰는 저장소라고 보면 됨
  - config(ServletConfig 변수), out(출력도구), page(이 페이지의 객체를 참조하는 Object 변수)

<br><br>

# 강의 43 - JSP로 만드는 Hello 서블릿

```jsp
{%raw%}
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%
int cnt=10; // 기본값

String cnt_=request.getParameter("cnt");
if(cnt_!=null&&!cnt_.equals(""))
	cnt=Integer.parseInt(cnt_);
%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
		<%for(int i=0;i<cnt;i++) {%>
			안녕, servlet<br>
		<%} %>
</body>
</html>
{%endraw%}
```

<br><br>

# 강의 44 - 스파게티 코드를 만드는 JSP

- 스파게티 코드: 코드 블록을 나눠서 만들게 되면, 코드 수정할 때 자바 코드만 보기가 힘듦. 내가 관심 있는 코드와 없는 코드가 섞여 있는 코드
- 한국말로는 실타래 코드라고 한다. 한 번 꼬이면 답이 없는 코드

- 스파게티 코드 예제(spag.jsp)

```jsp
{%raw%}
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%
	int num=0;
%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<%
String num_=request.getParameter("n");
if(num_!=null&&!num_.equals(""))
num=Integer.parseInt(num_);
%>
<body>
	<%if(num%2!=0) {%>
	홀수입니다.
	<%}
	else
	{%>
	짝수입니다.
	<%} %>
</body>
</html>
{%endraw%}
```

<br><br>

# 강의 45 - JSP MVC model1

- jsp 코드 블록을 어떻게 간단하게 만들 수 있을까?(스파게티 코드를 어떻게 피할까?)
- 이에 대해 나온 해결책이 MVC model1
  - 코드 블록 최소화: 입력 코드는 전부 위쪽에 놓고, 출력 코드는 전부 아래쪽에 놓기
  - 출력을 위해서 만드는 데이터(출력할 데이터)를 모델이라고 함.
  - 이러한 모델은 입력 코드부분에서 만들어서, 출력 코드 부분에 꽂으면 됨.
- 정리하면

  - Controller: 출력할 데이터(Model)를 만들어 내는 부분(입력과 제어를 담당하는 자바 코드)
  - View: 출력 담당(html 코드)
  - Model: 출력 데이터

- 스파게티 코드 예제(spag.jsp)에 mvc패턴을 적용 후

```jsp
{%raw%}
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%
	int num=0;
	String num_=request.getParameter("n");
	if(num_!=null&&!num_.equals(""))
	num=Integer.parseInt(num_);

	String result;
	if(num%2!=0)
		result="홀수";
	else
		result="짝수";
%>

<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<%=result %>입니다.
</body>
</html>
{%endraw%}
```

<br><br>

# 강의 46 - JSP MVC model1을 model2 방식으로

- mvc model1은 컨트롤러와 뷰가 물리적으로 구분되지 않았다(한 파일 내에 자바 코드(컨트롤러)와 뷰(html 코드)가 같이 있다)
- mvw model2는 컨트롤러와 뷰가 물리적으로 구분한 방식(자바 코드와 html 코드를 따로 분리)
- model2: dispatcher를 집중화하기 전의 모델
  - 컨트롤러와 뷰를 연결하기 위해서 포워딩하는 방법이 포함됨. 즉 컨트롤러에서 뷰로(서블릿에서 jsp(서블릿)로) 흐름을 이어받아서 코드를 진행하는 게 **포워딩**
  - 이 때 포워딩은 dispatcher로 하게 됨
  - 계속 페이지 만들다보면 컨트롤러에서 공통적으로 가지고 있는 dispatcher 기능이 비효율적
- model2: dispatcher를 집중화하기 전의 모델
  - dispatcher는 하나만 두고 컨트롤러의 기능만 따로 담는 방법
  - 즉 실질적으로 서블릿은 하나만 만들고, 업무 로직(컨트롤러)는 따로 POJO클래스라고 해서, 서블릿 클래스가 아닌 일반 클래스 형태로 만듦
  - 사용자 요청이 들어오면 dispatcher가 적절한 컨트롤러 찾아서 수행하게 함.
- forward
  - redirect와 forward
    - forward는 현재 작업하던 내용을 이어갈 수 있도록 할 때 씀
    - redirect는 현재 작업하던 내용과 전혀 관계없이 새로 요청할 때 씀
- 실습

  - `request.getRequestDispatcher("spag.jsp");`
    - 현재 spag라는 url이 있는데, 요청이 들어왔을 경우, spag.jsp로 요청을 전달하려고 함
    - 페이지명(spag.jsp)만 넣었지 경로를 넣지 않은 이유는, url 상으로 같은 디렉토리에 있다고 생각하기 때문에 지정하지 않음.
  - `dispatcher.forward(request, response);`
    - 위에서 얻은 dispatcher로 포워딩할 수 있음.
    - 즉 현재 작업했던 내용들을 request, response에 담고 있다면, 그 내용이 spag.jsp로 이어져서 진행될 것이다.
  - `request.setAttribute("result", result);`
    - 우리가 result에 출력할 결과를 담았고, 이를 spag.jsp로 넘겨줄 때 쓸 수 있는 저장소가 request다.
    - 즉 포워드 관계에 있는 둘 사이에 공유하는 저장소는 request가 사용됨

- spag.java(컨트롤러)

```java
package com.newlecture.web;

import java.io.IOException;

import jakarta.servlet.RequestDispatcher;
import jakarta.servlet.ServletException;
import jakarta.servlet.annotation.WebServlet;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;

@WebServlet("/spag")
public class spag extends HttpServlet{
	@Override
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		int num=0;
		String num_=request.getParameter("n");
		if(num_!=null&&!num_.equals(""))
		num=Integer.parseInt(num_);

		String result;
		if(num%2!=0)
			result="홀수";
		else
			result="짝수";

		request.setAttribute("result", result);
		RequestDispatcher dispatcher
			=request.getRequestDispatcher("spag.jsp");
		dispatcher.forward(request, response);
	}
}
```

- spag.jsp(뷰)

```jsp
{%raw%}
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>

<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<%=request.getAttribute("result")%>입니다.
</body>
</html>
{%endraw%}
```

<br><br>

# 강의 47 - EL(Expression Language)

- EL: 객체에서 값을 추출해서 출력하는 표현식
  - 코드 블록을 쓰지 않고 값을 꺼내 쓸 수 있다.
- `${result}`
  - `request.setAttribute("result", result);` -> `<%=request.getAttribute("result")%>입니다.`
  - 이 때 `<%=request.getAttribute("result")%>` 대신 `${result}입니다`로 간단히 쓸 수 있음
- `${list[0]}`
  - `List list=new ArrayList(){"1", "test"};`와 `request.setAttribute("list", list);` -> `((List)request.getAttribute("list")).get(0)`
  - 이 때 `((List)request.getAttribute("ist")).get(0)`대신 `${list[0]}`으로 쓸 수 있음
- `${n.title}`
  - `Map n=new HashMap("title","제목");`와 `request.setAttribute("n", n);` -> `((Map)request.getAttribute("n")).get("title")`
  - 이 때 `((Map)request.getAttribute("n")).get("title")` 대신 `${n.title}`로 쓸 수 있음

<br><br>

# 강의 48 - EL의 데이터 저장소

- page객체에 값을 저장하기
  - `<%pageContext.setAttribute("test","content")%>` 코드 블록을 jsp에 넣은 후 해당 jsp 파일 내에서 `${test}`라고 입력하면 저장된 값을 쓸 수 있음.
- EL이 저장 객체에서 값을 추출하는 순서
  - EL을 이용해서 뽑아낼 수 있는 것은 page 객체, request 객체, session 객체, application 객체 등 어디서든 뽑을 수 있다.
  - 값을 추출하는 우선순위는 page->request->session->application 순이다.
- 그럼 나는 session의 cnt를 바라고 ${cnt}를 썼는데, page에도 cnt가 저장되어 있다면, 어떻게 session의 cnt를 쓸 수 있을까?
  - 우선순위는 page가 더 높으므로 cnt는 page의 cnt가 출력된다.
  - 따라서 이와 비슷한 경우에는 다음의 키워드를 쓰면 된다.
    - ${pageScope.cnt}
    - ${requestScope.cnt}
    - ${sessionScope.cnt}
    - ${applicationScope.cnt}
- 클라이언트의 입력 값 추출
  - ${param.cnt}
    - param은 파라미터 값을 저장하고 있는 저장소
    - 파라미터란 "localhost:8080/spag?n=3"에서 n을 의미.
  - ${header.host}
    - header는 header 정보를 저장하고 있는 저장소
  - `${header.["host"]}`
    - 쓰려는 이름이 변수 명명 규칙에 맞지 않을 때(예로 대쉬가 들어갈 때), 대괄호와 따옴표로 감싸서 쓸 수 있음.
- pageContext 객체
  - pageContext: 페이지 범위의 컨텍스트 저장소
  - ${pageContext.request.method}: <%=pageContext.getRequest().getMethod() %>를 EL로 표현한 것

<br><br>

# 강의 49 - EL 연산자

- `empty`
  - `${empty param.n}`: "localhost:8080/spag?n=3"처럼 n에 값이 오지 않으면 빈 문자열("spag?n="일 때) 혹은 null("spag"일 때)이 되는데, 이 경우 empty 구문은 참이 된다.
- `+, -, *, (/와 div), (%와 mod)`
  - `${3/2}`는 1.5로 출력됨(일반적인 것처럼 1로 출력되지 않음)
- `(<와 lt),(>와 gt),(<=와 le),(>=와 ge)`
  - html 자체가 꺾음새를 가지고 있는데, html의 엄격한 구문으로 파악하면 오류를 유발할 수도 있음. 따라서 lt(less than), le(less and equal) 등의 키워드도 사용할 수 있음
- `(==와 eq),(!=와 ne)`
  - ne(not equal)
- `(&&와 and),(||와 or)`
- `?:`
  - `${empty param.n? '값이 비어있습니다': param.n}`: 삼항연산자

<br><br>

# 강의 50 - 수업용 프로젝트 준비하기

- 별달리 메모할 내용은 없었다.

<br><br>

# 강의 51 - JSP를 이용해서 자바 웹 프로그램 만들기 시작

- JSP 프로그래밍: Jasper가 작성할 서블릿 코드에 코드 블록을 적절히 끼워 달라고 지시하는 방식
- list.html을 list.jsp로 복사하면, 한글이 다 깨져있음. 기본적으로 복사한 파일에 대한 인코딩 방식이 ISO-8859-1이라서 그럼. 이 땐 alt+enter키 누른 후에 파일 인코딩 방식을 utf-8로 설정
- list.jsp를 실행하면 한글이 다 깨져 있음.
  - 지시 블록 사용: `<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>`
- list.jsp에서 공지사항 목록 부분을 약간 손 봄

```jsp
{%raw%}
<%for(int i=0;i<10;i++) {%>
	<tr>
		<td><%=i+1 %></td>
		<td class="title indent text-align-left"><a href="detail.html">스프링 8강까지의 예제 코드</a></td>
		<td>newlec</td>
		<td>
			2019-08-18
		</td>
		<td>146</td>
	</tr>
<%} %>
{%endraw%}
```

<br><br>

# 강의 52 - JDBC를 이용해 글 목록 구현하기

- 오라클 드라이버 가져오기

  - 웹 개발할 때는 웹에서 사용하는 라이브러리가 어딘가에 배포돼서 실행되기 때문에 build path로 경로 지정해서 사용하면 안 되고, 라이브러리를 프로젝트에 포함시켜야 한다.
  - WEB-INF 속 lib폴더가 있음. 여기에 우리가 사용하는 파일을 담아야 한다.

- 해당 코드 내용은 jdbc 강의 ex1의 program.java파일에서 복붙한 내용도 포함돼 있다.
- list.jsp

```jsp
{%raw%}
<%
String url = "jdbc:oracle:thin:@localhost:1521/xepdb1";
String sql = "select * from notice";

Class.forName("oracle.jdbc.driver.OracleDriver");
Connection con = DriverManager.getConnection(url, "newlec", "1234");
Statement st = con.createStatement();
ResultSet rs = st.executeQuery(sql);

%>

<!-- 중략 -->

<%while(rs.next()) {%>
	<tr>
		<td><%=rs.getInt("id") %></td>
		<td class="title indent text-align-left"><a href="detail.html"><%=rs.getString("title") %></a></td>
		<td><%=rs.getString("writer_id") %></td>
		<td>
			<%=rs.getDate("regdate") %>
		</td>
		<td><%=rs.getInt("hit") %></td>
	</tr>
<%} %>

<!-- 중략 -->

<%
	rs.close();
	st.close();
	con.close();
%>
{%endraw%}
```

<br><br>

# 강의 53 - 자세한 페이지 구현하기

- `<td class="title indent text-align-left"><a href="detail.jsp?id=<%=rs.getInt("id") %>"><%=rs.getString("title") %></a></td>`
  - 전 강의의 list.jsp의 일부분을 다음처럼 수정했다. 목록 페이지에서 게시글을 누르면 detail.jsp?id=<%=rs.getInt("id") %>로 이동하는 것이다.
  - 즉 id에 해당하는 게시글로 이동 가능하게 하기 위해 수정했다.
- detail.jsp를 구현하기
  - list.jsp와 차이나는 부분만 기록하겠다. 전의 list.jsp에서 추가한 내용 중 일부를 복붙한 후 조금만 수정했기 때문이다.

```jsp
{%raw%}
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
%>

<!-- 중략 -->

<div class="margin-top first">
	<h3 class="hidden">공지사항 내용</h3>
	<table class="table">
		<tbody>
			<tr>
				<th>제목</th>
				<td class="text-align-left text-indent text-strong text-orange" colspan="3"><%=rs.getString("title") %></td>
			</tr>
			<tr>
				<th>작성일</th>
				<td class="text-align-left text-indent" colspan="3"><%=rs.getDate("regdate") %>	</td>
			</tr>
			<tr>
				<th>작성자</th>
				<td><%=rs.getString("writer_id") %></td>
				<th>조회수</th>
				<td><%=rs.getString("hit") %></td>
			</tr>
			<tr>
				<th>첨부파일</th>
				<td colspan="3"><%=rs.getString("files") %></td>
			</tr>
			<tr class="content">
				<td colspan="4"><%=rs.getString("content") %></td>
			</tr>
		</tbody>
	</table>
	</div>

<div class="margin-top text-align-center">
	<a class="btn btn-list" href="list.jsp">목록</a>
</div>
{%endraw%}
```

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
