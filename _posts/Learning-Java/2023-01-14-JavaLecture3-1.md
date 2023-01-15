---
title: "[자바 JDBC(뉴렉처)] 1 ~ 24강"
excerpt: "jdbc 기초 강의 내용 기록"

categories:
  - Learning-Java
tags:
  - [JDBC]

toc: true
toc_sticky: true
toc_label: "페이지 주요 목차"

date: 2023-01-14
last_modified_at: 2023-01-15
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

- 강의 09에서 쓸 sql문 만들고 끝

<br><br>

# 강의 09 - 데이터 입력하기와 PreparedStatement

- PreparedStatement
  - `Statement st = con.createStatement();`의 Statement기능을 다 가지고 있음. 거기다 PreparedStatement는 쿼리문을 가지고 있고, sql문에 ? 안에 값을 채워넣을 수 있다.
  - `st.setString(1, title);`: title은 문자열이니 setString을 선택. 첫 번째 인자에는 몇 번째 ?를 채울 건지 index를 집어넣음. 이 때 시작 번호는 일반적인 인덱스와 다르게 1부터 시작.
  - `int result = st.executeUpdate();`
    - statement에서 executeQuery()는 select할 때 써서 결과집합을 얻고, 이를 ResultSet 객체에 받음.
    - 반면 executeUpdate()는 insert, update, delete할 때 사용. 이 함수는 몇 개의 열이 영향을 받았는지를 반환함. 여기서는 행을 하나 추가했으므로 1이 반환될 것임.
    - 여기서 st는 PreparedStatement 객체다. 따라서 일반 Statement와 다르게 st.executeQuery()나 st.executeUpdate()할 때 매개변수로 sql를 넣으면 안 된다. 이미 PreparedStatement가 sql를 지니고 있기 때문이다.
