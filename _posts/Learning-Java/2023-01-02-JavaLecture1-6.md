---
title: "[Do it! 자바 프로그래밍 입문] <컬렉션 프레임워크 - TreeSet, HashMap, TreeMap> ~ <자바 입출력(2)>"
excerpt: "컬렉션 프레임워크 / 내부 클래스(람다식, 스트림) / 예외 처리 / 입출력"

categories:
  - Learning-Java
tags:
  - [Java Lecture]

toc: true
toc_sticky: true
toc_label: "페이지 주요 목차"

date: 2023-01-02
last_modified_at: 2023-01-02
---

<br>

# [인프런 강의 주소](https://www.inflearn.com/course/%EC%9E%90%EB%B0%94-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D-%EC%9E%85%EB%AC%B8#curriculum)

<br>

# 보는 방법

블로그에 메모한 내용만으로 복습하기에는 무리. 복습할 땐 강의 영상 보고, 이건 상기용/테스트용. 메모한 거 보면서 강의 내용이 떠오르는지 등을 점검하는 데 활용.

<br>

# 3. 자바 JDK로 프로그래밍 날개 달기

## 컬렉션 프레임워크 - TreeSet, HashMap, TreeMap

- TreeSet 클래스
  - 자바 클래스 이름 앞에 Tree가 붙으면 정렬이 됩니다. 객체의 정렬에 사용되는 클래스. 중복을 허용하지 않으면서 오름차순이나 내림차순으로 객체를 정렬함.
  - 내부적으로 이진 검색 트리로 구현되어 있음. 이진 검색 트리에 자료가 저장될 때 비교하여 저장될 위치를 정함. 객체 비교를 위해 Comparable이나 Comparator 인터페이스를 구현해야 함.
- Comparable 과 Comparator 인터페이스
  - 정렬 대상이 되는 클래스가 구현해야 하는 인터페이스.
  - Comparable은 compareTo() 메서드를 구현. 매개변수와 객체 자신(this)를 비교.
  - Comparator는 compare() 메서드를 구현. 두 개의 매개 변수를 비교. Comparator 사용하려면, TreeSet 생성자에 Comparator가 구현된 객체(Member)를 매개변수로 전달.
  ```java
  TreeSet<Member> ts = new TreeSet<Member>(new Member());
  ```
  - 일반적으로 Comparable을 더 많이 사용. 이미 Comparable이 구현된 경우 Comparator를 이용하여 다른 정렬 방식을 정의할 수 있음.
- Map 인터페이스
  - key-value pair의 객체를 관리하는데 필요한 메서드가 정의됨. 이 때 key는 중복될 수 없음.
  - 검색을 위한 자료구조. key를 이용하여 값을 저장하거나 검색, 삭제할 때 사용하면 편리함.
  - 내부적으로 hash 방식으로 구현됨(`index = hash(key) // index는 저장 위치`).
  - key가 되는 객체는 객체가 유일한지의 여부를 알기 위해 equals()와 hashCode() 메서드를 재정의함.
- HashMap 클래스
  - Map 인터페이스를 구현한 클래스 중 가장 일반적으로 사용되는 클래스.
  - HashTable 클래스는 Vector처럼 동기화를 제공함.
  - 여러 메서드를 활용하여 pair 자료를 쉽고 빠르게 관리할 수 있음.
  - 객체 순회하기: iterator() 메서드는 하나의 Collection 객체만을 반환하므로 pair로 이루어진 객체는 각각 순회해야 함.
- TreeMap 클래스
  - key 객체를 정렬하여 key-value pari를 관리. key에 사용되는 클래스에 Comparable, Comparator 인터페이스를 구현.
  - 단 java의 많은 클래스들은 이미 Comparable이 구현되어 있음. 따라서 구현된 클래스를 key로 사용하는 경우는 구현할 필요 없음.

<br>

## 내부 클래스

<img width="506" alt="image" src="https://user-images.githubusercontent.com/112764753/210189975-80924ff8-2764-48cc-b2ca-bb08d7b773f5.png">

- 인스턴스 내부 클래스
  - static 멤버 변수나 메서드 사용 불가. 인스턴스 생성 후 사용하는 클래스니까.
- 정적 내부 클래스
  - 멤버 변수 사용 불가. static 클래스니까 생성 안 된 외부 클래스의 멤버 변수는 사용할 수 없지.

<br>

## 내부 클래스 - 람다식

