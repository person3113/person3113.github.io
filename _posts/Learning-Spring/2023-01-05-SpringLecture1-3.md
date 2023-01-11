---
title: "[스프링 입문(김영한)] '스프링 JdbcTemplate' ~ '다음으로'"
excerpt: "스프링 DB 접근 기술 / AOP / 다음으로"

categories:
  - Learning-Spring
tags:
  - [Spring Boot]

toc: true
toc_sticky: true
toc_label: "페이지 주요 목차"

date: 2023-01-05
last_modified_at: 2023-01-05
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

# 스프링 DB 접근 기술

## 스프링 JdbcTemplate

<br>

## JPA

### 키워드

- @Entity
- 어노테이션으로 데이터베이스와 매핑
- IDENTITY 전략(기본 키 생성을 데이터베이스에 위임하는 전략)
- JPQL(Java Persistence Query Language / 테이블이 아닌 엔티티 객체를 대상으로 검색하는 객체지향 쿼리)

### 의문점

- JPA란
  - JPA(Java Persistence API). 자바 진영에서 ORM(Object-Relational Mapping) 기술 표준으로 사용되는 인터페이스의 모음. 자바 어플리케이션에서 관계형 데이터베이스를 사용하는 방식을 정의한 인터페이스. 인터페이스 이기 때문에 Hibernate, OpenJPA 등이 JPA를 구현함
  - ORM(Object-Relational Mapping): 애플리케이션 Class와 RDB(Relational DataBase)의 테이블을 매핑(연결)한다는 뜻이며, 기술적으로는 어플리케이션의 객체를 RDB 테이블에 자동으로 영속화 해주는 것이라고 보면된다.
  - 사용 이유: JPA는 반복적인 CRUD SQL을 처리해준다. JPA를 사용하여 얻을 수 있는 가장 큰 것은 SQL아닌 객체 중심으로 개발할 수 있다는 것이다. 이에 따라 당연히 생산성이 좋아지고 유지보수도 수월하다.
- JPA와 JDBC의 차이점
  - [어렵지 않고 명확하게 잘 설명한 블로그. 보면 좋을 것 같음](https://velog.io/@kkimbj18/JPA-%EA%B8%B0%EB%B3%B8-%EA%B0%9C%EB%85%90%EA%B3%BC-%EC%82%AC%EC%9A%A9-%EC%9D%B4%EC%9C%A0-Feat.-JDBC)

<br>

## 스프링 데이터 JPA

<br><br>

# AOP

## AOP가 필요한 상황

### 키워드

- 메소드의 호출 시간 측정
- 공통 관심 사항(cross-cutting concern)과 핵심 관심 사항(core concern)

<br>

## AOP 적용

### 키워드

- AOP(Aspect Oriented Programming, 관점 지향 프로그래밍),
- @Aspect, @Around
- 프록시 방식 AOP

### 의문점

- 프록시 방식 AOP란?
  - AOP: 문제를 해결하기 위한 핵심 관심 사항과 전체에 적용되는 공통 모듈 사항을 기준으로 프로그래밍 함으로써 공통 모듈을 여러 코드에 쉽게 적용할 수 있도록 도와주는 역할.
  - AOP 용어
    - Advice:
      - 언제 공통 관심 기능을 핵심로직에 적용할 지를 정의함.
      - @Around (메소드 실행 전 후): 어드바이스가 타겟 메소드를 감싸서 타겟 메소드 호출 전, 후 어드바이스 기능을 수행
      - @Before : 어드바이스 타겟 메소드가 호출되기 전에 어드바이스 기능을 수행
      - @After : 타겟 메소드의 결과에 관계없이(성공, 예외 상관없이) 타겟 메소드가 완료되면 어드바이스 기능 수행
    - Joinpoint: Advice를 적용 가능한 지점을 의미함. 메소드호출, 필드값 변경 등이 Joinpoint에 해당.
    - Pointcut: Joinpoint의 부분집합으로서 실제로 Advice가 적용되는 Joinpoint를 나타냄. 스프링에서는 정규 표현식이나 AspectJ의 문법을 이용하여 Poincut을 재정의 가능.
    - Weaving: Advice를 핵심로직코드에 적용하는 것. 즉 공통코드를 핵심로직코드에 삽입하는 것.
    - Aspect: 여러 객체에 공통으로 적용되는 기능. 트랜잭션이나, 보안 등이 Aspect의 좋은 예다.
  - Spring AOP: 스프링은 자체적으로 프록시 기반의 AOP를 지원함. 따라서 스프링 AOP는 메서드 호출 Joinpoint만을 지원함. 필드 값 변경과 같은 Joinpoint를 사용하고 싶다면 AspectJ와 같이 다양한 Joinpoint를 지원하는 AOP프레임워크를 사용해야 한다.
  - 프록시 Proxy: 타겟을 감싸서 타겟의 요청을 대신 받아주는 랩핑(Wrapping) 오브젝트. 클라이언트에서 타겟을 호출하면, 타겟을 감싸고 있는 프록시가 호출되어, 어드바이스에 등록된 기능을 수행 후 타겟 메소드를 호출한다.
