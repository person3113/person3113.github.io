---
title: "[Do it! 자바 프로그래밍 입문] '자바 프로그래밍 시작하기' ~ '자바 입출력(2)'"
excerpt: "자바 전반을 다지는 개념 강의를 요약해서 기록"

categories:
  - Learning-Java
tags:
  - []

toc: true
toc_sticky: true
toc_label: "페이지 주요 목차"

date: 2022-12-26
last_modified_at: 2023-01-24
---

<br>

# [인프런 강의 주소](https://www.inflearn.com/course/%EC%9E%90%EB%B0%94-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D-%EC%9E%85%EB%AC%B8#curriculum)

<br>

# 보는 방법

블로그에 메모한 내용만으로 복습하기에는 무리. 복습할 땐 강의 영상 보고, 이건 상기용/테스트용. 메모한 거 보면서 강의 내용이 떠오르는지 등을 점검하는 데 활용.

<br>

# 1. 자바 기본 익히기

## 자바 프로그래밍 시작하기

- 바이트 코드
  - 자바 소스 코드는 컴파일되면 바이트 코드로 바뀐다.
  - 바이트 코드는 각 컴퓨터의 자바 가상 머신에서 실행된다.(OS와 무관하게 가상 머신이 깔려 있기만 하면 된다.)
  - 반면 C같은 소스 코드는 각 OS에 따라 각기 다른 실행 파일이 만들어져서, 다른 OS에서 실행하려면 다시 컴파일해야 한다는 점이 자바와의 차이점이다.

<br>

## 변수와 자료형 (1) ~ (3)

### 자료형

- 자료형: 변수가 사용할 메모리의 크기와 타입을 구분하기 위해서 자료형을 사용
- long형
  - `int num = 123456789987654321`는 4바이트 int의 범위를 벗어나서 이를 8바이트인 long형 변수에 저장해야 한다.
  - 단 `long num = 123456789987654321;` (X) / `long num = 123456789987654321l;` (O) / 숫자 뒤에 l(소문자 L)을 붙여야지 오류 안 남
- char형
  - 자바에서는 문자를 2바이트로 처리
  - 인코딩과 디코딩: 인코딩은 문자에 특정한 숫자값(코드 값) 을 부여하는 것 / 디코딩은 특정 숫자 값에서 문자로 변환하는 것
    - 문자 세트: 문자를 위한 숫자 값(코드 값)을 정해놓은 세트
      - 아스키(ASCII): 1바이트로 영문자, 숫자, 특수문자 등을 표현함
      - 유니코드(Unicode): 한글과 같은 대부분의 세계 언어를 표현하기 위한 표준 인코딩
    - `char ch = 'A'`: 문자를 변수에 저장하면, 문자에 해당하는 코드 값이 저장됨(인코딩돼서 문자 변수에 저장됨)
    - (내 추측, 확실X)`int ch = 65; System.out.println((char)ch)`: ch에 저장된 65라는 숫자가 출력될 때 디코딩돼서 출력되는 느낌
- double/float형(8바이트/4바이트)
  - `float f = 3.14` (x) / `float f = 3.14f` (O) / 실수 쓸 땐 기본적으로 double형이라서, float으로 쓸 때는 반드시 뒤에 f를 추가해야 한다.
- boolean형: 논리형, 1바이트
- 형 변환
  - ex) `int num = (int)3.14;`

### 상수와 리터럴

- 상수: 변하지 않는 값(다른 값 대입 불가, 프로그램 내에서 변경되지 말아야 하는 값)
  - 상수 선언: final 키워드 사용
  - 관습적으로 상수 이름은 대문자로 표현
  - ex) `final double PI = 3.14;`
- 리터럴: 프로그램에서 사용하는 모든 값들
  - ex) 10, 3.14, 'A', true 등
  - 리터럴에 해당되는 값은 특정 메모리 공간인 상수 풀(constant pool)에 있음
  - ex) `final double PI = 3.14;`에서 3.14라는 실수가 '어딘가(상수 풀)'에 저장되어 있다. 그 저장되어 있는 값이 복사돼서 PI라는 상수값이 됨
  - 상수 풀에 저장할 때 정수는 int로 실수는 double로 저장된다. 따라서 long이나 float값으로 저장해야 하는 경우 식별자(l,L,F,f)를 명시해야 한다.

![image](https://user-images.githubusercontent.com/112764753/209525399-e6486f1c-3960-4acc-a592-692c7863e627.png){: .align-center}

### 형 변환(type conversion & type casting)

- 형 변환
  - 묵시적 형 변환(type conversion)
    - 덜 정밀한 수에서 더 정밀한 수로 대입될 때(혹은 덜 정밀한 수와 더 정밀한 수가 연산될 때)
    - `long num = 3;`: int에서 long으로 자동 형 변환
    - `double d = 3.4f + 3;`: 3(int형)과 3.4f(float형)이 + 연산이 일어나면 int형이 float형으로 자동 형 변환. 그 후 생성된 값이 double형 변수에 대입될 때 float형에서 double형으로 자동 형 변환됨.
  - 명시적 형 변환(type casting)
    - 더 정밀한 수에서 덜 정밀한 수로 대입될 때, 변환되는 자료형을 명시해야 함.
    - 왜 명시해야 함? 자료의 손실이 발생할 수 있기 때문에, 컴파일러가 임의로 형 변환하지 못함.
    - `int n = (int) 3.14;`: double형이 int형으로 변환되면서, 소수점 이하 부분이 유실됨
  - What is difference between type casting and type conversion?
    - In type casting, a data type is converted into another data type by a programmer using casting operator.
    - Whereas in type conversion, a data type is converted into another data type by a compiler.

<br>

## 자바의 여러 가지 연산자 (1) ~ (2)

- 논리 연산자의 단락 회로 평가(short circuit evaluation)
  - 논리 곱(&&)은 앞의 항이 false이면, 뒤 항의 결과를 평가하지 않아도 false임을 알 수 있음. 왜냐면 두 항이 모두 true일 때만 결과가 true이기 때문
  - 논리 합(||)은 앞의 항이 treu이면, 뒤 항의 결과를 평가하지 않아도 true임을 알 수 있음. 왜냐면 두 항이 모두 false일 때만 결과가 false이기 때문
- 복합 대입 연산자
  - 복합 대입 연산자는 오른쪽에 있는 식을 "먼저" 계산하여, 그 결과를 대입한다.
  - `n = 5; n %= 3 + 5`
    - 위 식은 `n = n % 3 + 5`가 아니다. 즉 n은 7이 아니라는 소리다.
    - 위 식은 `n = n % (3 + 5)`와 같다. 따라서 n은 5가 된다.

<br>

## 제어 흐름 이해하기 (1) ~ (2)

- 자바 7부터 switch-case문의 case 값에 문자열 사용 가능

```java
public class Main {

    public static void main(String[] args) {
        String medal = "gold";

        switch (medal) {
            case "gold":
                System.out.println("gold");
                break;
            case "silver":
                System.out.println("silver");
                break;
            case "bronze":
                System.out.println("bronze");
                break;
            default:
                System.out.println("null");
                break;
        }
    }

}
```

- 자바도 do-while문이 있다. 학교에서 배운 c의 do-while문과 똑같이 쓰면 된다.

<br><br>

# 2. 자바의 핵심 - 객체지향 프로그래밍

## 클래스와 객체1 (1)

- 객체의 속성과 특성, 그리고 객체가 하는 기능들은 클래스의 멤버 변수, 그리고 메서드(멤버 함수)로 구현한다.
- 클래스 정의하기
  - class 이름은 대부분 대문자로 시작(관습적으로)
  - 하나의 java 파일에 하나의 클래스를 두는 것이 원칙이나, 여러 개의 클래스가 같이 있는 경우 public 클래스는 단 하나여야 한다. 또 public 클래스와 자바 파일의 이름은 동일해야 한다.
  - 자바의 모든 코드는 class 내부에 위치(import나 package 구문 제외하고 대부분은)
- 패키지
  - 패키지는 비슷한 성격의 자바 클래스들을 모아 놓은 자바의 디렉토리이다.
  - 특정 패키지에서 클래스를 생성하면 package 구문이 자동으로 삽입된다. package 키워드는 이 파일이 어떤 패키지의 파일인지를 알려주는 역할을 한다.
  - ex) house 패키지에서 클래스를 생성하면 다음처럼 `package house;`와 같은 문장이 자동으로 삽입된다.
