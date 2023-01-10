---
title: "[Do it! 자바 완전정복 - 15장 쓰레드] '01 (이론) 쓰레드의 개념과 필요성' ~ '03 (실습) 쓰레드 속성'"
excerpt: "쓰레드의 개념과 필요성 / Thread의 생성 및 실행방법 / 쓰레드 속성"

categories:
  - Learning-Java
tags:
  - [Java Lecture]

toc: true
toc_sticky: true
toc_label: "페이지 주요 목차"

date: 2023-01-08
last_modified_at: 2023-01-08
---

<br>

# [유튜브 강의 주소](https://www.youtube.com/watch?v=vKgDXrN5zAQ&list=PLR9w0n2BH7rci0nw5SGiYN0-Ded98-Ew5)

<br>

# 보는 방법

블로그에 메모한 내용만으로 복습하기에는 무리. 복습할 땐 강의 영상 보고, 이건 상기용/테스트용. 메모한 거 보면서 강의 내용이 떠오르는지 등을 점검하는 데 활용.

<br><br>

# 01 (이론) 쓰레드의 개념과 필요성

- Pragram vs. Process vs. Thread 1
  - 프로그램은 하드 디스크에 저장된 거, 프로세스는 메모리에 로딩된 프로그램
  - 멀티 프로세스: 로딩을 두 번 실행할 때(ex- 한글 파일 창을 두 개 띄울 때)
  - 쓰레드는 cpu를 사용하는 최소 단위(왜냐? cpu는 하드 디스크와 얘기하지 않고 메모리하고만 데이터를 주고 받음 => cpu는 memory 내의 프로세스와만 얘기 => cpu는 memory 내의 프로세스 속에 있는 쓰레드와만 얘기)
  - 단일 쓰레드 프로세스, 멀티 쓰레드 프로세스: 하나의 프로세스에 하나의 쓰레드가 있으면 단일 쓰레드 프로세스, 하나의 프로세스에 여러 개의 쓰레드가 있으면 멀티 쓰레드 프로세스
- 자바에서 Thread
  - .class 파일 실행(JVM은 main() 메소드 찾아서 실행해 줌) => main Thread 생성됨(시작 시점에는 단일 쓰레드(main thread만 존재) 생성됨 / 실행하기 위해선 CPU를 사용해야 하니까, 먼저 쓰레드를 생성해서 쓰레드가 cpu를 사용하게 함. 생성한 쓰레드 위에서 main 메소드를 돌리면 main 메소드가 cpu를 사용하게 되는 것임)
  - 이후 main thread에서 다른 thread 생성/실행하면 멀티 쓰레드가 됨
- multi-thread의 필요성
  - 여러 일이 동시에 진행될 필요성이 있을 때 해결책이 멀티 쓰레드(ex- 비디오 프레임과 자막이 동시에 처리돼서 프레임에 맞는 자막이 나와야 할 때)
  - 멀티 쓰레드는 어떻게 작업을 동시에 수행할까?
    - 동시성(concurrency)
      - 작업 수 > cpu 코어 수일 때
      - 즉 작업 두개인데, cpu는 한 개야(cpu 코어도 한 개야). 그럼 각 작업을 짧은 단위로 나누고 각 작업 단위를 계속 교차해서 처리함. 그럼 동시에 하는 것처럼 보임.
      - 즉 동시에 작업을 수행하는 것은 아니고, 따라서 이 경우는 멀티 쓰레드로 여러 작업을 동시에 한다고 해서 총 작업 처리 시간이 단축되는 것은 아님.
    - 병렬성(parallelism)
      - 작업 수 < cpu 코어 수일 때
      - 작업 두 개인데, cpu 코어도 두 개야. 그럼 각 작업을 각 코어에 할당해서 처리하면 됨.
      - 즉 이 때는 동시에 여러 작업을 처리하는 것임. 따라서 총 작업 처리 시간도 줄어듦(멀티 코어의 경우, 멀티 쓰레드로 작업을 처리하면 작업 소요 시간이 실제로 줄어듦)

<br><br>

# 01 (실습) 쓰레드의 개념과 필요성

- "01 (이론) 쓰레드의 개념과 필요성"와 중복되는 예제를 설명하고 끝이였음

<br><br>

# 02 (이론) Thread의 생성 및 실행방법

