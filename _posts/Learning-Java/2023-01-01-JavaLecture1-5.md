---
title: "■ [Do it! 자바 프로그래밍 입문] <인터페이스와 다형성 구현(2)> ~  <컬렉션 프레임워크 - Stack, Queue, HashSet>"
excerpt: "인터페이스 / 기본 클래스 / 제너릭 프로그래밍 / 컬렉션 프레임워크(ArrayList, Stack, Queue, HashSet)"

categories:
  - Learning-Java
tags:
  - [Java Lecture]

toc: true
toc_sticky: true
toc_label: "페이지 주요 목차"

date: 2023-01-01
last_modified_at: 2023-01-01
---

<br>

# [인프런 강의 주소](https://www.inflearn.com/course/%EC%9E%90%EB%B0%94-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D-%EC%9E%85%EB%AC%B8#curriculum)

<br>

# 보는 방법

블로그에 메모한 내용만으로 복습하기에는 무리. 복습할 땐 강의 영상 보고, 이건 상기용/테스트용. 메모한 거 보면서 강의 내용이 떠오르는지 등을 점검하는 데 활용.

<br>

# 2. 자바의 핵심 - 객체지향 프로그래밍

## 인터페이스와 다형성 구현(2)

- 인터페이스와 다형성
  - 인터페이스는 "Client Code(서비스(객체)를 사용하는 코드)"와 서비스를 제공하는 "객체" 사이의 약속이다.
  - 어떤 객체가 어떤 interface 타입이라 함은 그 interface가 제공하는 메서드를 구현했다는 의미. 따라서 Client는 어떻게 구현되었는지 상관없이 interface의 정의만을 보고 사용할 수 있음
  - 다양한 구현이 필요한 인터페이스를 설계하는 일은 매우 중요한 일임.

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
      - private 혹은 private static으로 선언한 메서드 구현. private static은 정적 메서드에서 사용하는 함수. 그냥 private은 default 메서드에서 쓰는 용도.
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
  - <T>에서 <>는 다이아몬드 연산자라고 함.
  - static 키워드는 T 앞에 사용할 수 없음. 왜냐면 T는 new해서 인스턴스를 생성할 때 뭐를 쓸 건지 결정되는데, static은 인스턴스 생성과 상관없이 메모리를 잡는다. 즉 어떤 타입이 올지 모르는데도... 그러니 static은 같이 안 쓰인다.
  - 다이아몬드 연산자 내부에서 자료형은 생략 가능함. : `ArrayList<String> list = new ArrayList<>();`
  - 제너릭에서 자료형 추론(자바 10부터): `var list = new ArrayList<String>();`
- 제너릭에서 대입된 자료형을 명시하지 않은 경우
  - GenericPrinter<Powder>와 같이 사용할 자료형(Powder)를 명시해야 함. 단 명시하지 않을 경우 강제 형 변환을 해야 함.
- `<T extends 클래스>`
  - T가 사용될 클래스를 제한하기 위해 사용. 예로 `<T extends Material>`이라 하면 Material에서 상속받지 않는 Water와 같은 클래스는 프린터 재료로 사용할 수 없음. 또 Material에 정의된 메서드를 공유할 수 있음.
- 제너릭 메서드
- 자료형 매개변수가 두 개인 경우

```java
public class Point<T, V> {
	T x;
	V y;

	Point(T x, V y){
		this.x = x;
		this.y = y;
	}
}
```

<br>

## 컬렉션 프레임워크

- 배열
  - 자료들이 순서대로 연속적으로 메모리에 저장됨. 원소들의 논리적 순서와 같은 순서로 기억공간에 저장(순차구조)하고 원소들의 논리적 순서 = 원소가 저장된 물리적 순서
  - 관련 클래스: ArrayList, Vector
- 연결 리스트
  - 물리적으로는 떨어져 있지만, 논리적으로는 바로 옆인게 특징.
  - 관련 클래스: LinkedList
- Stack과 Queue
  - 뭘로 구현하든 상관 없다. 즉 배열로도 가능하고, 연결 리스트로도 가능하다. 스택은 선형 자료구조고, 큐는 선형도 있고 원형도 있고.
  - 스택은 LIFO, 큐는 FIFO / 스택은 push()와 pop()과 peek(), 큐는 enqueue()와 dequeue()
  - 큐 앞은 front, 끝은 rear / 스택은 top
- Binary Search Tree
  - 나를 기준으로 왼쪽 부분(left child)는 무조건 작은 값, 오른쪽 부분은 큰 값이여야 함. 그러면 자료 검색할 때 편하지.
  - 또 중복 허용하지 않음.
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
  - Collection은 단일 값을 다루고, Map은 key-value 쌍(특기-축구)을 다룸. 이 때 key는 중복될 수 없음.
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
  - 객체 배열을 구현한 클래스. 일반적으로는 ArrayList를 더 많이 사용하고, 멀티 쓰레드 상태에서 리소스에 대한 동기화가 필요한 경우 Vectro를 사용.
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
  - 선언된 메서드
    - boolean hasNext(): 이후에 요소가 더 있는지를 체크. 요소가 있다면 true 반환.
    - E next(): 다음에 있는 요소를 반환.
- Set 인터페이스
  - Collection의 하위 리스트.
  - 중복을 허용하지 않음. 따라서 아이디, 주민번호 등 유일한 값이나 객체를 관리할 때 사용. 또 List는 순서가 있지만, Set은 순서가 없음. 따라서 저장된 순서와 출력 순서는 다를 수 있음.
  - get(i) 메서드 제공되지 않음.
