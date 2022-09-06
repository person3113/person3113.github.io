---
title: "[TIL] September In 2022"
excerpt: "The log that I learned what is related with programming in september 2022"

categories:
  - TIL
tags:
  - [TIL, Java, VSCode]

toc: true
toc_sticky: false

date: 2022-09-05
last_modified_at: 2022-09-06
---

# On 09/05 (Mon)

## "생활코딩 JAVA 입문 수업"

- Watching video lecture From "자바 설치" to "입력과 출력" in ["생활코딩"](https://opentutorials.org/course/3930)

- After watching video "입력과 출력", I wonder how to pass arguments in not eclipse but vsCode
  - launch.json
  ```json
  {
    "version": "0.2.0",
    "configurations": [
      {
        "type": "java",
        "name": "Launch Current File",
        "request": "launch",
        "mainClass": "${file}"
      },
      {
        "type": "java",
        "name": "Launch App",
        "request": "launch",
        "mainClass": "App",
        "projectName": "test project_6ad5dc36"
      }
    ]
  }
  ```
  - input "args": "one two" in the first curly brackets, it makes error.
  - I input "args": "one two" in the second curly brackets, it dose work.
  - Reference: [Stack overflow](https://stackoverflow.com/questions/52061958/vscode-java-launch-json-args)

<br><br>

# On 09/06 (Tue)

## what I learned today

- Opentutorials.org

  - From "JAVA1 - 12.1. 직접 컴파일하고 실행하기 : 소개" to "JAVA1 - 13.5. 자바 문서 보는 법 - 상속"
  - "문자의 비교 : ==과 equals의 차이점"
  - "JAVA 제어문 - 7.2. 배열"
  - From "JAVA method - 1. 수업소개" to "JAVA method - 9. 부록 - static"
  - From "JAVA 객체 지향 프로그래밍 - 1. 수업소개" to "JAVA 객체 지향 프로그래밍 - 9 수업을 마치며"

## the awkward or difficult parts

- Opentutorials.org

  - "JAVA 객체 지향 프로그래밍 - 6. static"
  - "JAVA 객체 지향 프로그래밍 - 7. 생성자와 this"

## the interesting parts

- Opentutorials.org

  - "JAVA1 - 13.2. 자바 문서 보는 법 - 패키지,클래스,변수,메소드"
    - pakage: container of classes
    - class: container of related variables and methods
    - ex) java.lang: pakage, String: class
  - "JAVA1 - 13.4. 자바 문서 보는 법 - 인스턴스"
    - when I should use methods in a certain class many times, it is good to make instances.
    - basic python grammer videe come to mind that said "클래스는 틀이다. 그 틀로 찍어낸 것이 인스턴스다".
    - "constructor, 즉 생성자가 없는 애들은 일회용이다"
    - constructor: make instance (ex- PrintWriter p1 = new PrintWriter("result1.txt"))
  - "JAVA1 - 13.5. 자바 문서 보는 법 - 상속"
    - in VSCode, right click at PrintfWriter -> "Show Type Hierarchy" -> the class "PrintWriter" inherit the class "Writer", and "Writer" inherit the class "Object"
    - inheritance: a child class can use parent classes method
    - override: the situation when I make a method "example()" in a child class, even if parent has a "example()" method.
  - "문자의 비교 : ==과 equals의 차이점"
    - all parts of this video is interesting.
  - "JAVA 제어문 - 7.2. 배열"
    - two ways:
      1. String[] users = new String[3];
      2. int[] scores = {10, 100, 100};
  - "JAVA method - 8. 부록 - access level modifiers"
    - all parts of this video is interesting.
  - "JAVA method - 9. 부록 - static"
    - all parts of this video is interesting.
