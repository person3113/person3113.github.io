---
title: "[Do it! 자바 완전정복 - 15장 쓰레드] '04 (이론) 쓰레드 동기화' ~ '06 (실습) 쓰레드의 상태 #2'"
excerpt: "쓰레드 동기화 / 쓰레드의 상태 #1 / 쓰레드의 상태 #2"

categories:
  - Learning-Java
tags:
  - []

toc: true
toc_sticky: true
toc_label: "페이지 주요 목차"

date: 2023-01-09
last_modified_at: 2023-01-09
---

<br>

# [유튜브 강의 주소](https://www.youtube.com/watch?v=vKgDXrN5zAQ&list=PLR9w0n2BH7rci0nw5SGiYN0-Ded98-Ew5)

<br>

# 보는 방법

블로그에 메모한 내용만으로 복습하기에는 무리. 복습할 땐 강의 영상 보고, 이건 상기용/테스트용. 메모한 거 보면서 강의 내용이 떠오르는지 등을 점검하는 데 활용.

<br><br>

# 04 (이론) 쓰레드 동기화

- 쓰레드 동기화(synchronized)
  - 하나의 작업이 완전히 완료된 후 다른 작업 수행(한 번에 하나의 일만 한다)
  - cf) 비동기식: 하나의 작업 명령 이후(완료와 상관없이) 바로 다른 작업 명령 수행
- 동기화 필요성: 두 개의 쓰레드가 하나의 객체를 공유할 때
  - 공유되는 객체를 한 번에 하나의 쓰레드만 사용할 수 있도록 함(하나의 쓰레드가 사용 중일 때는 객체를 lock)
  - 멀티 쓰레드를 사용하면서 생기는 문제점을 해결하기 위해 동기화 사용. 다만 동기화는 멀티 쓰레드의 장점(동시에 작업 수행)과 거리가 먼 방법. 즉 멀티 쓰레드의 문제점을 해결하기 위해 어쩔 수 없이 사용하는 것. 이러한 문제가 없다면 동기화는 가급적 사용 안 하는 것이 속도 면에서 좋다
- 동기화 방법
  - 메서드 동기화(synchronized method): 동시에 여러 쓰레드가 동기화 메서드 사용 불가(여러 쓰레드가 동시에 동기화 메서드를 실행할 수 없음)
  - 블록 동기화(synchronized block): 동시에 여러 쓰레드가 동기화 블록 사용 불가
- 메서드 동기화
  - 문법: 메서드의 리턴타입 앞에 synchronized 붙이면 끝
  ```java
  public synchronized void plusData(){
    // 구현부
  }
  ```
- 블록 동기화
  - 문법: synchronized (임의의 객체){ 동기화가 필요한 코드 블록 }
  - 임의의(아무) 객체: key를 가진 객체(모든 객체는 저마다의 key 하나를 가지고 있음. 따라서 진짜 아무 객체나 써도 상관없음). 일반적으로 클래스 내부에서 바로 사용할 수 있는 객체인 this를 사용
  ```java
  public void plusData(){
    synchronized(this){
      // 구현부
    }
  }
  ```
- 동기화 원리

  - 동기화되었다는 것은 단 하나의 key로 잠갔다는 개념. 동기화된 메서드나 블록은 하나의 key가 놓여있음. 이 때 어떤 쓰레드가 거기에 접근할 때 놓여잇는 key를 가지고 들어감. 가지고 들어가서 동기화된 곳 앞에는 key가 놓여져 있지 않고 들어간 쓰레드 안에 있음.
  - 이 때 다른 쓰레드가 앞의 쓰레드가 쓰고 있는 동기화된 곳에 접근함. 이 땐 앞에 key가 없음. 앞의 쓰레드가 다 완료한 후 key를 다시 돌려놓으면, 그 때 다른 쓰레드가 그 key를 가지고 들어가서 같은 작업을 처리.
  - 즉 모든 객체는 단 하나의 열쇠를 가지고 있음. 동기화를 사용하면 처음 사용하는 쓰레드가 객체의 key를 가짐. 다른 쓰레드는 먼저 사용 중인 쓰레드가 작업을 완료하고 열쇠를 반납할 때까지 대기(blocked)
    - 동기화 메서드: 자기자신의 객체(this)의 key를 사용
    - 동기화 블록: synchronized (임의의 객체){}에서 괄호 안에 넣은 객체의 key를 사용
  - 따라서 아래의 코드에서 어떤 쓰레드가 method1을 사용 중이면, 다른 쓰레드는 method1을 사용 못하는 건 당연하다. 이 뿐만 아니라 사용되는 메서드와 같은 key(예시에선 this)로 접근해야 하는 method2와 method3 안의 동기화 블록에도 접근 불가.

  ```java
  class myData{
    public synchronized void method1(){
      // 동기화 매서드
    }
    public synchronized void method2(){
      // 동기화 매서드
    }
    public synchronized void method3(){
      synchronized (this){
        // 동기화 블록
      }
    }
  }
  ```

