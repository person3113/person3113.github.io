---
title: "[Do it! 자바 프로그래밍 입문] <2. 자바의 핵심 - 객체지향 프로그래밍>의 <클래스와 객체1 (4)>부터 <객체 배열 사용하기(2)>까지"
excerpt: "클래스와 객체1 / 클래스와 객체2 / 배열"

categories:
  - JavaLecture
tags:
  - [Java]

toc: true
toc_sticky: true
toc_label: "페이지 주요 목차"

date: 2022-12-28
last_modified_at: 2022-12-28
---

<br>

# [인프런 강의 주소](https://www.inflearn.com/course/%EC%9E%90%EB%B0%94-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D-%EC%9E%85%EB%AC%B8#curriculum)

<br>

# 2. 자바의 핵심 - 객체지향 프로그래밍

## 클래스와 객체1 (4)

- 참조 자료형
  - 클래스로 제공되는 자료형(클래스로 선언된 자료형)
  - ex) String와 Data(라이브러리 클래스), Student(사용자 정의 클래스)
- 참조 자료형 활용

  - 원의 중심점을 Point라는 참조 자료형으로 선언. 이 때 생성자를 통해해 참조 변수의 객체를 생성함(왜냐면 클래스는 선언만 한다고 생기지 않기 때문)
  - Point.java

  ```java
  public class Point {
  	int x;
  	int y;
  }
  ```

  - Circle.java

  ```java
  public class Circle {
  	Point point;
  	int radius;

  	public Circle(){
  		point=new Point();
  	}
  }
  ```

- 정보 은닉(information hiding)

  - public 접근 제한자: 모든 패키지 내 모든 클래스에서 아무런 제한 없이 멤버 변수와 메소드를 사용할 수 있도록 함.
  - default 접근 제한(접근 제어자 아무것도 안 쓰는 경우): 같은 패키지에 소속된 클래스에서만 사용할 수 있도록 함.
  - private 접근 제한자: 동일한 패키지 내 클래스일지라도, 멤버 변수와 메소드를 사용하지 못하도록 제한. 오로지 클래스 내부에서만 사용할 수 있음.
  - 클래스 내 멤버 변수를 설정하거나 가져올 때는 직접 변수에 접근해서 사용하지 말고, get(), set() 메소드를 통해 사용하는 것이 오류를 줄일 수 있음

    - 즉, 멤버 변수에 직접 접근하는 것은 private으로 막아놓고, 변수와 관련해서 접근하고 싶으면 관련 get, set 등의 메소드를 public으로 오픈해.
    - 예시

    ```java
    class Birth {
      private int year;
      private int month;
      private int day;

      // get() 메소드
      public int getYear() {
        return year;
      }
      public int getMonth() {
        return month;
      }
      public int getDay() {
        return day;
      }

      // set() 메소드
      public void setYear(int year) {
        this.year = year;
      }
      public void setMonth(int month) {
        this.month = month;
      }
      public void setDay(int day) {
        this.day = day;
      }
    }
    ```

    - 그리고 오픈한 메소드에서 예외 처리 구문을 작성하면, month에 13과 같은 이상한 값을 넣는 오류를 막을 수 있음.

    ```java
    public void setMonth(int month) {
      if(month < 1 || month > 12)
        System.out.println("이상한 값을 집어넣으려 했습니다.")
      else
        this.month = month;
    }
    ```

<br>

## 클래스와 객체2 (1)