- 람다식(lambda expression)
  - 자바 8부터 지원
  - 자바에서 함수형 프로그래밍을 구현하는 방식. 클래스를 생성하지 않고 함수의 호출만으로 기능을 수행.
  - 함수형 프로그래밍
    : 함수를 기반으로 구현. 순수 함수(pure function)을 구현하고 호출함으로써 외부 자료에 부수적인 영향을 주지 않고 매개 변수만을 사용. 입력 받은 자료를 기반으로 수행되고 외부에 영향을 미치지 않으므로 병렬처리 가능. 안정적인 확장성 있는 프로그래밍 방식.
- 람다식 구현하기
  - 익명 함수 만들기
  - 매개 변수와 매개 변수를 활용한 실행문으로 구현: `(매개변수)->{실행문;}`
  - 함수 이름과 반환형을 없애고 ->을 사용. {}까지 실행문을 의미.
    : `(int x, int y) -> {return x + y;}`

<img width="457" alt="image" src="https://user-images.githubusercontent.com/112764753/210190866-56b8e232-a4f4-4a26-97e1-d9680b874921.png">

<img width="478" alt="image" src="https://user-images.githubusercontent.com/112764753/210191047-061f78e9-1980-4acc-aa45-ffb9d8921e7f.png">

<img width="516" alt="image" src="https://user-images.githubusercontent.com/112764753/210191215-0ed7e61a-195e-4abf-8604-373c76fda0cc.png">

<img width="436" alt="image" src="https://user-images.githubusercontent.com/112764753/210191405-0458ed30-af4c-4f7f-b211-77a7d18a50ae.png">

<img width="513" alt="image" src="https://user-images.githubusercontent.com/112764753/210191458-7ff112ea-3162-4589-826c-7186ccd56372.png">

<img width="506" alt="image" src="https://user-images.githubusercontent.com/112764753/210191484-e62454c3-c4c2-4d29-9f85-f03da85bbc7a.png">

<br>

## 내부 클래스 - 스트림

<img width="524" alt="image" src="https://user-images.githubusercontent.com/112764753/210195035-fe5bb008-515b-4923-a4dc-79ea4792fb76.png">

<img width="517" alt="image" src="https://user-images.githubusercontent.com/112764753/210195075-4fa53910-3277-4f9d-9ded-ca6cf678cd9b.png">

- 정수 배열과 ArrayList에 스트림 생성하고 사용하기

<img width="662" alt="image" src="https://user-images.githubusercontent.com/112764753/210195430-ff7c72a7-2a8c-4658-b9dd-11d701614808.png">

<img width="557" alt="image" src="https://user-images.githubusercontent.com/112764753/210195490-ab010af6-d4b1-46a0-a6dc-48701fbc293a.png">

<img width="479" alt="image" src="https://user-images.githubusercontent.com/112764753/210195593-8590c889-0d10-4908-80eb-22662f5bc6e5.png">

<br>

## 예외 처리

강의 듣는 지금은 딱히 와닿지 않아서 대충 작성함.

- 오류
  - 컴파일 오류(compile error): 프로그램 코드 작성 중 발생하는 문법적 오류.
  - 실행 오류(runtime error): 실행 중인 프로그램이 의도하지 않은 동작을 하거나(bug) 프로그램이 중지되는 오류.
  - 실행 오류 시 비정상 종료는 서비스 운영에 치명적.
  - 오류가 발생할 수 있는 경우에 로그를 남겨 추후 이를 분석하여 원인을 찾아야 함. 자바는 예외 처리를 통해 프로그램의 비정상 종료를 막고 log를 남길 수 있음.
- Error와 Exception 클래스
  - 시스템 오류: 가상 머신에서 발생. 프로그래머가 처리할 수 없음. 동적 메모리가 없는 경우, 스택 오버 플로우 등
  - 예외: 프로그램에서 제어할 수 있는 오류. 읽어 들이려는 파일이 존재하지 않는 경우, 네트워크 연결이 끊어진 경우.
- throw와 throws: 전자는 예외 발생, 후자는 예외 미룰 때 사용(try-catch 구문 안 쓸 때 예외 미루는 처리). throws에서 미룬다는 뜻은 예외가 발생한 메서드에서 예외 처리하지 않고, 이 메서드를 호출한 곳에서 예외 처리를 한다는 의미.
- 사용자 정의 예외

<br>

## 자바 입출력(1)