- import
  - 같은 패키지 내에서 다른 클래스를 사용할 때는 import할 필요 없다. 하지만 다른 패키지 속 클래스를 사용하고 싶을 때는 import를 해야 한다.
  - ex) `import house.HouseKim;` / house 패키지 내에 HouseKim이라는 클래스를 사용하기 위해 import함

<br>

## 클래스와 객체1 (2)

- 함수와 스택 메모리
  - 스택 메모리: 함수가 호출될 때 그 함수의 지역변수가 사용하는 메모리
  - 함수가 호출되면, 스택 메모리가 올라가고, 함수에 쓰는 지역변수가 생성됨
  - 함수의 기능이 끝나면 자동으로 반환되는 메모리(즉 지역변수니까 함수 기능 다 수행했으면 다시 소멸됨)

<img alt="stack_memory" src="https://user-images.githubusercontent.com/112764753/214052953-9c2e79a4-a52e-4dbf-9191-ff7e8415809e.png">

<br>

## 클래스와 객체1 (3)

- 인스턴스와 힙(heap) 메모리
  - 인스턴스는 힙 메모리에 생성됨(즉 힙 메모리는 new라는 키워드에 의해 생성됨)
  - 각각의 인스턴스는 다른 메모리에 다른 값을 가짐
- 참조 변수
  - 인스턴스가 생성된 것이 대입된 변수를 참조 변수라고 한다.(정확히는 메모리에 생성된 인스턴스를 가리키는 변수)
  - 참조 값: 생성된 인스턴스의 힙 메모리 주소 값
  - ex) `Student s = new Student();` / new Student()로 Student 클래스의 인스턴스가 메모리에 생성됨. -> 생성된 인스턴스의 주소 값을 참조 값이라 하고, 이 주소 값을 갖는 변수 s가 참조 변수다.
- 생성자
  - 인스턴스 생성 시 new 키워드와 함께 사용한 함수(주로 인스턴스를 초기화할 때의 명령어 집합)
  - 생성자의 이름은 그 클래스의 이름과 같음. 또 생성자를 정의할 때 반환값은 없음(void가 아니라 아예 안 씀)
  - 생성자는 메소드가 아님. 상속되지 않음.
    - ex) `Student s = new Student();`에서 Student()
  - 디폴트 생성자: 클래스 속에 사용자가 정의한 생성자가 없으면 컴파일러가 자동으로 넣어줌
    - 특징: 매개변수, 안의 내용 전부 없음
    - ex) `public Student(){}`
  - 사용자가 객체 생성될 때 특정 동작도 동시에 실행되면 좋겠다 싶으면 다음처럼 정의하면 됨
    - ex)
    ```java
    public Student(int id){
    	studentId = id;
    }
    ```
- 오버로딩
  - 어떤 메소드 또는 생성자 이름 하나로 여러 함수를 구현할 경우(여러 개의 같은 이름의 메소드 또는 생성자가 하나의 클래스에 있을 때)
  - 조건: 메소드 이름은 같되, 매개변수의 개수나, 타입이 달라야 한다.
  - ex) 하나의 클래스 안에 `public Student(){}` 생성자와 `public Student(int id){~ 구현 ~}` 생성자가 동시에 있을 경우
    - 위 예시처럼 되어있다면, `Student s = new Student();`나 `Student s = new Student(id값);` 중 아무거나 써도 된다.

<br>

## 클래스와 객체1 (4)

- 참조 자료형(reference data type)
  - 클래스로 제공되는 자료형(클래스 형으로 선언하는 자료형)
  - ex) String과 Date(라이브러리 클래스), Student(사용자 정의 클래스)
- 참조 자료형 활용

  - 원의 중심점을 Point라는 참조 자료형으로 선언. 이 때 생성자를 통해 참조 변수의 객체를 생성함(왜냐면 클래스는 선언만 한다고 생기지 않기 때문)
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

  - 클래스의 외부에서 클래스 내부의 멤버 변수나 메서드에 접근하지 못하게 하는 경우 사용
  - 클래스 내 멤버 변수를 설정하거나 가져올 때는 직접 변수에 접근해서 사용하지 말고, get(), set() 메소드를 통해 사용하는 것이 오류를 줄일 수 있음
  - public 접근 제한자: 모든 패키지 내 모든 클래스에서 아무런 제한 없이 멤버 변수와 메소드를 사용할 수 있도록 함.
  - default 접근 제한(접근 제어자 아무것도 안 쓰는 경우): 같은 패키지에 소속된 클래스에서만 사용할 수 있도록 함.
  - private 접근 제한자: 동일한 패키지 내 클래스일지라도, 멤버 변수와 메소드를 사용하지 못하도록 제한. 오로지 클래스 내부에서만 사용할 수 있음.

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

    - this를 이용해서 다른 생성자를 호출할 때는 그 앞에 어떤 구문도 작성되면 안된다. 왜냐하면 다음과 같은 경우가 발생할 수 있기 때문이다.
    - 아래에서 name은 멤버 변수고, 인스턴스가 생성되야 만들어지는(메모리가 잡히는) 변수다. 하지만 this()를 통해 다른 생성자 호출이 완료되야만 인스턴스가 확실히 생성됨. 그런데 그 이전에 name = "이름 아무거나"와 같이 넣으면, 생성되지 않은 메모리에 값을 대입하는 오류가 발생할 수 있음. 즉 아래 예시는 오류임.

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
  - 프로그램이 메모리에 적재(load)될 때 데이터 영역의 메모리에 생성됨.
  - 따라서 인스턴스 생성과 관계없이 클래스 변수는 사용 가능하고, 일반적으로 클래스 이름으로 클래스 변수 참조.
  - ex) `s1.stdNum`처럼 인스턴스 이름으로 static 변수 참조하진 않음. `Student.stdNum`처럼 클래스 이름으로 참조
- static 변수의 예
  - 학생이 생성될 때마다 학번이 증가해야 하는 경우, 기준이 되는 값은 static 변수로 생성하여 유지함.
  - 각 학생이 생성될 때마다 static 변수 값을 복사해 와서 하나 증가시킨 값을 생성된 인스턴스의 학번 변수에 저장해 줌.
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

  - 인스턴스는 기본적으로 여러 개 생성 가능. but 이 프로그램에서 이 객체는 여러 개면 문제가 되고, 하나만 생성되야 할 때 이 패턴을 쓴다.
  - 구현

    - static 활용(여러 인스턴스에서 가리키는 공통된(단 하나의) 변수니까)
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

  - 사용: `Company c1 = new Company()`처럼 일반적인 방법은 안되고, `Company c1 = Company.getOnlyOne();`처럼 객체를 생성해야 함.'
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
    - edstPos: 복사해서 대상 배열에 붙여넣기를 시작할 첫 번째 위치
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

    - 깊은 복사: 두 개의 배열이 서로 다르게 가리킬 필요가 있을 때. a2의 각 요소에 인스턴스를 생성해서 a1과 a2의 각 요소가 별개의 인스턴스가 되도록 함. 앞 예시에서는 `a2[0] = new Book();`을 통해 a2 배열 요소에 각 인스턴스를 생성하지 않고 복사해서, a2가 a1의 인스턴스를 가리킨 상황이었음.

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