- 쓰레드 생성 및 실행 방법
  - 생성 방법 1
    - Thread class를 상속받아 run() 메소드를 재정의해서 자식 클래스를 만들어서 사용
  - 생성 방법 2
    - Runnable interface를 구현(추상메서드 run()을 구현(implements))한 클래스를 만든다. 그 후 Thread 객체를 생성할 때 해당 생성자의 매개변수에 Runnable을 구현한 클래스의 객체를 넘겨서 Thread 객체를 만들면 됨
  - 실행 방법
    - Thread class의 start() 메서드를 호출하면 됨
    - Thread 객체는 한 번만 사용 가능하고, 재사용할 수 없음. 즉 각 thread 객체가 start()를 한 번밖에 호출할 수 없다.
  - run()을 재정의했지만 start()로 thread를 실행하는 이유?
    - 새로운 쓰레드를 생성/추가하려면 사전 준비가 많이 필요함. 이런 사전 준비를 다 하고, 새로운 쓰레드 위에 run()을 실행함. 이렇게 "사전 준비 + run() 실행"을 합쳐서 start() 메소드가 맡음
- 익명 내부 클래스를 통한 쓰레드 생성

  ```java
  public static void main(String[] args){
    // thread 생성
    Thread myThread = new Thread(new Runnable(){

      @Override
      public void run(){
        // run 내부 구현
      }
    })
  }
  ```

<br><br>

# 02 (실습) Thread의 생성 및 실행방법

- "02 (이론) Thread의 생성 및 실행방법"와 중복되는 예제를 설명하고 끝이였음

<br><br>

# 03 (이론) 쓰레드 속성

- thread 객체 가져오기
  - `Thread curThread = Thread.currentThread()`
  - Thread 클래스의 정적 메서드
  - Thread 참조 객체가 없는 경우 사용
    - ex1) main thread는 jvm에서 자동으로 만든 쓰레드. 이 때 참조 객체가 없음
    - ex2) (new Thread).start()할 때. `Thread a = new Thread();`에서 a와 같은 게 없음
- 실행 중인 Thread 개수 가져오기
  - Thread.activeCount()
  - Thread 클래스의 정적 메서드
  - 정확히는 같은 ThreadGroup 내의 thread 수를 가져옴. 생성된 쓰레드는 자신을 생성한 쓰레드와 같은 그룹에 속함
- 쓰레드 이름 설정 및 가져오기
  - setName()과 getName()
  - Thread 클래스의 인스턴스 메서드
  - 쓰레드의 이름을 지정하지 않는 경우 thread-1, thread-2 ... 와 같이 번호를 하나씩 증가시키면서 이름 자동 부여
- 쓰레드 우선순위 정하기 및 가져오기
  - setPriority()와 getPriority()
  - Thread 클래스의 인스턴스 메서드
  - 우선순위에서 최댓값은 Thread.MAX_PRIORITY으로 10이고, 기본값은 Thread.NORM_PRIORITY로 5이고, 최솟값은 Thread.MIN_PRIORITY로 1이다.
  - 우선순위란 동시성(concurrency)일 때 할당되는 time slot(시간 간격)에 비례하는 값. 즉 우선순위가 다르다는 뜻은 우선순위가 높은 작업의 time slot을 더 많이 할당하고, 다른 작업은 더 적게 할당한다는 뜻. 따라서 우선순위가 높은 작업이 더 먼저 끝나는 경우가 많을 것이다.
  - 단 항상 우선순위가 높은 게 먼저 끝나지 않는 경우도 간혹 있음. 또한 우선순위가 같은 경우 여러 쓰레드를 순서대로 start() 하면 실행한 순서대로 처리될 거라 예상하는데 그렇지 않음. 이론상으로는 맞지만, cpu 스케줄링에 따라, 또 특정 순간에 부하가 걸리는 경우 등의 여러가지 문제가 원인으로 자기가 할당받은 time slot을 다 못 채워서 순서가 바뀔 수 있다.
  - 단 병렬성일 때, 예로 코어가 4개인데 작업은 두개일 때, 우선순위는 의미없음. 왜냐면 각 작업이 각 코어에서 실행되니까 time slot으로 쪼갤 필요가 없지
- 쓰레드 데몬 설정
  - setDaemon(). `myThread.setDaemon(true);`라 하면 daemon thread가 됨. 주의점으로 start() 전에 이 메서드를 호출해야 함.
  - Thread 클래스의 인스턴스 메서드
  - 일반 쓰레드(user thread / 기본값): 일반쓰레드는 주쓰레드(main) 종료여부와 상관없이 자신의 쓰레드가 종료되어야 프로세스가 종료됨.
  - 데몬 쓰레드: 데몬쓰레드는 main쓰레드를 포함해서 모든 일반쓰레드(사용자쓰레드)가 종료되면, 작업이 완료되지 않아도 함께 종료됨.

<br><br>

# 03 (실습) 쓰레드 속성

- "03 (이론) 쓰레드 속성"와 중복되는 예제를 설명하고 끝이였음
