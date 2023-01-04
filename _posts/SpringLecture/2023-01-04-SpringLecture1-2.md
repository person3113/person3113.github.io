---
title: "[스프링 입문(김영한)] '회원 서비스 개발' ~ '스프링 통합 테스트'"
excerpt: "3. 회원 관리 예제 - 백엔드 개발 / 스프링 빈과 의존관계 / 스프링 DB 접근 기술"

categories:
  - SpringLecture
tags:
  - [Spring]

toc: true
toc_sticky: true
toc_label: "페이지 주요 목차"

date: 2023-01-04
last_modified_at: 2023-01-04
---

<br>

# [인프런 강의 주소](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EC%9E%85%EB%AC%B8-%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8/dashboard)

<br>

# 보는 방법

- 블로그에 메모한 내용만으로 복습하기에는 무리.
- 강의 자료는 해당 강의에서 제공해 줌. 강의 자료로 복습하고, 기억나지 않는다 싶으면 영상 보기.
- 여기서는 다음을 기록.
  - 강의 핵심 키워드
  - 강의를 보고 생긴 의문에 대해 구글링해서 찾은 내용
  - 나중에 다시 볼 때 참고할 만한 정보

<br>

# 3. 회원 관리 예제 - 백엔드 개발

## 회원 서비스 개발

### 의문점

- final 변수인데 조작한다고?
  - final 변수는 초기화 이후 값 변경이 발생하지 않도록 만든다. 예로 List에 final을 선언하여 list 변수의 변경은 불가능하다. 하지만 list 내부에 있는 변수들은 변경이 가능하여 문자열을 계속 추가할 수 있다.
  - 따라서, final 변수의 변경을 막아주지만 final 변수 내부에 갖고 있는 변수들은 변경이 가능하다.

<br>

## 회원 서비스 테스트

### 키워드

테스트 사고흐름(given, when, then), 의존관계 주입(Dependency Injection, DI)

### 의문점

- static import란?
  - 정적(static) 메소드와 변수를 더욱 쉽게 사용하기 위해서 static import 를 지원.
  - static import한 후에 클래스명 없이 정적 메소드와 정적 변수를 사용할 수 있음.
- DI란
  - 의존관계(A가 B를 의존한다): 의존대상 B가 변하면, 그것이 A에 영향을 미친다. 즉, B의 기능이 추가 또는 변경되거나 형식이 바뀌면 그 영향이 A에 미친다.
  - 의존관계를 외부에서 결정하고 주입하는 것이 DI(의존관계 주입)이다.
  - 예시는 강의영상 참고

<br><br>

# 스프링 빈과 의존관계

## 컴포넌트 스캔과 자동 의존관계 설정

### 키워드

스프링 컨테이너와 컨트롤러(스프링 빈), 스프링 빈 등록(컴포넌트 스캔 / @Controller, @Service, @Repository), @Autowired(DI, 의존성 주입), 싱글톤

<br>

## 자바 코드로 직접 스프링 빈 등록하기

### 키워드

@Configuration, @Bean, DI 방법(필드 주입, setter 주입, 생성자 주입)

<br><br>

# 회원 관리 예제 - 웹 MVC 개발

## 회원 웹 기능 - 홈 화면 추가

<br>

## 회원 웹 기능 - 등록

### 키워드

html(form, input 태그), get과 post방식

### 의문점

