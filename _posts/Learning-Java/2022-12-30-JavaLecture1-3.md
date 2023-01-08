---
title: "[Do it! 자바 프로그래밍 입문] <다차원 배열(3)> ~  <오버라이딩과 다형성(3)>"
excerpt: "배열 / 상속과 다형성"

categories:
  - Learning-Java
tags:
  - [Java Lecture]

toc: true
toc_sticky: true
toc_label: "페이지 주요 목차"

date: 2022-12-30
last_modified_at: 2022-12-30
---

<br>

# [인프런 강의 주소](https://www.inflearn.com/course/%EC%9E%90%EB%B0%94-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D-%EC%9E%85%EB%AC%B8#curriculum)

<br>

# 보는 방법

블로그에 메모한 내용만으로 복습하기에는 무리. 복습할 땐 강의 영상 보고, 이건 상기용/테스트용. 메모한 거 보면서 강의 내용이 떠오르는지 등을 점검하는 데 활용.

<br>

# 2. 자바의 핵심 - 객체지향 프로그래밍

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