- 스트림
  - 네트워크에서 자료의 흐름이 물과 같다는 의미에서 유래.
  - 다양한 입출력 장치에 독립적으로 일관성 있는 입출력을 제공하는 방식.
- 스트림의 구분
  - 대상 기준: 입력 스트림 / 출력 스트림
  - 자료의 종류: 바이트 스트림 / 문자 스트림
  - 기능: 기반 스트림 / 보조 스트림

<img width="429" alt="image" src="https://user-images.githubusercontent.com/112764753/210199987-1c80d357-eb3d-4b0b-8c13-fd30db5b4f9c.png">

<img width="432" alt="image" src="https://user-images.githubusercontent.com/112764753/210200066-419b615a-af86-4f3b-bfb0-5d200154f0f3.png">

<img width="541" alt="image" src="https://user-images.githubusercontent.com/112764753/210200142-7b5103c9-67c1-43ad-9898-a6045fb90de5.png">

- 표준 입출력
  - System.out: 표준 출력(모니터) 스트림
  - System.in: 표준 입력(키보드) 스트림
  - System.err

<img width="582" alt="image" src="https://user-images.githubusercontent.com/112764753/210200548-10e08b81-5088-40fa-90d3-ab305cf8b5d4.png">

- 바이트 단위 스트림(InputStream)
  - 바이트 단위 입력용 최상위 스트림. 추상 클래스로 하위 클래스가 구현하여 사용.
  - 주요 하위 클래스
    - FileInputStream
    - ByteArrayInputStream
    - FilterInputStream
  - 주요 메서드: read(), close()
- 바이트 단위 스트림(OuputStream)
  - 바이트 단위 출력용 최상위 스트림. 추상 클래스로 하위 클래스가 구현하여 사용.
  - 주요 하위 클래스
    - FileOutputStream
    - ByteArrayOutputStream
    - FilterOutputStream
  - 주요 메서드: write(), flush(), close()
- flush()와 close() 메서드
  - 출력을 위해 잠시 자료가 머무르는 출력 버퍼를 강제로 비우고 자료를 출력할 때 flush().
  - close() 메서드 내부에서 flush()가 호출되므로 close() 메서드가 호출되면 출력버퍼가 비워짐.
- 문자 단위 스트림(Reader)
  - 문자 단위로 읽는 최상위 스트림.
  - 하위 클래스에서 상속받아 구현함.
  - 주요 하위 클래스
    - FileReader
    - InputStreamReader
    - BufferedReader: 문자로 읽을 때 배열을 사용하여 한꺼번에 읽을 수 있는 기능을 제공해주는 보조 스트림.
  - 주요 메서드: read(), close()
- 문자 단위 스트림(Writer)
  - 문자 단위로 쓰는 최상위 스트림.
  - 하위 클래스에서 상속받아 구현함.
  - 주요 하위 클래스
    - FileWriter
    - InputStreamWriter
    - BufferedWriter: 문자로 쓸 때 배열을 사용하여 한꺼번에 쓸 수 있는 기능을 제공해주는 보조 스트림.
  - 주요 메서드: write(), flush(), close()

<br>

## 자바 입출력(2)

<img width="488" alt="image" src="https://user-images.githubusercontent.com/112764753/210202720-3600402b-2d37-4145-8805-69108d76df55.png">

<img width="482" alt="image" src="https://user-images.githubusercontent.com/112764753/210203058-60146859-9635-4c71-90db-01298f405e63.png">
- 확실 X(100% 내 추측)
  - 보조 스트림 생성자로 생성할 때, 그 생성자 매개변수에 보조/기반 스트림을 넣어주고, 마지막에는 무조건 기반 스트림을 넣는 걸로 보임.
  - ex) `BufferedReader = new BufferedReader(new InputStreamReader(System.in));`
  : BufferedReader, InputStreamReader는 보조 스트림. System.in은 표준 입력 스트림(아마 기반 스트림).

<img width="452" alt="image" src="https://user-images.githubusercontent.com/112764753/210203849-6842de13-d60a-47b9-9819-b23e01289c2f.png">

<img width="436" alt="image" src="https://user-images.githubusercontent.com/112764753/210204319-7a9a993d-d03f-4ad3-9b16-74c567b9fe7e.png">

<img width="551" alt="image" src="https://user-images.githubusercontent.com/112764753/210204889-25b7084b-2569-477a-bbc7-5ed5ceffdfb7.png">

- Serializable 인터페이스(마커 인터페이스)
- Externalizable 인터페이스
- RandomAccessFile 클래스