<br>

## 다차원 배열(3)

- `int[][] a = new int[2][3];`일 때 `a.length`는 행의 길이를 나타내서 2를 뜻함.(전체 원소 갯수인 6이 아님)

<br>

## ArrayList 클래스(4)

- ArrayList 클래스

  - 기존 배열은 길이가 고정되어 있어서, 사용 중 부족한 경우 다른 배열로 복사하는 코드를 직접 구현해야 함. 또 삭제와 십입의 경우도 이를 스스로 코드로 구현해야 함.
  - ArrayList 클래스는 객체 배열이 구현된 클래스. 여러 메소드와 속성을 사용해서 객체 배열을 편리하게 관리 가능

  - ex)

  ```java
  // 어떤 객체 타입을 쓸 건지 <> 안에 표시
  ArrayList<Book> list = new ArrayList<Book>();
  list.add(new Book("책 이름", "작가"));

  for(int i = 0; i < list.size(); i++){
  	// 평범한 배열처럼 a[i]처럼 인덱스로 직접 접근하는 것은 ArrayList에서는 지원하지 않음.
  	// get 메소드를 통해 배열의 i번째 인덱스에 있는 요소를 가져올 수 있음
  	System.out.println(list.get(i));
  }
  ```

<br>

## 상속과 다형성(1)

- 상속(inheritance)

  - 이미 구현된 클래스를 상속받아서 속성과 기능이 확장되는 클래스를 구현
  - 상속하는 클래스는 상위 클래스(parent class, base class, super class), 상속 받는 클래스는 하위 클래스(child class, derived class, sub class)라 부른다.
  - 상위 클래스는 하위 클래스보다 일반적인 의미를 가지고, 하위 클래스는 상위 클래스보다 구체적인 의미를 가짐.
  - ex) 포유류와 사람

  ```java
  class Mammal{}
  class Human extends Mammal{

  }
  ```

  => 이 때 extends 뒤에는 단 하나의 class만 사용할 수 있음. 자바는 single inheritance만 지원함.

  - protected 접근 제어자

    - 기본적으로 default와 비슷하다. 같은 패키지 내에서는 접근 제한이 없지만, 다른 패키지 속 클래스의 경우 기본적으로는 접근이 안된다. 단 다른 패키지 속 클래스라도, 상속받은 클래스(자식 클래스)에 한해서 접근을 허용.
    - ex)

    ```java
    class Customer{
    	protected int ID;
    	protected double bonusRatio;
    }

    class VIPCustomer extends Customer{

    	public VIPCustomer(){
    		ID = "VIP";
    		bonusRatio = 0.05;
    	}
    }
    ```

    - 접근 제어자 범위
      - public: 모든 클래스
      - protected: 같은 패키지 내 클래스에서 접근 가능 + 다른 패키지 내라도 자식 클래스에서는 접근 가능
      - default: 같은 패키지 내 클래스에서 접근 가능
      - private: 동일한 패키지 내 클래스라도 접근 불가. 오직 자신의 클래스 내부에서만 사용 가능.

<br>

## 상속과 다형성(2)

- 상속에서 클래스 생성 과정
  - 하위 클래스가 생성될 때 상위 클래스가 먼저 생성됨. 즉 상위 클래스의 생성자가 호출되고 하위 클래스의 생성자가 호출됨.
  - 하위 클래스의 생성자에서는 무조건 상위 클래스의 생성자가 호출되어야 함. 하위 클래스 생성자 속에 상위 클래스 생성자가 없는 경우, 컴파일러는 상위 클래스의 기본 생성자를 호출하기 위한 super()를 코드에 넣어줌.(super()로 호출되는 생성자는 상위 클래스의 기본 생성자임.)
  - ex) 상위 클래스 기본 생성자가 호출되는 경우
  ```java
  public VIPCustomer() {
  	// 컴파일러가 자동으로 추가하는 코드. 상위 클래스의 생성자 Customer()가 호출됨.
  	super();
  }
  ```
  - 만약 상위 클래스의 기본 생성자가 없는 경우(매개변수가 있는 생성자만 존재하는 경우) 하위 클래스는 명시적으로 상위 클래스를 호출해야 함.
  - ex) 명시적으로 상위 클래스 생성자를 호출하는 경우
  ```java
  public VIPCustomer(int id, String name) {
  	// Customer(int id, Stirng name) 생성자가 호출됨.
  	super(id, name);
  }
  ```
- super 예약어
  - this가 자기 자신의 인스턴스의 주소를 자기는 것처럼, super는 상위 클래스의 주소를 자기게 됨. 하위 클래스가 상위 클래스에 접근할 때 사용할 수 있음.
- 업캐스팅(상위 클래스로의 묵시적 형 변환)
  - 상위 클래스형으로 변수를 선언하고 여기에 하위 클래스 인스턴스를 대입할 수 있음(하위 클래스는 상위 클래스를 내포하고 있으므로 상위 클래스로 묵시적 형 변환이 가능함.)
  - ex) `Customer kim = new VIPCustomer();`
    => VIPCustomer() 생성자의 호출로 VIPCustomer 인스턴스의 변수는 모두 생성되었다. 하지만 변수 kim의 경우 타입이 Customer이다. 따라서 접근할 수 있는 변수나 메소드는 Customer의 변수와 메소드임.

<br>

## 오버라이딩과 다형성(3)

- 메서드 오버라이딩
  - 상위 클래스에 정의된 메서드 중 하위 클래스와 기능이 맞지 않거나 추가 기능이 필요한 경우 사용. 같은 이름과 같은 매개변수로 하위 클래스에서 재정의함.
  - 오버로딩은 같은 이름의 함수가 여러 개라면, 오버라이딩은 덮어 쓰는 것.
- 가상 메서드
  - java는 모든 메서드가 가상 메서드임. c++은 가상 메서드 인 게 있고 아닌게 있지만.
  - 묵시적 형 변환(업캐스팅)된 변수에서 재정의된 메서드를 호출하면, 기존의 메서드가 아니라 재정의된 메서드가 실행됨.
  - ex)
  ```java
  Customer kim = new VIPCustomer();
  kim.clacPrice(10000);
  ```
  - 일반적으로는 어떤 객체의 변수나 메서드의 참조는 그 타입에 따라 이루어짐(`Customer kim = new VIPCustomer();`에서 변수 kim은 Customer 타입이므로 해당 해당 멤버 변수와 메서드만 참조 가능).
  - 하지만 가상 메서드의 경우 타입과 상관없이 실제 생성된 인스턴스의 메서드가 호출되는 원리(`kim.clacPrice(10000);`에서 clacPrice()는 Customer와 VIPCustomer에 둘 다 있는 메서드라면, VIPCustomer의 것이 호출된다는 뜻).
