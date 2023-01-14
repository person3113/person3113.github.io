---
title: "[자바 JDBC(뉴렉처)] 25강도 있음!25강도 있음!25강도 있음!25강도 있음!25강도 있음!25강도 있음!25강도 있음!25강도 있음!25강도 있음!25강도 있음!25강도 있음!25강도 있음!25강도 있음!25강도 있음!25강도 있음!25강도 있음!25강도 있음!25강도 있음!25강도 있음!25강도 있음!25강도 있음!25강도 있음!25강도 있음!25강도 있음!25강도 있음!25강도 있음!25강도 있음!"
excerpt: ""

categories:
  - Learning-Java
tags:
  - [JDBC]

toc: true
toc_sticky: true
toc_label: "페이지 주요 목차"

date: 2023-01-14
last_modified_at: 2023-01-14
---

<br>

# [유튜브 강의 주소](https://www.youtube.com/watch?v=c0s7g7iVtwc&list=PLq8wAnVUcTFWxwoc41CqmwnO-ZyRDL0og)

<br>

# 보는 방법

- 강의가 화면 자료에 내용 설명이 적고, 적당히 설명해서 구체적으로 메모 힘들다. 따라서 키워드 위주나 내용 간단 요약 느낌으로 기록했다.
- 강의 내용 복습은 메모한 것뿐만 아니라 강의 영상도 봐라. 이는 상기용/테스트용 정도로만 활용해라.
- 여기서는 다음을 기록.
  - 강의 목차와 키워드나 내용 요약
  - (가끔씩) 강의를 보고 생긴 의문에 대해 구글링해서 찾은 내용

<br><br>

# 강의 01 - JDBC란 무엇인가?

- SQL을 작성할 수 있는 사람들은 DB client 프로그램을 통해 쿼리를 실행
- SQL을 작성할 수 없는 사람들은 업무용 프로그램(UI)을 통해 쿼리를 실행
- DBC(database connectivity): DB API(쉽게 생각해서 함수 이름)은 DBMS마다 차이난다. 따라서 oracle DB API쓰다가 mysql로 바꾸면 제대로 동작 안 할 것임.
- JDBC: 앱 만드는 사용자가 각 dbms api 직접 사용 x. 차이나는 부분을 단일화해서 자바가 제공. 어떤 dbms를 사용하든 단일한 api(함수 이름) 제공. 사용자는 각 dbms api를 신경쓸 필요 x.
- JDBC driver: jdbc는 깡통. 각 dbms를 실제로 구동시키는 드라이버가 필요. 이 드라이버를 JDBC로 다루는 것.

<br><br>

# 강의 02 - DBMS와 JDBC Driver 준비하기

- 오라클 드라이버 홈페이지에서 다운받고 프로젝트에 적용하기

<br><br>

# 강의 03 - JDBC 기본 코드의 이해

- Class.forName("oracle.jdbc.driver.OracleDriver")
  - 코드 실행 후 메모리에 드라이버 올라감(드라이버 객체 생성됨)
- Connection con = DriverManger.getConnection(...)
  - 코드 실행 후 연결이 되고, 연결이 성공적으로 되면 Connection 객체 반환됨
- Statement st = con.createStatement()
  - 사용자로부터 요구받은 쿼리를 이 녀석을 통해 실행하게 될 것임
- ResultSet rs = st.executeQuery(sql)
  - 코드 실행해서 쿼리 실행하면, DB 서버에서 결과집합이 만들어지고, 레코드를 가리키는 커서(포인터)가 생성되고, 행 하나를 받을 수 있는 빈 그릿이 만들어짐.
  - 커서와 관련해서 BOF(before of file), EOF
- rs.next()
  - 클라이언트가 레코드 하나 받음(레코드 하나를 받는 그릇이 ResultSet)
- String title = rs.getString("title")
  - title 컬럼에 해당하는 것을 뽑아옴

<br><br>

# 강의 04 - 쿼리 실행하기 실습

- `Connection con = DriverManager.getConnection(url, "newlec", "1234");`: 사용자 이름은 대소문자 가리지 않는 것 같음.
- `String title = rs.getString("title");`: 컬럼 이름도 대소문자 가리지 않는 것 같음.

```java
package ex1;

import java.sql.*;

public class program {
    public static void main(String[] args) throws Exception {
        String url = "jdbc:oracle:thin:@localhost:1521/xepdb1";
        String sql = "select * from notice";

        Class.forName("oracle.jdbc.driver.OracleDriver");
        Connection con = DriverManager.getConnection(url, "newlec", "1234");
        Statement st = con.createStatement();
        ResultSet rs = st.executeQuery(sql);

        if (rs.next()) {
            String title = rs.getString("title");
            System.out.println(title);
        }

        rs.close();
        st.close();
        con.close();
    }
}

```

<br><br>

# 강의 06 - 문제 #1 풀이겸 문제 #2

- 문제 1: 레코드의 모든 컬럼 출력하기

  - `Date regDate`에서 Date는 java.util.Date이고, sql.Date가 아니다.

  ```java
  while (rs.next()) {
      int id = rs.getInt("id");
      String title = rs.getString("title");
      String writerId = rs.getString("writer_id");
      Date regDate = rs.getDate("regdate");
      String content = rs.getString("content");
      int hit = rs.getInt("hit");

      System.out.printf("%d,%s,%s,%s,%s,%d\n", id, title, writerId, regDate, content, hit);
  }
  ```

<br><br>

# 강의 07 - 문제 #2 풀이와 SQL을 잘해야 하는 이유

- 문제 2: 조회수가 10이 넘는 게시글만 출력하시오

  - 데이터 필터링, 정렬, 그룹화 등의 모든 데이터 가공/연산은 데이터베이스에서 처리. 자바는 UI 레이아웃만
    - print 구문을 if(hit>10)로 감싸면 되지 않나? 아니다.
    - 만약 데이터가 1억개가 있는데 조건 만족하는 조회수는 2개밖에 없으면 while문으로 고작 두개 출력하려고 1억번 돌리나?

  ```java
  String sql = "select * from notice where hit>10"

  while (rs.next()) {
      int id = rs.getInt("id");
      String title = rs.getString("title");
      String writerId = rs.getString("writer_id");
      Date regDate = rs.getDate("regdate");
      String content = rs.getString("content");
      int hit = rs.getInt("hit");

      System.out.printf("%d,%s,%s,%s,%s,%d\n", id, title, writerId, regDate, content, hit);
  }
  ```

<br><br>

# 강의 08 - 데이터 입력을 위한 쿼리문 준비하기

