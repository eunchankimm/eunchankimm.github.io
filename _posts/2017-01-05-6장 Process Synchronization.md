---
layout: post
title : "6장 Process Synchronization"
category: "운영체제"
tag: [운영체제,취업]
---

## 6장 Process Synchronization

###데이터 접근

![2018-01-05 1 29 11](https://user-images.githubusercontent.com/19322354/34595580-7aa45a0c-f21c-11e7-9b71-583d7422ad63.png)

> Data를 읽는 순서에 따라서 결과 값이 바뀔수 있기 때문에 이에 대한 문제를 Synchronization문제라 한다



![2018-01-05 1 29 17](https://user-images.githubusercontent.com/19322354/34595581-7b4afff6-f21c-11e7-8df0-c3495049fdd9.png)

> Race Condition = 경쟁 상태   —> 이것을 조율해줘야 하는 어떠한 해결 방법이 필요하다!

프로세스는 일반적인 경우라면 자기 주소공간만 접근하기 때문에 이런 race condition 은 발생하지않지만, 프로세스가 본인이 실행할수 없는부분은 운영체제에게 대신 system call을 하여 커널의 코드가 그 프로세스대신 실행이 되는데, 커널의 코드에 접근 할때, 커널의 변수를 쓴다는건데 이때 만약 cpu를 뺏기고 다른 프로세스에게 cpu가 넘겨졌는데, 이 프로세스도 만약 system call을 하여 똑같은 커널의 코드에 접근하고 변수를 쓴다면 이때 race condition이 발생



### Race condition 이 발생하는 3가지

1. kernel 수행 중 인터럽트 발생 시
2. Process가 System call을 하여 kernel mode로 수행 중인데 context switch가 일어나는 경우
3. Multiprocessor에서 shared memory 내의 kernel data



![2018-01-05 2 43 04](https://user-images.githubusercontent.com/19322354/34596905-c47dae26-f226-11e7-8466-2cc23b6f7f62.png)



 프로세스가 커널모드에서 운영체제를 통해 진행중일 때 interrupt가 들어와도 멈추지않고, 일단 커널모드의 프로세스가 완료 된 후,  interrupt 가 실행되게 끔 함



![2018-01-05 2 47 38](https://user-images.githubusercontent.com/19322354/34597008-6e23944a-f227-11e7-9a59-d1a27c295840.png)



![2018-01-05 2 47 46](https://user-images.githubusercontent.com/19322354/34597009-6eeda74e-f227-11e7-84bc-6bf0ac3a888f.png)



preempt : 뺏기지 아니한다



![2018-01-05 2 47 53](https://user-images.githubusercontent.com/19322354/34597011-6fe446d0-f227-11e7-80f0-c0af25f4a660.png)





![2018-01-05 3 34 51](https://user-images.githubusercontent.com/19322354/34598070-fe982fc6-f22d-11e7-94e9-3c6250951367.png)



![2018-01-05 3 37 39](https://user-images.githubusercontent.com/19322354/34598157-6dd6552a-f22e-11e7-8ffd-0ab43d003b96.png)



![2018-01-05 3 37 48](https://user-images.githubusercontent.com/19322354/34598160-6e7f59b8-f22e-11e7-8c94-ce690c9b3746.png)



![2018-01-05 3 37 55](https://user-images.githubusercontent.com/19322354/34598161-6f440a74-f22e-11e7-86d6-f18a31221bba.png)

>Bounded Waiting - critical section에 들어가고자 하는 process가 세개가 있는데, 두개가 번갈아 가면서 사용을 할 때 를 방지





<img width="793" alt="2018-01-08 3 52 43" src="https://user-images.githubusercontent.com/19322354/34661311-36775e66-f48c-11e7-9b50-b495ff64503b.png">

> 공유데이터를 그냥 접근하면 동시접근 때문에 문제가 발생
>
> 공유데이터 접근 이전에 entry section을 통해 lock을 걸어 공유데이터 접근을 막고,
>
> 공유데이터를 다 쓰면, lock을 푼다음에 다른 프로세스가 critical section에 접근 할수 있게 한다.

---

### Software 적으로 lock을 푸는 Algorithm 1

<img width="823" alt="2018-01-08 3 55 48" src="https://user-images.githubusercontent.com/19322354/34661356-71a2b62a-f48c-11e7-986a-f3da70170c36.png">

> turn = 0이면 0번 process 차례, turn = 1 이면 1번 process 차례 라는 뜻
>
> 상대편이 critical section에 들어가야만 내가 critical section에 들어갈 수 있는데 이  알고리즘은 progress(진행) 에 대한 문제가 있다. 왜냐하면 0번 process는  critical section을 계속들어가고 싶고, 1번 process는 1번만 들어간다고 하면, 0번 process는 critical section을 1번 말고는 더이상 들어 갈 수 없다.



### Software 적으로 lock을 푸는 Algorithm 2

<img width="876" alt="2018-01-08 3 55 54" src="https://user-images.githubusercontent.com/19322354/34661357-7272d986-f48c-11e7-9d8a-6c1253476576.png">

> 만약 process P가 critical section에 들어가고 싶으면 flag를 true로 바꾼후, 들어 갈때 상대방이 critical section을 들어 가고 싶은지 확인.  만약 상대방이 true로 바꿔놨으면, 기다린다.
>
> critial section을 사용 후 false로 다시 바꿈
>
> 이 Algorithm에 문제는 process p가 flag를 true로 바꾸면 ,  cpu를 빼앗기고 다른 process에게 준다음 만약, 이 때 flag를 true로 바꿔준다면 , 계속 cpu를 빼앗기는 상황에 둘다 flag가 true이기 때문에 critical section에 아무도 못 들어간다.



### Software 적으로 lock을 푸는 Algorithm 3 (Peterson's 			Algorithm)

<img width="790" alt="2018-01-08 4 13 46" src="https://user-images.githubusercontent.com/19322354/34661726-ebeeeeb0-f48e-11e7-8f37-248e79ce9965.png">

> 첫번째 알고리즘과 두번째 알고리즘의 변수 모두 사용 (flag , turn)
>
> 만약 process P 가 critical section에 들어가려고 할 때, flag를 true로 하며 turn을 상대방으로 바꾸어 놓는다.  이 때 상대방이 flag가 true이고, 상대방의 턴이면 기다린다. 그 나머지 경우에는 자기가 critical section에 들어간다.
>
> 둘다 critical section에 들어가고 싶을 경우 turn을 따져서 들어 가는 것,
>
> 만약 process 가 한개만 critical section에 들어가려고 하면, turn 신경 안쓰고 그냥 들어감
>
> Spin lock ! = 어떤 process가 critical section에 들어가 있는 상태에서 , 다른 process가 while문을 계속 돌면서 critical section에들어 갈 수 있는지 확인을 한다. cpu를 잡을 상태에서 헛 짓을 하고 있다는 것! , 
>
> (중간중간에 cpu를 빼앗길수 있는 상황을 고려해서 code가 짜여져있기 때문에 복잡한것)

---

###Hardware 적으로 lock을 걸고 푸는 법

<img width="811" alt="2018-01-08 4 44 22" src="https://user-images.githubusercontent.com/19322354/34662334-368bcdf4-f493-11e7-8f5f-6fa528f77dd1.png">

> 만약 하나의 instruction을 가지고 어떤 데이터를 읽고 쓰는 작업을 동시에 실행 할 수 있다면,
>
> 하드웨어적인 instruction을 준다면 간단하게 lock의 문제를 해결할 수 있다.



#### 세마포어

<img width="734" alt="2018-01-10 12 13 35" src="https://user-images.githubusercontent.com/19322354/34754217-bec6b1c6-f5ff-11e7-9a29-9069586d443e.png">

> 추상 자료형 : object + operation  
>
> ex) 세마포어 자료형(S) {세마포어는 정수값을 가질수 있다} 는  P 와 V의 연산 방법이 있다.
>
> 세마포어를 쓰는 이유는 lock을 걸고 푸는 걸 세파모어를 이용해서 간단하게 프로그래머에게 제공
>
> 다른 방향으로는 공유 자원을 획득하고 반납하는 것을 처리해 줌
>
> P 연산은 공유 자원을 획득 하는 과정   ,    V 연산은 공유 자원을 반납 하는 과정
>
> S는 공유 자원의 갯수



<img width="876" alt="2018-01-10 2 11 52" src="https://user-images.githubusercontent.com/19322354/34756739-3fb4531e-f610-11e7-895c-d81f770ace34.png">



<img width="855" alt="2018-01-10 2 13 59" src="https://user-images.githubusercontent.com/19322354/34756787-8b567ba8-f610-11e7-9502-f63f2c3a500d.png">







<img width="866" alt="2018-01-10 2 14 06" src="https://user-images.githubusercontent.com/19322354/34756790-8c1dcd3e-f610-11e7-9b22-d159df526196.png">

> P - 자원의 획득
>
> V - 자원의 획득 , 혹시 반납한 자원을 기다리는 잠들어 있는 process가 있다면 깨워주는 역할도 함

<img width="746" alt="2018-01-10 2 31 36" src="https://user-images.githubusercontent.com/19322354/34757194-fd1713e0-f612-11e7-8eb6-e1b77f19a4ea.png">



<img width="564" alt="2018-01-10 2 47 37" src="https://user-images.githubusercontent.com/19322354/34757535-40d4cd6e-f615-11e7-8dc9-24a98df0276b.png">

>Binary semaphore - 자원의 경우가 하나 인 경우 
>
>Counting semaphore - 자원의 갯수가 여러개 있어서 여분이 있을 때 가져다 쓸수 있을 때



<img width="843" alt="2018-01-10 2 47 52" src="https://user-images.githubusercontent.com/19322354/34757537-43a80f42-f615-11e7-969f-d28ac4daf0cb.png">

> 예를 들어 disk A 에서 읽어서 disk B에 쓰기를 하려면 자원 S 와 Q를 써야하는데, P0 이라는 process가 S를 p1이라는 process가 Q를 가지고 있으면 서로 무한정 기다리게 된다.
>
> 해결방법으론 S와 Q를 의 자원을 사용하려면 무조건 S부터 얻은 다음에 Q를 사용하게 하는 것

---



### Classical Problems of Synchronization

- Bounded-Buffer Problem (Producer-Consumer Problem)
- Readers and Writers Problem
- Dining - Philosophers Problem

###Bounded-Buffer Problem (Producer-Consumer Problem)

<img width="931" alt="2018-01-12 3 15 36" src="https://user-images.githubusercontent.com/19322354/34862053-8f346928-f7ab-11e7-9e92-b9b3390ac3e0.png">

>Buffer - 임시로 데이터를 저장하는 공간
>
>Bounded Buffer - 버퍼의 공간이 유한하다
>
>Producer - 공유 버퍼에 데이터를 하나 만들어서 집어 넣는 역할
>
>Consumer - 
>
>**Problem**
>
>​	비어 있는 버퍼에 생산자 2개가 동시에 접근하면 안되기 때문에 LOCK을 걸고 작업
>
>​	소비자 가 동시에 접근 하는 것도 같이 LOCK을 검
>
>​	Buffer가 유한하기 때문에 생산자들이 계속 데이터를 넣어서 full - buffer가 된다면 더이상 데
>
>​	이터를 넣을 공간이 없기 때문에 , 강제적으로 소비자를 기다려야 함
>
>여기서 세마포어에 필요한 변수는 (Lock , 현재 buffer의 사용량 표시 Count)

<img width="796" alt="2018-01-12 3 15 45" src="https://user-images.githubusercontent.com/19322354/34862056-9072d18a-f7ab-11e7-9372-122ed06e46b7.png">

> mutex - 공유 버퍼에게 하나의 프로세스만 접근하도록 하는 세마포어

```c++
Producer
do{
	데이터 x 생성
	P(empty)	-    비어 있는 버퍼 획득
	P(mutex)	-    버퍼에 lock 검
	buffer에 데이터를 집어넣은 후
	V(mutex)	-	 버퍼에 lock을 품
	v(full)		-	 데이터가 들어가 있는 버퍼를 놓아줌
}
```



###Readers-Writers Problem

<img width="821" alt="2018-01-12 3 15 52" src="https://user-images.githubusercontent.com/19322354/34862057-92011dae-f7ab-11e7-9e8c-cb951d67766e.png">

> DB = 공유데이터
>
> process = DB에 데이터를 쓰면 Writer , DB에 데이터를 읽으면  Reader
>
> 여기서 Writer 와 Reader의 차이점은 Writer은 동시에 여러개가 하면 X
>
> 그러나 Reader은  여러개가 한번에 DB에 접근해도 됨



<img width="843" alt="2018-01-12 3 15 57" src="https://user-images.githubusercontent.com/19322354/34862059-9323272c-f7ab-11e7-85e8-3f699b7cea30.png">

> DB에 대한 접근에 대한 lock의 변수는 = db
>
> readcount도 공유변수이기 때문에 lock을 건 필요가 있으므로 그걸 위해 mutex를 사용
>
> 이 때 Reader가 계속 들어와 Writer가 Starvation(기아) 가 발생 할 수 있다





<img width="775" alt="2018-01-12 4 05 15" src="https://user-images.githubusercontent.com/19322354/34863382-7a80b0e8-f7b2-11e7-9070-d1fb0e090b3c.png">