- 다형성(polymorphism)

  - 하나의 코드가 여러가지로 구현되어 실행되는 것. 정보은닉, 상속과 더불어 객체지향 프로그래밍의 가장 큰 특징 중 하나. 객체지향 프로그래밍의 유연성, 재활용성, 유지보수성에 기본이 되는 특징임.
  - 다형성 구현하기(오버라이딩과 가상 메서드): 하나의 클래스를 상속받은 여러 클래스가 있다면, 클래스마다 같은 이름의 서로 다른 메서드를 재정의한다(오버라이딩). 상위 클래스 타입으로 선언된 하나의 변수에 여러 인스턴스가 대입되어 다양한 구현이 실행될 수 있음(가상 메서드).
  - ex) 아래 예시는 move()라는 메서드를 재정의했다. 상위 클래스 타입(Animal)으로 선언된 변수인 animal에 여러 인스턴스(new Human(), new Tiger(), new Eagle())가 대입됨. 하나의 코드(moveAnimal 메서드 속 `animal.move();`)가 여러가지로 구현되어 실행되는 것(Human, Tiger, Eagle의 재정의된 move()로 실행됨)이 다형성!

  ```java
  class Animal{
  	public void move() {
  		System.out.println("동물이 움직인다.");
  	}
  }

  class Human extends Animal{
  	public void move() {
  		System.out.println("인간이 두 발로 움직인다.");
  	}
  }

  class Tiger extends Animal{
  	public void move()
  	{
  		System.out.println("호랑이가 네발로 움직인다.");
  	}
  }

  public class AnimalTest {
  	 public static void main(String[] args) {
  		  AnimalTest aTest = new AnimalTest();
  		  aTest.moveAnimal(new Human());
  		  aTest.moveAnimal(new Tiger());
  		  aTest.moveAnimal(new Eagle());
  	 }

  	 public void moveAnimal(Animal animal) {
  		animal.move();
  	 }
  }
  ```

<br>

## 다형성 활용과 다운캐스팅(4)

- 상속은 언제 사용할까?
  - IS-A 관계(inheritance): 일반적인 개념(상위 클래스)과 구체적인 개념(하위 클래스) 사이의 관계. 상위 클래스에 공통적인 기능을 모아두고, 하위 클래스에 추가적인 기능을 만들 수 있음.
  - HAS-A 관계(composition): 한 클래스가 다른 클래스를 소유한 관계. 코드 재사용의 한 방법. 즉 한 클래스에서 멤버 변수를 필요하면 다른 클래스 타입으로 선언해서 사용하는 것.
- 다운 캐스팅(instanceof)
  - 각 하위 클래스는 고유한 기능을 추가해서 가질 수 있음. 하지만 하위 클래스의 인스턴스를 업캐스팅해서 사용하면, 상위클래스에 있는 속성이나 메소드밖에 사용할 수 없음. 그럼 하위 클래스의 고유한 기능을 쓰려면 업캐스팅해서 사용한 상위 클래스의 변수를, 다시 하위클래스로 바꿔야만 함. 따라서 다운캐스팅이 쓰임.(즉 오버라이딩으로 해결되지 않으면 다운 캐스팅을 써서 각 하위클래스의 고유의 기능을 사용하는 거임.)
  - 이 때 하나의 상위클래스 변수가 여러 하위클래스의 인스턴스를 담을 수 있음. 즉 aTest라는 변수의 클래스는 animal임. 이 때 이 변수 안에 여러 하위 클래스의 인스턴스(human, tiger, eagle)를 담아서 쓰는 로직이 있다고 하자. 그럼 human 클래스의 고유 기능인 readBook()이란 함수를 쓰기 위해서 무턱대고 다운캐스팅해서 쓰면, 만약 다른 하위 클래스가 담기면 human으로 형 변환 못하고 에러남.
  - 따라서 aTest의 인스턴스 타입이 human일 때만 다운캐스팅해야 한다. 이 때 원래 인스턴스 타입을 체크하는 예약어가 instanceof 임.
  - ex)
  ```java
  // 하위 클래스가 상위 클래스로 형 변환되는 것은 묵시적으로 이루어짐.
  Animal hAnimal = new Human();
  // hAnimal의 인스턴스 자료형이 Human형이라면
  if(hAnimal instanceof Human)
  	// 인스턴스 hAnimal을 Human형으로 다운 캐스팅
  	Human human = (Human)hAnimal;
  ```

<br>

## 추상클래스 활용하기(1)

- 추상 클래스(abstract class)
  - 일반적으론 추상 메서드를 포함한 클래스.
  - 추상 메서드는 구현코드 없이 메서드의 선언만 있음. abstract 예약어 사용.
    ex) `abstract int add(int x, int y);`는 선언만 있는 추상 메서드 / `int add(int x, int y){};`는 {}부분이 구현 내용이라 추상 메서드 아님.
  - 추상 클래스는 new(인스턴스화)할 수 없음. 추상 클래스와 반대되는 개념은 구체적인 클래스(concrete class)임.
- 추상 클래스 구현하기
  - 메서드에 구현코드가 없으면 abstract로 선언해야 함. abstract로 선언된 메서드를 가진 클래스도 abstract로 선언해야 함.
  - ex)
  ```java
  public abstract class Computer {
  	public abstract void display();
  	public abstract void typing();
  }
  ```
  - 만약 모든 메서드가 구현코드가 있지만 클래스가 abstract로 선언된 경우도, 추상 클래스가 되어 new 할 수 없음.
- 추상 클래스 사용
  - 추상 클래스는 상속을 위한 클래스.
  - 추상 메서드: 하위 클래스가 구현해야 할 메서드. 즉 각 하위 클래스마다 다르게 구현되어야 하는 기능.
  - 구현된 메서드: 하위 클래스가 공통으로 사용할 수 있는 기능 구현. 경우에 따라서는 하위 클래스가 재정의(overriding)할 수 있음.

<br>

## 추상클래스와 템플릿 메서드 활용(2)

- 추상 클래스와 템플릿 메서드
  - 템플릿 메서드: 추상 메서드나 구현된 메서드를 활용해서 전체 기능의 흐름(일련의 과정, 시나리오)을 정의하는 메서드. 이 메서드는 시나리오라서 바뀌면 안 되니(ex- startCar()로 시동을 키지 않고 drive()하면 순서 알맞지 않아 오류 날 수 있음), final로 선언해서 하위 클래스에서 재정의할 수 없도록 함.
  - ex)
  ```java
  public abstract class car {
  	public abstract void drive();
  	public abstract void stop();
  	public void startCar(){
  		System.out.println("시동을 킵니다.");
  	}
  	public void turnOff(){
  		System.out.println("시동을  끕니다.");
  	}
  	// 템플릿 메서드
  	public final void run(){
  		startCar();
  		drive();
  		stop();
  		turnOff();
  	}
  }
  ```
  - 프레임워크에서 많이 사용되는 설계 패턴(프레임워크는 구현의 순서가 정해진 경우가 대부분이기 때문에). 추상 클래스로 선언된 상위 클래스에 **템플릿 메서드**를 활용하여 전체적인 흐름을 정의함. 하위 클래스에서 다르게 구현되어야 하는 부분은 **추상 메서드**로 선언해서 하위 클래스가 구현하도록 함.
- final 예약어

  - final 변수는 값이 변경될 수 없는 상수임. 즉 final 변수는 오직 한 번만 값을 할당받을 수 있음. 예로는 `public static final double PI = 3.14;`

    - 프로젝트 시 여러 파일에서 공유해야 하는 상수 값은 하나의 파일에 선언하여 사용하면 편리함.

    ```java
    // Define.java 파일
    public class Define {
    	public static final int MIN = 1;
    	public static final int MAX = 9999;
    }

    // UsingDefine.java 파일
    public class UsingDefine {
    	public static void main(String[] args){
    		System.out.println("최솟값은 " + Define.MIN + "입니다.");
    		System.out.println("최댓값은 " + Define.MAX + "입니다.");
    	}
    }
    ```

  - final 메서드는 하위 클래스에서 재정의(overriding)할 수 없음.
  - final 클래스는 더 이상 상속되지 않음