- sql 변수에 sql 코드를 + 연산자로 붙여서 저장할 때 실수하기 좋으니까 주의(예로 + 연산 후 띄어쓰기가 없어서 에러. 따라서 + 연산자가 수행된 후에 sql문이 어떤 모습일 지 상상해라.

```java
package ex1;

import java.sql.*;

public class program2 {
    public static void main(String[] args) throws Exception {
        String url = "jdbc:oracle:thin:@localhost:1521/xepdb1";
        String sql = "INSERT INTO notice (title, writer_id, content, files)" +
        "VALUES(?,?,?,?)";

        Class.forName("oracle.jdbc.driver.OracleDriver");
        Connection con = DriverManager.getConnection(url, "newlec", "1234");

        String title = "test2";
        String writerId = "oracle";
        String content = "asd";
        String files = "";

        // Statement st = con.createStatement();
        PreparedStatement st = con.prepareStatement(sql);
        st.setString(1, title);
        st.setString(2, writerId);
        st.setString(3, content);
        st.setString(4, files);

        int result = st.executeUpdate();
        System.out.println(result);

        con.close();
    }
}
```

<br><br>

# 강의 10 - 데이터 수정을 위한 쿼리 준비하기

- 강의 11에서 쓸 sql문 만들고 끝

<br><br>

# 강의 11 - 데이터 수정을 구현하기

- 아래 코드는 강의 9의 코드와 거의 유사.

```java
String sql = "update notice set title=?, content=?, files=?" +
        " where id=?";

Class.forName("oracle.jdbc.driver.OracleDriver");
Connection con = DriverManager.getConnection(url, "newlec", "1234");

String title = "test3";
String content = "content3";
String files = "";
int id = 6;

PreparedStatement st = con.prepareStatement(sql);
st.setString(1, title);
st.setString(2, content);
st.setString(3, files);
st.setInt(4, id);

int result = st.executeUpdate();
System.out.println(result);
```

<br><br>

# 강의 12 - 데이터 삭제하기

- 내용은 강의 9, 11과 거의 유사하기 때문에 기록 생략

<br><br>

# 강의 13 - CRUD를 담당하는 서비스 클래스 생성하기

- NoticeService.java
  - getList() 함수 생성. 내용은 기존의 강의 7에서 작성한 코드에서 조금 손 본 정도

```java
public class NoticeService {
    public List<Notice> getList() throws ClassNotFoundException, SQLException {
        String url = "jdbc:oracle:thin:@localhost:1521/xepdb1";
        String sql = "select * from notice";

        Class.forName("oracle.jdbc.driver.OracleDriver");
        Connection con = DriverManager.getConnection(url, "newlec", "1234");
        Statement st = con.createStatement();
        ResultSet rs = st.executeQuery(sql);

        List<Notice> list = new ArrayList<>();

        while (rs.next()) {
            int id = rs.getInt("id");
            String title = rs.getString("title");
            String writerId = rs.getString("writer_id");
            Date regDate = rs.getDate("regdate");
            String content = rs.getString("content");
            int hit = rs.getInt("hit");

            Notice notice = new Notice(id, title, writerId, regDate, content, hit);
            list.add(notice);
        }

        rs.close();
        st.close();
        con.close();

        return list;
    }
}
```

- Notice.java
  - notice 정보 담을 객체 생성

```java
public class Notice {
    private int id;
    private String title;
    private String writerId;
    private Date regDate;
    private String content;
    private int hit;

    public Notice() {
    }

    public Notice(int id, String title, String writerId, Date regDate, String content, int hit) {
        this.id = id;
        this.title = title;
        this.writerId = writerId;
        this.regDate = regDate;
        this.content = content;
        this.hit = hit;
    }

    // setter and getter는 중요하지 않고 길이가 길어서 생략

}
```

<br><br>

# 강의 14 - NoticeService 구현 마무리

- Notice.java

  - notice 객체에 빠진 files 필드 추가 및 그에 따른 추가 수정 조금
  - `private String files;` 넣고, 그에 따라 생성자도 이걸 받을 수 있도록 수정.
  - 그에 따라 강의 13에 작성한 getList() 내부도 조금 수정.

- NoticeService.java
  - insert(), update(), delete() 함수 추가. 각 함수의 내용은 ex1 안의 각 코드를 복붙한 후에 조금 수정만 했음.

```java
package com.newlecture.app.service;

import java.sql.*;
import java.util.*;
import java.util.Date;

import com.newlecture.app.entity.Notice;

public class NoticeService {
    private String url = "jdbc:oracle:thin:@localhost:1521/xepdb1";
    private String uid = "newlec";
    private String pwd = "1234";
    private String driver = "oracle.jdbc.driver.OracleDriver";

    public List<Notice> getList() throws ClassNotFoundException, SQLException {
        String sql = "select * from notice";

        Class.forName(driver);
        Connection con = DriverManager.getConnection(url, uid, pwd);
        Statement st = con.createStatement();
        ResultSet rs = st.executeQuery(sql);

        List<Notice> list = new ArrayList<>();

        while (rs.next()) {
            int id = rs.getInt("id");
            String title = rs.getString("title");
            String writerId = rs.getString("writer_id");
            Date regDate = rs.getDate("regdate");
            String content = rs.getString("content");
            int hit = rs.getInt("hit");
            String files = rs.getString("files");

            Notice notice = new Notice(id, title, writerId, regDate, content, hit, files);
            list.add(notice);
        }

        rs.close();
        st.close();
        con.close();

        return list;
    }

    public int insert(Notice notice) throws SQLException, ClassNotFoundException {
        String sql = "INSERT INTO notice (title, writer_id, content, files)" +
                "VALUES(?,?,?,?)";

        Class.forName(driver);
        Connection con = DriverManager.getConnection(url, uid, pwd);

        String title = notice.getTitle();
        String writerId = notice.getWriterId();
        String content = notice.getContent();
        String files = notice.getFiles();

        // Statement st = con.createStatement();
        PreparedStatement st = con.prepareStatement(sql);
        st.setString(1, title);
        st.setString(2, writerId);
        st.setString(3, content);
        st.setString(4, files);

        int result = st.executeUpdate();

        st.close();
        con.close();
        return result;
    }

    public int update(Notice notice) throws ClassNotFoundException, SQLException {
        String sql = "update notice set title=?, content=?, files=?" +
                " where id=?";

        Class.forName(driver);
        Connection con = DriverManager.getConnection(url, uid, pwd);

        String title = notice.getTitle();
        String content = notice.getContent();
        String files = notice.getFiles();
        int id = notice.getId();

        // Statement st = con.createStatement();
        PreparedStatement st = con.prepareStatement(sql);
        st.setString(1, title);
        st.setString(2, content);
        st.setString(3, files);
        st.setInt(4, id);

        int result = st.executeUpdate();

        st.close();
        con.close();
        return result;
    }

    public int delete(int id) throws ClassNotFoundException, SQLException {
        String sql = "delete notice where id=?";

        Class.forName(driver);
        Connection con = DriverManager.getConnection(url, uid, pwd);

        // Statement st = con.createStatement();
        PreparedStatement st = con.prepareStatement(sql);
        st.setInt(1, id);

        int result = st.executeUpdate();

        st.close();
        con.close();
        return result;
    }
}
```

<br><br>

# 강의 15 - 사용자 인터페이스 붙이기(공지사항 목록)

- 게시글 ui는 콘솔 형식으로 제공
  - NoticeConsole.java에서 printNotice() 구현. 다음 강의에도 수정될 수도 있음(추측)
- NoticeConsole.java

```java
package com.newlecture.app.console;

import java.sql.SQLException;
import java.util.List;

import com.newlecture.app.entity.Notice;
import com.newlecture.app.service.NoticeService;

public class NoticeConsole {

    private NoticeService service;

    public NoticeConsole() {
        service = new NoticeService();
    }

    public void printNotice() throws ClassNotFoundException, SQLException {
        List<Notice> list = service.getList();

        System.out.println("───────────────────────────────────────────────────────────────────────");
        System.out.printf("<공지사항> 총 %d 개의 게시글\n", 12);
        System.out.println("───────────────────────────────────────────────────────────────────────");

        for (Notice n : list) {
            System.out.printf("%d. %s / %s / %s\n",
                    n.getId(), n.getTitle(), n.getWriterId(), n.getRegDate());
        }

        System.out.println("───────────────────────────────────────────────────────────────────────");

        System.out.printf("            %d/%d pages\n", 1, 2);

    }

    public int inputNoticeMenu() {
        return 0;
    }
}
```

<br><br>

# 강의 16 - 공지사항 메뉴 붙이기

- NoticeConsole.java
  - printNotice()는 강의 15의 코드와 동일.
  - inputNoticeMenu() 구현
- program5.java
  - NoticeConsole.java 이용해서 콘솔 출력하는 파일

```java
// NoticeConsole.java
public class NoticeConsole {

    public int inputNoticeMenu() {
        Scanner sc = new Scanner(System.in);
        System.out.print("1.상세조회/ 2.이전/ 3.다음/ 4.글쓰기/ 5.종료 >");

        String menu_ = sc.nextLine();
        int menu = Integer.parseInt(menu_);

        return menu;
    }
}
```

```java
// program5.java
package ex1;

import com.newlecture.app.console.NoticeConsole;

public class program5 {
    public static void main(String[] args) throws Exception {
        NoticeConsole console = new NoticeConsole();

        Exit: while (true) {
            console.printNotice();
            int menu = console.inputNoticeMenu();

            switch (menu) {
                case 1: // 상세조회
                    break;
                case 2: // 이전
                    break;
                case 3: // 다음
                    break;
                case 4: // 글쓰기
                    break;
                case 5: // 종료
                    System.out.println("종료합니다.");
                    break Exit;
                default:
                    System.out.println("<<사용방법>> 메뉴는 1~4까지만 입력할 수 있습니다.");
                    break;
            }
        }

    }
}
```

<br><br>

# 강의 17 - 페이징을 위한 쿼리 만들기

- 페이징을 위한 쿼리 만들기
  - 10개의 게시글을 최신순으로 얻기 위해 ROWNUM과 서브 쿼리를 활용. 서브 쿼리는 두 번 중첩해서 사용
  - 먼저 등록일을 기준으로 최신순으로 정렬 후, ROWNUM을 붙이기.
  - 그런 다음 정렬된 레코드에 올바르게 붙여진 ROWNUM을 10개만 뽑는 걸 WHERE절에서 구현

```SQL
SELECT * FROM (
    SELECT ROWNUM NUM, N.* FROM (
        SELECT * FROM NOTICE ORDER BY REGDATE DESC
    ) N
)
WHERE NUM BETWEEN 1 AND 10;
```

<br><br>

# 강의 18 - 페이징 쿼리 이용하기

- NoticeService 속 getList 약간 수정
  - sql문을 강의 17에서 얻은 sql문으로 교체
  - `PreparedStatement st = con.prepareStatement(sql);`로 교체
  - sql문 속 ?를 setInt로 채워넣기

```java
 public List<Notice> getList(int page) throws ClassNotFoundException, SQLException {
        String sql = "SELECT * FROM (" +
                "SELECT ROWNUM NUM, N.* FROM (" +
                "SELECT * FROM NOTICE ORDER BY REGDATE DESC" +
                ") N" +
                ")" +
                " WHERE NUM BETWEEN ? AND ?";
        int start = 1 + (page - 1) * 10; // 등차수열 일반항: 1, 11, 21, 31 ...
        int end = 10 * page; // 10, 20, 30, 40 ...

        Class.forName(driver);
        Connection con = DriverManager.getConnection(url, uid, pwd);

        // PreparedStatement로 바꾸기.
        // st로 sql 속 ? 다 set해 준 다음에 ResultSet 얻어야 함.
        PreparedStatement st = con.prepareStatement(sql);
        st.setInt(1, start);
        st.setInt(2, end);

        ResultSet rs = st.executeQuery();

        List<Notice> list = new ArrayList<>();
        while (rs.next()) {
            int id = rs.getInt("id");
            String title = rs.getString("title");
            String writerId = rs.getString("writer_id");
            Date regDate = rs.getDate("regdate");
            String content = rs.getString("content");
            int hit = rs.getInt("hit");
            String files = rs.getString("files");

            Notice notice = new Notice(id, title, writerId, regDate, content, hit, files);
            list.add(notice);
        }

        rs.close();
        st.close();
        con.close();

        return list;
    }
```

<br><br>

# 강의 19 - 목록을 위한 View 생성하기

- 강의 17의 페이징을 위한 쿼리를 뷰로 저장 후 강의 18의 getList 속 SQL문 수정

```sql
CREATE VIEW NOTICE_VIEW AS
SELECT * FROM (
    SELECT ROWNUM NUM, N.* FROM (
        SELECT * FROM NOTICE ORDER BY REGDATE DESC
    ) N
);
```

```java
// NoticeService 속 getList() 내의 SQL 수정
String sql = "SELECT * FROM NOTICE_VIEW WHERE NUM BETWEEN ? AND ?";
```

<br><br>

# 강의 20 - 이전 / 다음 구현하기

- program5.java
  - case 2, 3에 movePrevList()와 moveNextList() 추가

```java
case 2: // 이전
    console.movePrevList();
    break;
case 3: // 다음
    console.moveNextList();
    break;
```

- NoticeConsole.java
  - page 추가: 페이지 상태 정보를 담고 있는 변수
  - printNoticeList()의 getList() 메서드에 page 매개변수로 추가
  - movePrevList(), moveNextList() 추가

```java
public class NoticeConsole {

    private NoticeService service;
    private int page; // 페이지 상태 정보를 담고 있는 변수

    public NoticeConsole() {
        service = new NoticeService();
        page = 1; // page 기본값은 1
    }

    public void printNoticeList() throws ClassNotFoundException, SQLException {
        List<Notice> list = service.getList(page);
        // 기타 구현코드
    }

    public void movePrevList() {
        if (page == 1) {
            System.out.println("이전 페이지가 없습니다");
            return;
        }

        page--;
    }

    public void moveNextList() {
        page++;
    }
}
```

<br><br>

# 강의 21 - 게시글 갯수 구하기

- NoticeConsole 속 printNoticeList()에 총 게시글 수 나타내는 지역변수 count 만들기(`int count=service.getCount();`)
- count를 지역변수로 선언한 이유는 매번 글이 추가되거나 삭제될 수 있고, 이를 반영해야 하니까 printNoticeList()가 호출될 때마다 새로 업데이트되어야 하기 때문이다.
- 그럼 NoticeService에 getCount()를 구현한다. 구현은 기존에 NoticeService의 getList() 코드를 바탕으로 한다. 왜냐면 이 함수도 select 쿼리를 이용할 거기 때문이다. 참고로 단일값(여기서는 count값 하나)을 얻어온다는 말을 다르게 스칼라(scalar)값을 얻어온다고 말한다.

```java
public int getCount() throws ClassNotFoundException, SQLException {
    int count = 0;
    String sql = "SELECT COUNT(ID) COUNT FROM NOTICE";

    Class.forName(driver);
    Connection con = DriverManager.getConnection(url, uid, pwd);
    Statement st = con.createStatement();
    ResultSet rs = st.executeQuery(sql);

    if (rs.next())
        count = rs.getInt("count");

    rs.close();
    st.close();
    con.close();

    return count;
}
```

<br><br>

# 강의 22 - 마지막 페이지 구하기

- NoticeConsole에 printNoticeList()에서 현재 페이지는 NoticeConsole가 갖고 있는 page 변수를 활용
- 마지막 페이지는 lastPage 지역변수를 만들고 다음처럼 대입. `int lastPage = count % 10 == 0 ? count / 10 : count / 10 + 1;`
- 현재 게시글 수가 30, 40처럼 10으로 나누어떨어질 시 count / 10이 마지막 페이지수. 반면 33, 43처럼 10으로 안 나눠지면, count / 10 + 1을 해야 게시글 전부를 조회 가능

```java
public void printNoticeList() throws ClassNotFoundException, SQLException {
    List<Notice> list = service.getList(page);
    int count = service.getCount();
    int lastPage = count % 10 == 0 ? count / 10 : count / 10 + 1;

    System.out.println("───────────────────────────────────────────────────────────────────────");
    System.out.printf("<공지사항> 총 %d 개의 게시글\n", count);
    System.out.println("───────────────────────────────────────────────────────────────────────");
    for (Notice n : list) {
        System.out.printf("%d. %s / %s / %s\n",
                n.getId(), n.getTitle(), n.getWriterId(), n.getRegDate());
    }
    System.out.println("───────────────────────────────────────────────────────────────────────");
    System.out.printf("            %d/%d pages\n", page, lastPage);

}
```

- NoticeConsole에 moveNextList() 메서드도 구현하기
- 매번 moveNextList()로 다음 페이지로 옮길 때 매번 getCount()해서 lastPage를 구하는 이유는, 그래야지 중간에 글이 추가되거나 삭제되었을 때도 대비 가능하니까

```java
public void moveNextList() throws ClassNotFoundException, SQLException {
    int count = service.getCount();
    int lastPage = count % 10 == 0 ? count / 10 : count / 10 + 1;

    if (page == lastPage) {
        System.out.println("=========================");
        System.out.println("[다음 페이지가 없습니다]");
        System.out.println("=========================");
        return;
    }

    page++;
}
```

<br><br>

# 강의 23 - 검색 메뉴 붙이기

- NoticeConsole에서 inputNoticeMenu()가 있다. 검색 기능을 출력문에 추가하자(`System.out.print("1.상세조회/ 2.이전/ 3.다음/ 4.글쓰기/ 5. 검색 / 6.종료 >");`)
- 메뉴를 수정했으니, main()에서 case 5는 검색, case 6은 종료로 수정. 그 후 검색할 때 필요한 입력 단어를 받는 함수 추가

```java
case 5: // 검색
    console.inputSearchWord();
    break;
```

- NoticeConsole에서 inputSearchWord() 메서드를 구현. 이 때 searchField, searchWord는 다른 메서드에 쓰일 예정이므로 클래스 멤버 변수로 선언

```java
private String searchField;
private String searchWord;

public NoticeConsole() {
    service = new NoticeService();
    page = 1; // page 기본값은 1
    searchField = "";
    searchWord = "";
}

public void inputSearchWord() {
    Scanner sc = new Scanner(System.in);
    System.out.println("검색 범주(title/content/writerId) 중에 하나를 입력하세요 > ");
    searchField = sc.nextLine();

    System.out.println("검색어> ");
    searchWord = sc.nextLine();
}
```

<br><br>

# 강의 24 - 검색 서비스 추가하기

- 검색을 위해 NoticeService에서 getList() 매개변수로 field(검색 범주), query(검색 단어) 추가(`getList(int page, String field, String query)`)
- getList() 내의 sql문을 수정(` sql = "SELECT * FROM NOTICE_VIEW WHERE " + field + " LIKE ? AND NUM BETWEEN ? AND ?";`)
  - 이 때 field를 굳이 불편하게 넣지 말고 ?로 둔 다음에 st.setString하면 안되나라고 생각할 수 있음. 안 되는 이유는 setString은 무조건 set할 때 따옴표로 감싸서 넣음(문자열(값)이라고 생각하고). 하지만 field는 컬럼명이라서 안 됨.
- sql문 수정에 따라 다음 코드도 수정

```java
st.setString(1, "%" + query + "%");
st.setInt(2, start);
st.setInt(3, end);
```

- NoticeConsole에 printNoticeList() 안에서 다음처럼 수정(`List<Notice> list = service.getList(page, searchField, searchWord);`)
- 그 후 NoticeConsole() 생성자 안에서 searchField에 기본값(title) 설정
  - 기본값 설정 안 하면 etList() 내의 sql(`SELECT * FROM NOTICE_VIEW WHERE " + field + " LIKE ? AND NUM BETWEEN ? AND ?`) 속 field가 ""가 되서 오류남(main() 처음 실행할 때)

```java
public NoticeConsole() {
    service = new NoticeService();
    page = 1; // page 기본값은 1
    searchField = "title";
    searchWord = "";
}
```
