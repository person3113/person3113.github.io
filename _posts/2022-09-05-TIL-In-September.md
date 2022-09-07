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
last_modified_at: 2022-09-07
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

<br><br>

# On 09/07 (Wed)

## Opentutorials.org

- From "Java 상속 - 1. 수업소개" to "Java 상속 - 6 수업을 마치며"

## "Do it! 알고리즘 코딩테스트 자바 편"

- p.17~p.44

  - "숫자의 합 구하기 (문제 001/35p)"

    - Scanner sNum=new Scanner(System.in); //create Scnner instance
    - int N = sc.nextInt(); // returns the Int scanned from the input.
    - String sNum = sc.next(); // scan input data like "12345"
    - char[] cNum = sNum.toCharArray(); // ex) "1234".toCharArray() -> return character array {1,2,3,4}
    - how to convert '1' to 1
      - In ASCII, the difference between the code values of letters and numbers of the same meaning is 48.
      - '1' - 48 or '1' - '0'

    <br>

  - "평균 구하기 (문제 002, 38p)(Baekjoon Online Judge: 1546번) "

    - This problem wasn't difficult for me, but I failed to solve it.
    - My solving process is as follow:

    ```java
    import java.util.Scanner;

    public class Main {
      public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        int[] grade = new int[n];

        for (int i = 0; i < n; i++) {
            grade[i] = sc.nextInt();
        }

        int max = grade[0];
        for (int i = 0; i < n; i++) {
            if (grade[i] > max) {
                max = grade[i];
            }
        }

        for (int i = 0; i < n; i++) {
            grade[i] = grade[i] * 100 / max;
        }

        int sum = 0;
        for (int i = 0; i < n; i++) {
            sum += grade[i];
        }
        System.out.println((double) sum / n);

        sc.close();
      }
    }
    ```

    <br>

  - 00:00 ~ 12:00 in "코딩 테스트 핵심이론 강의 - 시간복잡도 (Java)"

    - I didn't understand why "1,000,000log(1,000,000)" is about 20,000,000. Because I thought the base of the log is 10.
    - Therefore, I searched several sites and found out the following facts.

      - First, in the "Big-O" notation, the coefficient is discarded and only the degree is considered.
      - Second, The logarithm has the following properties.
        ![image](https://t1.daumcdn.net/cfile/tistory/997A6D435C5299E021)
        Source: <https://nacture.kr/73>

        for example, "log a N" is "log c N / log c a". this coefficient is "log c a", so it is discarded. Therefore, O(log a N) is O(log c N). After all, the base dosen't really matter.