<br>

## 인터페이스 선언과 구현하기(1)

### 강의내용

- 인터페이스

  - 모든 메서드가 추상 메서드로 이루어진 클래스. 형식적인 선언만 있고 구현은 없음.
  - 인터페이스에 선언된 모든 메서드는 public abstract로 추상 메서드이며, 모든 변수는 public static final로 상수이다.
  - ex) 즉 인터페이스에서 변수를 선언하면 모든 변수 앞에는 컴파일할 때 public static final이 붙고, 모든 메서드에 public abstract가 붙음. 따라서 예시처럼 아무 것도 안 써도 됨.

  ```java
  public interface Calc {
  	double PI = 3.14;

  	int add(int n1, int n2);
  	int sub(int n1, int n2);
  	int mul(int n1, int n2);
  	int div(int n1, int n2);
  }
  ```

- 인터페이스 구현

  - implements를 사용해서 구현. 앞 예시의 Calc의 추상 메서드를 전부 구현하는 게 일반적. 아니면 일부만 구현하면 추상 클래스가 됨. 그 추상 클래스는 상속을 통해 다른 클래스가 나머지 추상 메서드 구현해야 함.
  - ex) add, sub, mul, div 중 둘만 구현해서 Calculator는 추상 클래스가 됨.

  ```java
  // [Calculator.java 파일] / Calc 인터페이스를 일부만 구현한 추상 클래스인 Calculator
  public class Calculator implements Calc {
  	int add(int n1, int n2){
  		return n1 + n2;
  	}
  	int sub(int n1, int n2){
  		return n1 - n2;
  	}
  }

  // [CompleteCalc.java 파일] / 추상 클래스 Calculator 상속받아서 구현한 클래스 CompleteCalc
  public class CompleteCalc extends Calculator {
  	int mul(int n1, int n2){
  		return n1 * n2;
  	}
  	int div(int n1, int n2){
  		return n1 / n2;
  	}
  }
  ```

- 인터페이스 형 변환(`Calc calc = new CompleteCalc();`)
  - 인터페이스 타입으로 선언한 변수에 이를 구현한 여러 객체를 대입할 수 있음. 상속에서의 형 변환(업캐스팅)과 동일한 의미.
  - 또 CompleteCalc 클래스는 Calc(인터페이스)를 구현한 Calculator를 상속받았다. 따라서 인터페이스의 구현 클래스의 자식 클래스도 구현 객체가 될 수 있다.
  - 형 변환 시 사용할 수 있는 메서드는 인터페이스에 선언된 메서드만 사용할 수 있음.

### 추상클래스와 인터페이스의 차이는 뭘까?

- 추상 클래스는 그 추상 클래스를 상속받아서 기능을 이용하고, 확장시키는 데 있다.
  - 상속은 슈퍼클래스의 기능을 재사용하고, 추가로 자기사 필요한 기능을 확장해서 사용할 수 있다.
  - 또한 IS-A 관계(하위 클래스는 상위 클래스다)를 만족한다. 즉 개는 동물이다. 사람은 동물이다 ...
- 반면 인터페이스는 해당 인터페이스를 구현한 객체들이 동일한 동작을 보장하기 위해 존재한다.
  - 인터페이스는 구현 클래스 is able to 인터페이스 라고 해석할 수 있다.(Comparable: 비교할 수 있는 / Runnable: 실행할 수 있는)
  - 즉 인터페이스와 이를 구현하는 클래스 간에는 상속처럼 일반적-구체적 관계가 없을 수 있다. 그저 특정 기능을 할 수 있도록 구현한다는 느낌이다.

### 왜 인터페이스는 다중 상속되고, 클래스는 단일 상속만 될까?

- 클래스 상속과 달리 인터페이스는 구현코드가 없기 때문에, 다중 상속이 가능하다.
- 클래스를 다중상속할 때의 문제점
  - 만약 자식클래스가 둘 이상의 클래스로부터 다중 상속을 받는다고 가정하자. ParentA 부모 클래스에 love() 라는 메서드가 이미 정의되어있고, 마찬가지로 ParentB 라는 부모 클래스에도 love() 메서드가 존재한다. love() 메서드가 동일한 시그니처를 가진다면, 자식클래스는 둘 중 어느것을 상속받아야 할까? (동일 시그니처 : 메서드의 접근 제어자, 리턴 타입, 메서드 명, 매개변수가 모두 동일함을 의미. 메서드 구현부는 같을수도, 다를 수도 있다.)
  - 자식 클래스 입장에서는, love() 메서드를 상속받을 때, 어느 부모의 메서드를 상속받아야할 지 파악할 수 없게 된다. 이미 구현이 완료된 동일 시그니처의 메서드를 상속받는 일은 결국 불가능하다.
- 인터페이스는 다중상속이 가능한 이유
  - 인터페이스는 다중 상속이 가능하지만, 위의 문제를 피해 갈 수 있다. 왜냐하면 메서드가 정의되지 않았기 때문이다.
  - 즉 선언된 형태의 메서드만 가지는 인터페이스는, 동일 시그니처의 메서드를 얼마든 상속받아도 아무런 문제가 발생하지 않는다.
  - 해당 메서드는 아직 구현되지 않아 자식 클래스에서 새롭게 정의해야 하기 때문이다.
- 그럼 default method는? (자바 8 이후부터)

  - 디폴트 메서드는 기본 구현을 가지는 메서드며, 인터페이스에서 쓸 수 있게 되었다. 그럼 구현 부분이 있는 인터페이스는 다중 상속을 할 수 없지 않은가? 그렇다.
  - 아래 예시처럼 ParentA, ParentB가 love라는 동일한 디폴트 메소드가 있다면, 두 인터페이스를 상속받자마자 에러가 다음처럼 뜬다.

  ```java
  interface ParentA {
    default void love() {
      System.out.println("A's love");
    };
  }

  interface ParentB {
    default void love() {
      System.out.println("B's love");
    };
  }

  // 다중상속
  class child implements ParentA, ParentB {
    /*
    오류 발생: Duplicate default methods named love with the parameters () and () are inherited from the types ParentB and ParentA
    */
  }
  ```

  - 해결법으론 child 클래스에서 love() 메소드를 오버라이딩 해서 사용해야 한다. 그러니 에러가 사라졌다.

  ```java
    class child implements ParentA, ParentB {
      @Override
      public void love() {

      }
    }
  ```

<br>

## 인터페이스와 다형성 구현(2)

- 인터페이스와 다형성

  - 인터페이스는 "Client Code(서비스(객체)를 사용하는 코드)"와 서비스를 제공하는 "객체" 사이의 약속이다(즉 JDBC 느낌)
  - 어떤 객체가 어떤 interface 타입이라 함은 그 interface가 제공하는 메서드를 구현했다는 의미. 따라서 Client는 어떻게 구현되었는지 상관없이 interface의 정의만을 보고 사용할 수 있음

- 인터페이스도 메소드 재정의와 타입 변환이 가능해서 다형성을 구현할 수 있다.
  - 인터페이스의 다형성: 인터페이스의 사용 방법, 사용하는 코드는 같으나 구현 객체를 교체하여 프로그램 실행 결과를 다양하게 할 수 있다.
  - 따라서 클라이언트 프로그램은 실제 구현 내용을 몰라도 인터페이스의 정의만 알면 객체를 사용할 수 있다
  - 예) JDBC를 사용하기 위해 DB와 연결하는 Connection, 쿼리를 날리는 Statement, 결과를 받는 ResultSet 등이 필요하다. 그래서 각 DB 회사 들은 저 인터페이스를 구현하는 jar를 개발하고, 사용자는 이 jar를 import한다. 결국, DB의 종료와 상관없이 같은 명령문을 사용할 수 있다.