<br><br>

# 04 (실습) 쓰레드 동기화

- "04 (이론) 쓰레드 동기화"와 중복되는 예제를 설명하고 끝이였음

<br><br>

# 05 (이론) 쓰레드의 상태 #1

- 쓰레드 상태
  - NEW: 쓰레드가 생성된 상태(Thread myThread = new MyThread()). start() 전 상태
  - RUNNABLE
    - NEW 상태에서 myThread.start()된 후 RUNNABLE 상태 됨.
    - 쓰레드가 cpu를 사용할 수 있는 상태
    - Thread.yield(): 어떤 쓰레드가 아직 cpu 사용할 필요 없을 때(ex- 아직 필요한 데이터가 없을 때), 다른 쓰레드에게 cpu 사용을 양보하고 자신은 실행 대기 상태로 전환됨.
  - TERMINATED: RUNNABLE 상태에서 run()이 완료된 후 TERMINATED(종료) 상태가 됨
  - 일시정지 상태
    - TIMED WAITING
      - RUNNABLE 상태에서 Thread.sleep(1000)이나 인스턴스.join(1000)할 때
      - sleep()와 join()
        - 모두 이를 호출한 쓰레드가 지정된 시간만큼 일시정지 됨.
        - 차이점: 예로 myThread.join(1000)을 하면, 이 메소드를 호출한 쓰레드는 일시정지되고, myThread에게 1000ms를 cpu에게 할당한다는 뜻.
      - interrupt(): 일시정지 상태가 풀려서 다시 RUNNABLE 상태가 됨. 즉 Thread.sleep(1000)해서 1000ms를 다 채우지 않아도 interrupt()를 호출하면 예외(InterruptedException)를 발생시킴. 그래서 다 채우지 않고 일시정지 상태에서 빠져나옴(ex- thread1.interrupt()를 호출하면 thread1에 InterruptedException 예외를 발생시켜 일시정지 상태를 탈출함)
    - BLOCKED: 사용하는 객체가 lock되면 BLOCKED 상태.
    - WAITING
      - join()과: TIMED WAITING의 myThread.join(1000)과 달리, myThread.join()을 하면 myThread한테 해당 쓰레드가 작업을 전부 완료할 때까지(종료될 때까지) cpu를 할당함. 그럼 나머지 쓰레드는 그동안 전부 WAITING 상태가 됨.
      - wait(), notify(), notifyAll(): Object의 메서드(즉 어느 객체나 해당 메서드를 가지고 있음). 사용은 동기화 블록에서만 사용 가능
- 쓰레드 상태값 가져오기
  - `State state = myThread.getState();`. 쓰레드의 상태값을 enum 타입인 Thread.State 객체로 리턴함.
  - enum 상수: NEW, RUNNABLE, TERMINATED, TIMED_WAITING, BLOCKED, WAITING

<br><br>

# 05 (실습) 쓰레드의 상태 #1

- "05 (이론) 쓰레드의 상태 #1"와 중복되는 예제를 설명하고 끝이였음

<br><br>

# 06 (이론) 쓰레드의 상태 #2

- "05 (이론) 쓰레드의 상태 #1"과 중복되지 않는 내용만 메모함
- wait()
  - wait()를 호출한 쓰레드는 일시정지 상태가 되며, 스스로 Runnable 상태가 될 수 없음. 다른 쓰레드가 notify(), nodifyAll()로만 깨울 수 있음. 깨워지면 다시 일시정지된 부분부터 쭉 실행됨(notify(), nodifyAll()로 깨워진 후, wait() 다음줄부터 실행됨)
  - 동기화 메서드, 동기화 블록에서만 사용 가능
- notify(), nodifyAll()
  - notify()는 하나의 쓰레드를, nodifyAll()은 wait() 중인 모든 쓰레드를 깨움.
  - 동기화 메서드, 동기화 블록에서만 사용 가능

<br><br>

# 06 (실습) 쓰레드의 상태 #2

- "06 (이론) 쓰레드의 상태 #2"와 중복되는 예제를 설명하고 끝이였음