- this 키워드

  - 생성된 인스턴스 스스로를 가리키는 예약어
    - 즉 인스턴스가 생성된다는 뜻은 메모리에 생긴다는 뜻. 이 때 자기 자신의 메모리 주소를 가리키는 게 this.
    - 참고: 참조 변수를 통해 멤버 변수나 메소드를 쓰면(ex- Student.id), 참조 변수가 가리키는 주소를 참조해서 되는 것이다.
    - 따라서 클래스 내에 아래 예시처럼 this를 반환하고, 이를 main함수에서 `Student s1 = new Student()`한 후 `s1.returnSelf()`를 출력하면 그 객체의 주소값이 출력됨.(자신의 주소를 반환함)
    ```java
    public Student returnSelf(){
    	return this;
    }
    ```
  - 생성자에서 다른 생성자를 호출

    - 매개변수 없는 생성자를 호출하면 this.name에는 "이름 아직 안 받았음"을, this.age에는 0을 넣고 싶을 때가 있음. 이 때 각각 대입하려고 쓰기에 귀찮고, 이미 다른 생성자를 이용하면 한 줄로 설정 가능. 이처럼 다른 생성자를 호출할 떼 this를 이용

    ```java
    public Student (){
    	this("이름 아직 안 받았음", 0);
    }

    public Student (String name, int age){
    	this.name = name;
    	this.age = age;
    }
    ```

    - this를 이용해서 다른 생성자를 호출할 때는 그 앞에 어떤 구문도 작성되면 안된다. 왜냐하면 this()를 통해 다른 생성자 호출이 완료되야만 인스턴스가 확실히 생성됨. 그런데 그 이전에 name = "이름 아무거나"와 같이 넣으면, 생성하지 않은 곳에 메모리를 할당하는 등의 오류가 발생할 수 있음. 즉 아래 예시는 오류임.

    ```java
    public Student (){
    	name = "이름 아무거나"; // 오류!!
    	this("이름 아직 안 받았음", 0);
    }
    ```

<br>

## 클래스와 객체2 (2)

딱히 메모할 건 없었다.

<br>

## 클래스와 객체2 (3) - static 변수

- static 변수(클래스 변수)
  - 여러 개의 인스턴스가 같은 메모리의 값을 공유하기 위해 사용. 다르게는 클래스에 고정된 변수이다.
  - 따라서 인스턴스 생성과 관계없이 클래스 변수는 사용 가능하고, 일반적으로 클래스 이름으로 클래스 변수 참조.
  - ex) `s1.stdNum`처럼 인스턴스 이름으로 static 변수 참조하진 않음. `Student.stdNum`처럼 클래스 이름으로 참조
- static 메소드(클래스 메소드)
  - 메소드 앞에 static 키워드 사용해서 구현
  - 주로 static 변수를 위한 기능을 제공
  - 활용: static 변수는 공용으로 쓰여서 바뀌면 모든 객체가 영향을 받으니까 민감하니 private으로 설정. 그리고 이를 접근할 때는 static 메소드로 하고.
  ```java
  private static int stdNum = 1000;
  	public static int getStdNum(){
  		return stdNum;
  }
  ```
  - static 메소드도 인스턴스의 생성과 관계없이 클래스 이름으로 직접 메소드 호출
  - ex) staic 메소드도 `s1.getStdNum()`가 아니라 `Student.getStdNum()`처럼 클래스 이름으로 접근하는 게 일반적
- static 메소드와 멤버 변수
  - static 메소드에서 인스턴수 변수(멤버 변수)는 사용할 수 없음. 왜냐하면 클래스 메소드는 인스턴스가 생성 안 되도 사용 가능. 그런데 인스턴스 변수의 경우 꼭 인스턴스가 먼저 생성되어야 해. 따라서 생성되지도 않은 인스턴스 변수에 대입을 하는 등의 문제가 발생 가능.

<br>

## 클래스와 객체2 (4) - singleton 패턴

- singleton 패턴

  - 인스턴스는 기본적으로 여러 개 생성 가능. but 이 프로그램에서 이 객체는 여러 개면 안되고, 하나만 생성되야 할 때 이 패턴을 쓴다.
  - 구현

    - static 활용(여러 인스턴스에서 공통된 변수니까)
    - 생성자는 private으로 설정(클래스 내부에서만 생성자를 이용해서 인스턴스를 한 개 만들고 끝내는 것이다. 이렇게 안하면 default 생성자가 쓰이는데, 이는 public이라서 외부에서 여러번 생성자를 호출해서 여러 개를 만들 수 있다.)
    - public으로 선언된 static 메소드를 제공해야 한다.(다른 클래스에서 static으로 설정된 객체를 사용해야 하니까)
    - ex)

    ```java
    public class Company {
    	// static 활용
    	private static Company onlyOne = new Company();

    	// 생성자는 private으로 설정
    	private Company(){}

    	// public으로 선언된 static 메소드를 제공
    	public static Company getOnlyOne(){
    		return onlyOne;
    	}
    }
    ```

  - 사용: `Company c1 = new Company`처럼 일반적인 방법은 안되고, `Company c1 = Company.getOnlyOne();`처럼 객체를 생성해야 함.'
  - `Company c1 = new Company`, `Company c2 = new Company`의 경우 서로 다른 각각의 객체가 생성됨. 그러나 `Company c1 = Company.getOnlyOne();`, `Company c2 = Company.getOnlyOne();`의 경우 c1과 c2는 서로 다른 객체가 아니라, Company라는 클래스 속에 존재하는 같은 객체를 뜻함.