<br>

## 인터페이스 활용하기(3)

- 인터페이스 요소
  - 상수: 모든 변수는 상수로 변환됨
  - 추상 메서드: 모든 메서드는 추상 메서드로 됨.
  - java 8부터 다음이 추가되었음.
    - 디폴트 메서드: 기본 구현을 가지는 메서드, 구현 클래스에서 재정의할 수 있음. default 예약어를 씀
    ```java
    public interface Calc{
    	default void method(){
    	// 구현 코드
    	}
    }
    ```
    - 정적(static) 메서드: 인스턴스 생성과 상관없이 인터페이스 타입으로 사용할 수 있는 메서드. static을 사용.
    - private 메서드: 인터페이스를 구현한 클래스에서 사용하거나 재정의할 수 없음. 인터페이스 내부에서만 기능을 제공하기 위해 구현하는 메서드.
      - private 혹은 private static으로 선언한 메서드 구현.
      - private static으로 선언한 메서드는 같은 인터페이스 안의 정적 메서드에서 사용하는 함수. private으로 선언한 메서드는 같은 인터페이스 안의 default 메서드에서 쓰는 용도.
- 두 인터페이스의 디폴트 메서드가 중복되는 경우: 인터페이스를 구현하는 클래스에서 중복된 디폴트 메서드를 재정의함.
- 인터페이스 상속
  - 인터페이스 '간'에도 상속이 가능. 구현코드의 상속이 아니므로 형 상속(type inheritance)라고 함.
  - ex)
  ```java
  // X, Y는 인터페이스임.
  public interface MyInterface extends X, Y{
  	void myMethod();
  }
  ```
- 인터페이스 구현과 클래스 상속 함께 사용하기: "extends 클래스 implements 인터페이스"
  - ex)
  ```java
  public class BookShelf extends Shelf implements Queue {
  	// 내용
  }
  ```
  - 프레임워크(스프링, 안드로이드)에서 이런 경우가 종종 있음. 프레임워크는 로직 틀이 정해져 있고, 이 틀 안에서 필요한 걸 구현하는 것.

<br><br>

# 3. 자바 JDK로 프로그래밍 날개 달기

## 기본 클래스(1)

- java.lang 패키지
  - 프로그래밍시 import하지 않아도 자동으로 import됨. `import java.lang.*;` 문장이 추가됨
  - 많이 사용하는 기본 클래스들이 속한 패키지(String, Integer, System 등)
- Object 클래스
  - java.lang.Object 클래스이자, 모든 클래스의 최상위 클래스. 컴파일러가 변환시 자동으로 extends Object를 각 클래스에 추가함.
  - 모든 클래스는 Object 클래스에서 상속 받고, Object 클래스의 메서드를 사용할 수 있으며, Object 클래스의 메서드 중 일부는 재정의할 수 있음(final로 선언된 메서드는 재정의할 수 없음).
- Object 클래스 메서드

  - String toString()
    - 객체의 정보를 string으로 바꾸어서 사용할 때 많이 쓰임. 자기가 만든 클래스에서 이 메서드를 재정의해서 사용.
    - String이나 Integer 클래스에는 이미 재정의되어 있음.
  - boolean equals(Object obj)

    - 두 인스턴스의 주소값을 비교해서 true/false를 반환.
    - 재정의하여 두 인스턴스가 논리적으로 동일한지를 반환(String이나 Integer 클래스에는 이미 재정의되어 있음).

    ```java
    String str1 = new String("test");
    String str2 = new String("test");

    // [물리적으로 비교] str1과 str2의 메모리 주소값이 같은지를 비교하고,
    // 이 결과는 false이다. Object 속 equals()는 기본적으로 이와 같다.
    System.out.println(str1 == str2);

    // [논리적으로 비교] str1과 str2의 문자열이 같은지를 비교하고,
    // 그 결과 true를 리턴한다.
    System.out.println(str1.equals(str2));
    ```

  - int hashCode()
    - hash: 정보를 저장, 검색하기 위해 사용하는 자료구조. 힙 메모리에 인스턴스가 저장되는 방식이 hash.
    - 자료의 특정 값(키 값)에 대해 저장위치를 반환해주는 해시 함수를 사용함. hashCode() 메서드는 인스턴스의 저장 주소를 반환함. 다르게 말하면 자바 가상 머신(JVM)이 저장한 인스턴스의 주소값을 10진수로 나타냄.
    - 서로 다른 메모리의 두 인스턴스가 같다면? 재정의된 equals() 메서드의 값이 true일 것이고, 동일한 hashCode() 반환값을 가져야 함. 즉 논리적으로 동일함을 위해 equals() 메서드를 재정의하였다면, hashCode() 메서드로 재정의하여 주소는 다를지라도 논리적으로 같은 값이면 동일한 값이 반환되도록 해야 함.
    - 예로 String 클래스의 경우, 동일한 문자열 인스턴스에 대해 동일한 정수가 반환됨. 또 Integer 클래스의 경우, 동일한 정수값의 인스턴스에 대해 같은 정수값이 반환됨.
  - Object clone()
    - 객체의 원본을 복제하는데 사용하는 메서드. 원본을 유지해 놓고 복사본을 사용할 때.
    - clone() 메서드를 사용하면 객체의 정보(예로 멤버변수 값)가 같은 인스턴스가 또 생성되는 것이므로 객체 지향 프로그램의 정보은닉, 객체 보호의 관점에서 위배될 수 있음. 따라서 객체의 clone() 메서드 사용을 허용한다는 의미로 cloneable 인터페이스를 명시해 줌.
    ```java
    class Circle implements Cloneable {
    	// 코드 내용
    }
    ```

<br>

## 기본 클래스(2)

- String 클래스

  - String을 선언하는 두 가지 방법으로, 힙 메모리(인스턴스가 생성되면 힙 메모리라는 곳에 생성됨)에 인스턴스로 생성되는 경우와 상수 풀에 있는 주소를 참조하는 방법 두 가지.
  - 상수 풀의 문자열을 참조하면 모든 문자열이 같은 주소를 가리킴.

  ```java
  // 생성자의 매개변수로 문자열 생성
  String str1 = new String("abc");

  // 문자열 상수를 가리키는 방식
  String str2 = "abc";
  ```

- String 클래스로 문자 연결
  - 한 번 생성된 String 값(문자열)은 불변(immutable). 두 개의 문자열을 연결하면 메모리에 새로운 인스턴스가 생성됨. 문자열 연결을 계속하면 메모리에 gabage가 많이 생길 수 있음.
  - ex) `str1 = str1.concat(str2);`
- StringBuilder, StringBuffer 사용하기

  - 내부적으로 가변적인 char[] 배열을 가지고 있는 클래스. 문자열을 여러 번 연결하거나 변경할 때 사용하면 유용함. 매번 새로 생성하지 않고 기존 배열을 변경하므로 gabage가 생기지 않음.
  - StringBuffer는 멀티 쓰레드 프로그래밍에서 동기화(Synchronization / 쓰레드 간의 순서를 보장. 즉 두 쓰레드가 동시에 리소스에 접근할 때 문제가 발생할 수 있는데, 이를 방지. 즉 쓸 땐 리소스에 락을 걸고, 다 쓰면 락을 풀고.)를 보장. 단일 쓰레드 프로그램에서는 StringBuilder를 사용하기를 권장.
  - toString() 메서드로 String을 반환.

  ```java
  String str = new String("abc");
  StringBuilder buffer = new StringBuilder(str);

  buffer.append(" def"); // 문자열 추가
  str = buffer.toString(); // String 클래스로 변환
  ```

