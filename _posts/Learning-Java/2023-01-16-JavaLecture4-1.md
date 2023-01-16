---
title: "[서블릿/JSP(뉴렉처)] "
excerpt: ""

categories:
  - Learning-Java
tags:
  - [Servlet, JSP]

toc: true
toc_sticky: true
toc_label: "페이지 주요 목차"

date: 2023-01-16
last_modified_at: 2023-01-16
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

- 난 vsCode로 쭉 할 거기 때문에 강의 내용과 별개로 몇개 기록하겠다.
- 톰켓 서버를 vsCode에서 사용하기 위한 익스텐션은 Community Server Connectors다. 몇년 전 블로그를 보면 Tomcat for Java 익스텐션을 다운받아서 사용하였다. 하지만 이는 deprecated 상태라 다운 받을 수 없게 되었다.
- 익스텐션 다운 후 explorer 탭 안에서 java projects 탭 밑에 servers 탭이 생긴 것을 알 수 있다. 그럼 거기서 새 서버를 추가할 수 있다.
- [참고 사이트](https://code.visualstudio.com/docs/java/java-tomcat-jetty)니까 다시 볼 필요 있을 때 참고할 것
