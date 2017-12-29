---
post: layout
title: "1장 운영체제 Introduction to Operating Systems"
category: "운영체제"
---

<img width="159" alt="2017-12-25 9 01 08" src="https://user-images.githubusercontent.com/19322354/34339447-796c93ce-e9b7-11e7-90e7-15a23c0bac4f.png">

#### 운영체제(커널)의 목적

- 컴퓨터의 **자원(하드웨어)의 효율적 관리** <— 가장 중요 

  (자원 : 프로세서, 기억장치, 입출력 장치)

- 사용자에게 컴퓨터를 편리하게 사용할수 있는 환경을 제공



####하드웨어 자원

CPU , 메모리 —> 주어진 자원내에 가장 효율(최대한의 성능)적인 사용

(하지만 너무 효율을 따지면 특정 사용자 or 프로그램은 손해를 볼 수 있음) --> 이러한 형평성도 운영체제가 관리



#### 소프트웨어 자원

프로세스 , 파일 , 메시지

<img width="611" alt="2017-12-25 11 50 35" src="https://user-images.githubusercontent.com/19322354/34340822-6e8ff8bc-e9ce-11e7-9ac0-8f3bb8806898.png">



#### 운영 체제의 분류

1.동시 작업 기능 여부

- 단일 작업

  *한 번에 하나의 작업만 처리*  (ex MS-DOS or 기능이 하나인 특수목적의 기계의 운영체제 - 엘리베이터)

  ​

- 다중 작업

  *동시에 두 개 이상의 작업 처리* (ex UNIX,MS WINDOWS , 스마트폰과 같은 범용 처리 기기)



2.사용자 수

- 단일 사용자

  MS-DOS, MS Windows



- 다중 사용자

  UNIX, NT server



3.처리 방식

- 일괄 처리

  작업 요청의 일정량 모아서 한꺼번에 처리

  작업이 완전 종료될 때까지 기다려야함

  <img width="500" alt="2017-12-26 12 00 28" src="https://user-images.githubusercontent.com/19322354/34340923-ff306176-e9cf-11e7-89aa-0b73a2f994ee.png">

- 시분할(time sharing) - *현재 사용하는 방법*

  여러 작업을 수행할 때 컴퓨터 처리 능력을 일정한 시간 단위로 분할하여 사용

  일괄 처리 시스템에 비해 짧은 응답 시간을 가짐

  interactive한 방식 (linux, windows, 매킨토시,안드로이드 앙앙)

  사용자가 많아질 수록 응답 시간이 느려질 수 있음 (but 실시간방식 처럼 DeadLine이 있는건 아님)

  ​

  <img width="456" alt="2017-12-26 12 01 46" src="https://user-images.githubusercontent.com/19322354/34340924-ffee8908-e9cf-11e7-81d0-8b7a6d59e51f.png">

- 실시간

  정해진 시간 안에 어떠한 일이 반드시 종료됨이 보장되어야 하는 실시간 시스템을 위한 os - Deadline





#### 용어정리

- Multitasking

  *cpu에서 매 순간 하나의 일만 하는데 엄청나게 빨리 와따리가따리 일하기 때문에 여러개가 일하는 것처럼 보임 ㅇㅈ? ㅇ ㅇㅈ*

- Multiprogramming

  *메모리에 여러 프로그램 들이 동시에 올라가는 것*

- Time sharing - 시분한

  *Multitasking과 유사*

- Multiprocess

위에 모두 비슷한 의미를 가짐

---

- Multiprocessor

  *하나의 컴퓨터에 CPU(processor)가 여러 개 붙어 있음을 의미* - 레알로다가 여러개의 일이 한번에 일함 (위에 용어들과는 다른 의미)



### 운영 체제의 예

- 유닉스 - (원래는 대형 컴퓨터 전용이였뜸)

  코드의 대부분을 C언어로 작성

  높은 이식성

  최소한의 커널 구조 - (메모리에 상주하는 부분)

  복잡한 시스템에 맞게 확장 용이

  소스 코드 공개

  프로그램 개발에 용이

  다양한 버전 (System V, FreeBSD, SunOS, Solaris, Linux)

  어셈블리어(기계어)로 운영체제를 만듦 - 어셈블리어로 운영체제를 만들기 때문에 만들어진 언어가 c언어



- DOS(Disk Operating System)

  MS사에서 1981년 IBM-PC를 위해 개발

  단일 사용자용 운영체제, 메모리 관리 능력의 한계(주 기억 장치 : 640KB)



- MS Windows

  MS사의 다중 작업용 GUI 기반 운영 체제

  Plug and Play, 네트워크 환경 강화

  DOS용 응용 프로그램과 호환성 제공

  풍부한 지원 소프트웨어



### 운영 체제의 구조

<img width="667" alt="2017-12-26 12 34 17" src="https://user-images.githubusercontent.com/19322354/34341151-88b319da-e9d4-11e7-9025-563a5ec3bb37.png">