<br>

## 배열과 ArrayList(1)

딱히 메모할 부분은 없었다.

<br>

## 객체 배열 사용하기(2)

- 객체 배열
  - 기본 자료형 배열과 참조 자료형 배열(객체 배열): ex) `int[] nums = new int[5];` / `Book[] library = new Book[5];`
  - 객체 배열만 생성한 경우 요소는 null로 초기화됨. 각 요소를 new를 통해 생성하여 저장해야 함.
    - 즉 객체 배열을 선언하면(`Book[] library = new Book[5];`) 책 5권이 만들어지는 것이 아님. 책이 만들어질 건데, 책을 가리킬 주소를 담을 자리(Book 인스턴스를 저장한 메모리 주소를 담는 자리)가 5개 만들어지는 것임.
    - `library[0] = new Book("어린왕자", "생택쥐페리");`처럼 각 요소를 new를 통해 인스턴스를 생성해야 함.
- 배열 복사

  - `System.arraycopy(src, srcPos, dest, destPos, length);`
    - src: 복사할 배열 이름
    - srcPos: 복사할 배열의 첫 번째 위치
    - dest: 복사해서 붙여 넣을 대상 배열 이름
    - edstPos: 복사해서 대상 배열에 붙여넣기를 시작할 첫 번
    - length: src에서 dest로 자료를 복사할 요소 개수
  - 기본 자료형 배열 복사 예시

  ```java
  int[] a1 = {10, 20, 30};
  int[] a2 = {1, 2, 3};
  System.arraycopy(a1, 0, a2, 1, 2);
  // a2는 복사 후 {1, 10, 20}으로 바뀜
  ```

  - 객체 배열 복사

    - 얕은 복사(System.arraycopy): 복사할 때 배열 요소의 값이 복사된 게 아니라, 주소가 복사됨. 만약 a1 객체 배열에서 a2 객체배열로 복사했고, a1[0]의 객체 정보를 수정하면, 자동으로 a2[0]의 객체 정보도 바뀜. 즉 두 배열의 각 요소가 같은 인스턴스를 가리키고 있는 상태.

      <img width="500" alt="image" src="https://user-images.githubusercontent.com/112764753/209825103-29253299-85e8-4be9-93fe-5229bb2d051f.png">{: .align-center}

    - ex)

    ```java
    Book[] a1 = new Book[2];
    Book[] a2 = new Book[2];

    a1[0] = new Book("책1", "작가1");
    a1[1] = new Book("책2", "작가2");

    System.arraycopy(a1, 0, a2, 0, 2);
    ```

    - 깊은 복사: 두 개의 배열이 서로 다르게 가리킬 필요가 있을 때. a2의 각 요소에 인스턴스를 생성해서 a1과 a2의 각 요소가 별개의 인스턴스가 되도록 함. 앞 예시에서는 `a2[0] = new Book();`과 같이 a2 배열 요소에 각 인스턴스를 생성하지 않고 복사해서, a2가 a1의 인스턴스를 가리킨 상황이었음.

    <img width="500" alt="image" src="https://user-images.githubusercontent.com/112764753/209826912-b6fdecfa-c3e1-42d7-96ee-77b19f102373.png">{: .align-center}

    ```java
    Book[] a1 = new Book[2];
    Book[] a2 = new Book[2];

    a1[0] = new Book("책1", "작가1");
    a1[1] = new Book("책2", "작가2");

    // a2로 깊은 복사할 거면
    a2[0] = new Book();
    a2[1] = new Book();

    for(int i = 0; i < a1.length; i++){
    	a2[i].setBookName(a1[i].getBookName());
    	a2[i].setAuthor(a1[i].getAuthor());
    }
    ```

- 향상된 for문

  - 배열 요소의 처음부터 끝까지 모든 요소를 참조할 때 편리한 for문
  - ex)

  ```java
  String[] s = {"element 1", "element 2", "element 3"};

  for(String str : s){
  	System.out.println(str);
  }
  // 출력 결과:
  // element 1
  // element 2
  // element 3
  ```
