---
title: "[Do it! 자바 프로그래밍 입문] <다형성 활용과 다운캐스팅(4)> ~ <인터페이스 선언과 구현하기(1)>"
excerpt: "추상클래스 / 인터페이스"

categories:
  - Learning-Java
tags:
  - []

toc: true
toc_sticky: true
toc_label: "페이지 주요 목차"

date: 2022-12-31
last_modified_at: 2022-12-31
---

<br>

# [인프런 강의 주소](https://www.inflearn.com/course/%EC%9E%90%EB%B0%94-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D-%EC%9E%85%EB%AC%B8#curriculum)

<br>

# 보는 방법

블로그에 메모한 내용만으로 복습하기에는 무리. 복습할 땐 강의 영상 보고, 이건 상기용/테스트용. 메모한 거 보면서 강의 내용이 떠오르는지 등을 점검하는 데 활용.

<br>

# 2. 자바의 핵심 - 객체지향 프로그래밍

## 다형성 활용과 다운캐스팅(4)

- 상속은 언제 사용할까?
  - IS-A 관계(inheritance): 일반적인 개념(상위 클래스)과 구체적인 개념(하위 클래스) 사이의 관계. 따라서 단순히 코드를 재사용하는 목적으로 사용하지 않음.
  - HAS-A 관계(composition): 한 클래스가 다른 클래스를 소유한 관계. 코드 재사용의 한 방법. 즉 한 클래스에서 멤버 변수를 필요하면 다른 클래스 타입으로 선언해서 사용하는 것.
- 다운 캐스팅(instanceof)
  - 하위 클래스가 상위 클래스로 형 변환되는 것은 묵시적으로 이루어짐. 다시 원래 자료형인 하위 클래스로 형 변환하려면 명시적으로 다운캐스팅 히야 함. 이 때 원래 인스턴스 타입을 체크하는 예약어가 instanceof임.
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

- 인터페이스 형 변환
  - 인터페이스를 구현한 클래스는 인터페이스 형으로 선언한 변수로 형 변환할 수 있음. 상속에서의 형 변환과 동일한 의미.
  - ex) `Calc calc = new CompleteCalc();`
  - 클래스 상속과 달리 구현코드가 없기 때문에 여러 인터페이스를 구현할 수 있음. 형 변환 시 사용할 수 있는 메서드는 인터페이스에 선언된 메서드만 사용할 수 있음.
