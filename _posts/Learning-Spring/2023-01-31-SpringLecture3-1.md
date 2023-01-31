---
title: "[스프링 부트와 JPA 활용1 (김영한)] "
excerpt: ""

categories:
  - Learning-Spring
tags:
  - [Spring Boot, JPA]

toc: true
toc_sticky: true
toc_label: "페이지 주요 목차"

date: 2023-01-31
last_modified_at: 2023-01-31
---

<br>

# [인프런 강의 주소](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8-JPA-%ED%99%9C%EC%9A%A9-1/dashboard)

<br>

# 보는 방법

- 블로그에 메모한 내용만으로 복습하기에는 무리.
- 강의 자료는 해당 강의에서 제공해 줌. 강의 자료로 복습하고, 기억나지 않는다 싶으면 영상 보기.
- 여기서는 다음을 기록.
  - 강의 핵심 키워드
  - (가끔씩) 강의를 보고 생긴 의문에 대해 구글링해서 찾은 내용
  - (가끔씩) 나중에 다시 볼 때 참고할 만한 정보

<br>

# 프로젝트 환경설정

## 프로젝트 생성

### 복습 때 꼭 참고! 폴더명 때문에 두 번째 실행이 안 될 수 있다. ("the system cannot find the path specified")

- 문제 상황
  - "실전! 스프링 부트와 JPA 활용1 (김영한)"이라는 폴더 안에 프로젝트를 생성해서 처음 실행하니 잘 되었다.
  - 하지만 두 번째 실행했을 때 "the system cannot find the path specified"라는 출력과 함께 아예 안 되었다.
- 폴더명이 문제였다.
  - 대충 감으로 때려 맞췄다. 주로 이런 오류는 오라클 설치할 때도 그렇고, 한글이 지원되지 않거나, 특수문자가 있으면 뭔가뭔가 꼬여서 안 되는 경우를 떠올렸다.
  - 그래서 나는 "!"가 문제라 추측하고 "실전!" 부분을 지워서 "스프링 부트와 JPA 활용1 (김영한)"이라고 수정했다.
  - 그 후 5~6번 계속 실행해도 잘 작동하게 되었다.