- Wrapper 클래스
  - 기본 자료형(primitive data type)에 대한 클래스
  - boolean(기본형): Boolean(Wrapper 클래스) / byte: Byte / char: Character / short: Short / int: Integer / long: Long / float: Float / double: Double
- 오토 박싱(auto boxing)과 언박싱(unboxing)

  - Integer는 객체이고, int는 4바이트 기본 자료형임. 두 개의 자료를 같이 연산할 때 자동으로 변환이 일어남.

  ```java
  Integer n1 = new Integer(100);
  int n2 = 200;

  // n1(객체)이 자동으로 n1.intValue()로 변환(언박싱)
  int sum = n1 + n2;
  // n2(기본 자료형)가 자동으로 Integer.valueOf(n2)로 변환(오토박싱)
  Integer n3 = n2;
  ```

  - 단 `Integer n1 = new Integer(100);`과 같이 생성자를 사용하는 거는 자바 9 이전까지고, 자바 9 이후부터는 생성자 사용 없이 `Integer n1 = 100;`로 함.

- Class 클래스
  - 자바의 모든 클래스와 인터페이스는 컴파일 후 class 파일로 생성됨. class 파일에는 객체의 정보(멤버 변수, 메서드, 생성자 등)이 포함되어 있음.
  - Class 클래스는 컴파일된 class 파일에서 객체의 정보를 가져올 수 있음.
- Class 클래스 가져오기
  1.  Object 클래스의 getClass() 메서드 이용하기
  ```java
  String s = new String();
  // getClass() 메서드의 반환형은 Class
  Class c = s.getClass();
  ```
  2.  클래스 파일 이름을 Class 변수에 직접 대입하기: `Class c = String.Class;`
  3.  Class.forName("클래스 이름") 메서드 사용하기: `Class c = Class.forName("java.lang.String");`(입력한 클래스 이름이 있으면, 이를 메모리에 로딩함(동적 로딩))
- Class 클래스로 정보 가져오기
  - reflection 프로그래밍: Class 클래스를 이용하여 클래스의 정보(생성자, 멤버변수, 메서드)를 가져오고 이를 활용하여 인스턴스를 생성하고, 메서드를 호출하는 등의 프로그래밍 방식.
  - 로컬 메모리에 객체가 없어서 데이터 타입을 직접 알 수 없는 경우(원격에 객체가 있는 경우 등) 객체 정보만을 이용하여 프로그래임할 수 있음.
  - Constructor, Method, Field 등 java.lang.reflect 패키지에 있는 클래스들을 활용하여 프로그래밍.
  - 단, 일반적으로 자료형을 알 수 있는 경우에는 사용하지 않음.
- Class.forName() 메서드로 동적 로딩하기
  - 동적 로딩은, 컴파일 시 데이터 타입이 모두 로딩되는 것(static loading)이 아니라, 실행 중에 데이터 타입이 로드되는 방식.
  - 프로그래밍할 때는 어떤 클래스를 사용할 지 모를 때 변수로 처리한다. 그리고 실행될 때 해당 변수에 대입된 값의 클래스가 실행될 수 있도록 Class 클래스에서 제공하는 static 메서드
  - 실행 시에 로딩되므로 경우에 따라 다른 클래스가 사용될 수 있어 유용함. 컴파일 시간 때 체크할 수 없으므로 해당 문자열에 대한 클래스가 없는 경우 예외(ClassNotFoundException)이 발생할 수 있음.

<br>

## 제너릭 프로그래밍

- 제너릭(generic) 프로그래밍
  - 변수의 선언이나 메서드의 매개변수를, 하나의 참조 자료형이 아니라 여러 자료형이 쓰일 수 있도록 프로그래밍하는 방식
  - 실제 사용되는 참조 자료형으로의 변환은 컴파일러가 검증하므로 안정적인 프로그래밍 방식
  - 컬렉션 프레임워크에서 많이 사용되고 있음.
- 제너릭 클래스 정의하기

  - 여러 참조 자료형으로 대체될 수 있는 부분을 하나의 문자로 표현. 이 문자를 자료형 매개변수라고 함.

  ```java
  public class GenericPrinter<T> {
  	private T material;

  	public void setMaterial(T material){
  		this.material = material;
  	}
  	public T getMaterial(){
  		return material;
  	}
  }
  ```

- 자료형 매개변수 T
  - type의 의미로 T를 많이 사용함.
  - `<T>`에서 <>는 다이아몬드 연산자라고 함.
  - static 키워드는 T 앞에 사용할 수 없음.
    - 왜냐면 T는 new해서 인스턴스를 생성할 때 뭐를 쓸 건지 결정된다. (`GenericPrinter<Powder> printer = new GenericPrinter<Powder>();`)
    - static은 인스턴스 생성과 상관없이 메모리를 잡는다. 즉 어떤 타입이 올지 모르는데도... 그러니 static은 같이 안 쓰인다.
  - 다이아몬드 연산자 내부에서 자료형은 생략 가능함. : `ArrayList<String> list = new ArrayList<>();`
  - 제너릭에서 자료형 추론(자바 10부터): `var list = new ArrayList<String>();`
- 제너릭에서 대입된 자료형을 명시하지 않은 경우
  - `GenericPrinter<Powder>`와 같이 사용할 자료형(Powder)를 명시해야 함. 단 명시하지 않을 경우 강제 형 변환을 해야 함.
  - 즉 안 쓰는 경우에는 Object로 간주하는 거랑 마찬가지라서, `Powder powder = (Powder) powderPrinter.getMaterial();`처럼 다운캐스팅해야 함.
- `<T extends 클래스>`
  - T가 사용될 클래스를 제한하기 위해 사용. 위 예시에서 GenericPrinter의 참조 자료형으로, Plastic, Powder는 상식적으로 말이 되지만, Water로 프린터할 순 없다. 이 때 Plastic, Powder는 받아들이고, Water는 안 받아들이고 싶을 때 사용.
  - 그럼 먼저 GenericPrinter의 재료(Material)로 사용할 클래스들(Plastic, Powder)는 Material이라는 상위 클래스를 상속받도록 수정한다.
  - 그 후 `public class GenericPrinter<T>`에서 `<T>`를 `<T extends Material>`로 고치면, Material에서 상속받지 않는 Water와 같은 클래스는 프린터 재료로 사용할 수 없음. 또 Material에 정의된 메서드를 공유할 수 있음.
- 제네릭 메서드, 자료형 매개변수가 두 개인 경우
  - 제네릭 메서드: 아래 예시에서 getX, getY 메서드가
  - 자료형 매개변수가 두 개인 경우: `public class Point<T, V>`

```java
public class Point<T, V>{
  T x;
  V y;

  public T getX(){
    return x;
  }

  public V getY(){
    return y;
  }
}
```

<br>

## 컬렉션 프레임워크

- 배열
  - 같은 타입의 자료들이 순서대로 연속적으로 메모리에 저장됨. 원소들의 논리적 순서와 같은 순서로 기억공간에 저장됨.(순차구조). 즉 원소들의 논리적 순서 = 원소가 저장된 물리적 순서
  - 길이가 고정되어 있다.
  - 요소 접근은 빠르게 가능하지만(인덱스 연산), 삽입과 삭제는 시간이 오래 걸린다.
  - 관련 클래스: ArrayList, Vector
- 연결 리스트
  - 물리적으로는 떨어져 있지만, 논리적으로는 바로 옆인게 특징.
  - 길이가 가변적이다.
  - 삽입과 삭제가 빠르지만, 요소 접근은 느리다.
  - 관련 클래스: LinkedList
