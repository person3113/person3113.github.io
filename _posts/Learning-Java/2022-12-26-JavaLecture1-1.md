---
title: "[Do it! 자바 프로그래밍 입문] <1. 자바 기본 익히기> ~ <오버라이딩과 다형성(3)>"
excerpt: "변수와 자료형 / 클래스와 객체1 / 클래스와 객체2 / 배열 / 상속과 다형성"

categories:
  - Learning-Java
tags:
  - []

toc: true
toc_sticky: true
toc_label: "페이지 주요 목차"

date: 2022-12-26
last_modified_at: 2023-01-23
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
- 오버로드
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
  - 하지만 가상 메서드의 경우 타입과 상관없이 실제 생성된 인스턴스의 메서드가 호출되는 원리(`kim.clacPrice(10000);`에서 clacPrice()는 Customer와 VIPCustomer 둘 다 있는 메서드라면, VIPCustomer의 것이 호출된다는 뜻).
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