- get과 post 방식은?
  - HTTP 요청 메서드
    - URL을 이용하면 서버에 특정 데이터를 요청할 수 있다. 여기서 요청하는 데이터에 특정 동작을 수행하고 싶으면 어떻게 해야 할까요? 바로 HTTP 요청 메서드(Http Request Methods)를 이용.
    - 일반적으로 HTTP 요청 메서드는 HTTP Verbs라고도 불리우며 아래와 같이 주요 메서드를 갖고 있습니다. 이와 같이 데이터에 대한 조회, 생성, 변경, 삭제 동작을 HTTP 요청 메서드로 정의 가능.
    - PUT,DELETE도 post방식으로 가능. 사실 대부분 GET,POST방식만 사용.
      - GET : 존재하는 자원에 대한 요청
      - POST : 새로운 자원을 생성
      - PUT : 존재하는 자원에 대한 변경
      - DELETE : 존재하는 자원에 대한 삭제
  - GET
    - GET은 요청을 전송할 때 필요한 데이터를 Body에 담지 않고, 쿼리스트링을 통해 전송.
    - 쿼리스트링: URL의 끝에 ?와 함께 이름과 값으로 쌍을 이루는 요청 파라미터. 만약 요청 파라미터가 여러 개이면 &로 연결(ex- www.example-url.com/resources?name1=value1&name2=value2)
    - GET은 불필요한 요청을 제한하기 위해 요청이 캐시될 수 있다. js, css, 이미지 같은 정적 컨텐츠는 데이터양이 크고, 변경될 일이 적어서 반복해서 동일한 요청을 보낼 필요가 없다. 정적 컨텐츠를 요청하고 나면 브라우저에서는 요청을 캐시해두고, 동일한 요청이 발생할 때 서버로 요청을 보내지 않고 캐시된 데이터를 사용.
  - POST
    - POST는 리소스를 생성/변경하기 위해 설계되었기 때문에 GET과 달리 전송해야될 데이터를 HTTP 메세지의 Body에 담아서 전송한다. HTTP 메세지의 Body는 길이의 제한없이 데이터를 전송할 수 있다. 그래서 POST 요청은 GET과 달리 대용량 데이터를 전송할 수 있다
    - POST로 요청을 보낼 때는 요청 헤더의 Content-Type에 요청 데이터의 타입을 표시해야 한다. Content-Type의 종류로는 application/x-www-form-urlencoded, text/plain, multipart/form-data 등이 있다.
    - 데이터 타입을 표시하지 않으면 서버는 내용이나 URL에 포함된 리소스의 확장자명 등으로 데이터 타입을 유추한다. 만약, 알 수 없는 경우에는 application/octet-stream로 요청을 처리한다.

<br>

## 회원 웹 기능 - 조회

### 키워드

html(table 관련 태그)

### 의문점

- MVC란?
  - MVC 는 Model, View, Controller의 약자다. 하나의 애플리케이션, 프로젝트를 구성할 때 그 구성요소를 세가지의 역할로 구분한 패턴이다.
  - 모델
    - 데이터를 가진 객체를 모델이라고 지칭한다. 모델의 상태에 변화가 있을 때 컨트롤러와 뷰에 이를 통보한다. 이와 같은 통보를 통해 뷰는 최신의 결과를 보여줄 수 있고, 컨트롤러는 모델의 변화에 따른 적용 가능한 명령을 추가, 제거, 수정할 수 있다.
    - 모델의 규칙
      - 사용자가 편집하길 원하는 모든 데이터를 가지고 있어야만 함
      - 뷰나 컨트롤러에 대해서 어떠한 정보도 알지 말아야 함
      - 변경이 일어나면, 변경 통지에 대한 처리방법을 구현해야 함
  - 뷰
    - View는 클라이언트 측 기술인 HTML/CSS/Javascript들을 모아둔 컨테이너이다. 사용자 인터페이스 요소를 나타낸다. 사용자가 볼 결과물을 생성하기 위해 모델로부터 정보를 얻어온다.
    - 뷰의 규칙
      - 모델이 가지고 있는 정보를 따로 저장해서는 안됨
      - 모델이나 컨트롤러와 같이 다른 구성 요소를 몰라야 함
      - 변경이 일어나면, 변경 통지에 대한 처리방법을 구현해야 함
  - 컨트롤러
    - 데이터와 사용자 인터페이스 요소들을 잇는 다리역할. 사용자가 데이터를 클릭하고, 수정하는 것에 대한 "이벤트"들을 처리하는 부분.
    - 컨트롤러의 규칙
      - 모델이나 뷰에 대해서 알고 있어야 한다.
      - 모델이나 뷰의 변경을 모니터링 해야 한다.

<br><br>

# 스프링 DB 접근 기술

## H2 데이터베이스 설치

### 의문점

- JDBC란?
  - 자바 언어로 다양한 종류의 관계형 데이터베이스에 접속하고 SQL문을 수행하여 처리하고자 할 때 사용되는 표준 SQL 인터페이스 API이다.
  - JDBC 인터페이스: JDBC 프로그램을 하기 위한 API들로서, Java SE(Standard Edition)에서 제공하는 java.sql 패키지를 의미한다. JDBC 프로그램을 구현할 때 실제로 사용하는 객체들은 대부분 몸체가 없는 인터페이스 이다.
  - JDBC 드라이버: 실제 DB관련 기능이 동작하려면 이 인터페이스 만으로는 작업할 수 없다. 그렇기 떄문에 java.sql의 인터페이스들을 상속하여 메소드의 몸체를 구현한 클래스 파일들이 필요하며 이 파일들을 JDBC 드라이버라고 한다.

<br>

## 순수 JDBC

### 키워드

개방-폐쇄 원칙(OCP, Open-Closed Principle), DI (Dependencies Injection)

<br>

## 스프링 통합 테스트

### 키워드

@SpringBootTest, @Transactional(테스트 케이스, rollback)