- Stack과 Queue
  - 뭘로 구현하든 상관 없다. 즉 배열로도 가능하고, 연결 리스트로도 가능하다. 스택은 선형 자료구조고, 큐는 선형도 있고 원형도 있고.
  - 스택은 LIFO, 큐는 FIFO / 스택은 push()와 pop()과 peek(), 큐는 add()와 offer(), poll()와 remove(), peek()이 있음.
  - 큐 앞은 front, 끝은 rear / 스택은 top
- Binary Search Tree
  - 나를 기준으로 왼쪽 부분(left child)는 무조건 작은 값, 오른쪽 부분은 큰 값이여야 함. 그러면 자료 검색할 때 편하지.
  - 또 중복 허용하지 않음.
  - 이를 통해서 정렬을 할 수 있음
    - inorder(중위 순회): (왼쪽 자식) (나) (오른쪽 자식) 순으로 순회
    - 무조건 왼쪽 부분에 작은 값, 오른쪽 부분에 큰 값이 있는게 BST니까, 중위 순회를 하면 오름차순으로 순회하게 됨.
  - 관련 클래스: TreeSet, TreeMap(앞의 클래스 둘 다 Red-Black Tree로 구현되어 있다.)
- 해싱(hasing)
  - 산술 연산을 이용한 검색 방식.
  - 해싱 함수(Hashing function)에 key값(검색 키)을 넣어주면 index값(검색 자료의 주소)를 반환해줌.
  - 관련 클래스: HashMap, HashSet

<br>

## 컬렉션 프레임워크 - ArrayList

- 컬렉션 프레임워크
  - 프로그램 구현에 필요한 자료구조를 구현해 놓은 라이브러리. 시간 복잡도 면에서 최적화된 알고리즘을 사용할 수 있음. 여러 인터페이스와 구현 클래스 사용 방법을 이해해야 함.
  - java.util 패키지에 구현되어 있음.
  - Collection은 단일 값을 다루고, Map은 key-value 쌍(예로 특기-축구)을 다룸. 이 때 key는 중복될 수 없음.
  - List는 선형구조(한 줄로 쭉 늘어선 듯한 구조)고, 앞뒤 순서가 있음. 요소 중복되도 상관 없음.
  - Set은 집합임. 순서가 상관없음. 요소 중복되면 안됨. 그래서 유일한 데이터(id, 학번)를 주로 다룰 때 쓰임.
  - HashTable과 HashMap은 Vector와 ArrayList와 유사함. HashTable은 동기화를 지원하고, HashMap은 동기화 지원 안 함. HashMap을 주로 사용함.
  - 파일에서 읽어들여서 key-value 쌍으로 저장할 일이 있는데, 이 때 Properties를 사용.

<img width="680" alt="image" src="https://user-images.githubusercontent.com/112764753/210171885-d23771e3-e1a4-4823-bfef-81d9b4eceeb4.png">{: .align-center}

- Collection 인터페이스
  - 하나의 객체를 관리하기 위한 메서드가 선언된 인터페이스. 하위에 List와 Set 인터페이스가 있음. 여러 클래스들이 Collection 인터페이스를 구현함.
  - List 인터페이스: 순서가 있는 자료 관리. 중복 허용. 이를 구현한 클래스로 ArrayList, Vector, LinkedList, Stack, Queue 등이 있음.
  - Set 인터페이스: 순서가 정해져 있지 않음. 중복을 허용하지 않음. 이를 구현한 클래스로는 HashSet과 TreeSet 등이 있음.

<img width="340" alt="image" src="https://user-images.githubusercontent.com/112764753/210172005-3d1dff9b-f71c-4269-be6a-bdafb5bd89ac.png">{: .align-center}

- Collection 인터페이스의 주요 메소드
  - 핵심은 모든 메소드와 클래스를 기억할 필요 없다. 주요한 것만 몇 개 기억하고, 나머지는 공식문서에서 찾아서 사용하자.
  - `boolean add(E e)`: Collection에 객체를 추가
  - `void clear()`: Collection에 모든 객체를 제거
  - `iterator<E> iterator`: Collection을 순환할 반복자(iterator)를 반환
  - `boolean remove(Object o)`: Collection에 배개변수에 해당하는 인스턴스가 존재하면 제거
  - `int size()`: Collection에 있는 요소 개수 반환
- ArrayList와 Vector
  - 객체 배열을 구현한 클래스. 일반적으로는 ArrayList를 더 많이 사용하고, 멀티 쓰레드 상태에서 리소스에 대한 동기화가 필요한 경우 Vector를 사용.
  - 동기화(synchronization): 두 개의 쓰레드가 동시에 하나의 리소스에 접근할 때 순서를 맞추어 데이터에 오류가 발생하지 않도록 함.
  - ArrayList에 동기화 기능이 추가되어야 하는 경우: `Collections.synchronizedList(new ArrayList<String>());`
  - 쓰레드와 동기화는 OS와 관련되어 있음. 하드에 프로그램에 있고, 이걸 더블 클릭하면 메모리에 올라오고 이 때 프로그램을 프로세스라고 함. 프로세스는 **쓰레드**라는 작업 단위가 있고, 이게 cpu를 점유함(하나의 프로세스는 반드시 하나 이상의 쓰레드를 가지게 됨).
  - 하나의 프로그램에서 어떤 작업을 동시에 하는 것처럼 보이기 위한 게 **멀티 쓰레드 프로그램**(쓰레드 두개 이상)이다. 각 쓰레드가 메모리에 접근할 때, 어느 쓰레드가 접근하고 있는 곳에 다른 쓰레드가 접근하면 문제. 그래서 동기화 필요.

<br>

## 컬렉션 프레임워크 - Stack, Queue, HashSet

- Stack과 Queue
  - Stack과 Queue의 기능은 구현된 클래스가 있지만 ArrayList나 LinkedList를 활용해서 구현할 수 있음.
  - stack: LIFO. 게임의 무르기 기능, 최근 자료 추출 등에서 사용.
  - queue: FIFO. 선착순, 대기열 구조.
- Iterator 사용하여 순회하기
  - Collection의 개체(ArrayList, HashSet 등)를 순회하는 인터페이스
  - iterator() 메서드 호출: `Iterator<Member> ir = memberArrayList.iterator();`
  - list의 경우 for문으로 순회할 수 있지만, set같이 순서가 없는 경우 필요함.
  - 선언된 메서드
    - boolean hasNext(): 이후에 요소가 더 있는지를 체크. 요소가 있다면 true 반환.
    - E next(): 다음에 있는 요소를 반환.
- Set 인터페이스
  - Collection의 하위 리스트.
  - 중복을 허용하지 않음. 따라서 아이디, 주민번호 등 유일한 값이나 객체를 관리할 때 사용. 또 List는 순서가 있지만, Set은 순서가 없음. 따라서 저장된 순서와 출력 순서는 다를 수 있음.
  - get(i) 메서드 제공되지 않음. 인덱스 i의 값을 get하겠다는 뜻인데, set은 순서가 없으니까 당연히 안 됨.

<br>

## 컬렉션 프레임워크 - TreeSet, HashMap, TreeMap

- TreeSet 클래스
  - 자바 클래스 이름 앞에 Tree가 붙으면 정렬이 된다. 객체의 정렬에 사용되는 클래스. 중복을 허용하지 않으면서 오름차순이나 내림차순으로 객체를 정렬함.
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
  - key 객체를 정렬하여 key-value pair를 관리. key에 사용되는 클래스에 Comparable, Comparator 인터페이스를 구현.
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
