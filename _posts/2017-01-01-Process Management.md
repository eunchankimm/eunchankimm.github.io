---
layout: post
title: "4. Process Management - 1"
category: "운영체제"
tag: [운영체제,취업]
---



## 4. Process Management - 1

#### 프로세스 생성

<img width="831" alt="2018-01-01 2 36 48" src="https://user-images.githubusercontent.com/19322354/34465767-3c5f1cfa-ef01-11e7-808a-9cf08db7cfb7.png">

프로세스 실행 -> CPU + 메모리 필요 

wate == block



<img width="715" alt="2018-01-01 2 39 45" src="https://user-images.githubusercontent.com/19322354/34465780-a91da55a-ef01-11e7-8fa3-9cef84104134.png">

자식은 부모를 그대로 복사 = 주소공간 + 운영체제 데이터(PCB + 자원들)(fork())  => 그 후 새로운 프로그램올림(exec())

- 프로세스 문맥을 복사 == fork()
- 리눅스 같은 효율적인 운영체제는 처음부터 프로세스 문맥, 즉 fork()를 바로 하지않고, PC만 하나 만들어서 부모 프로세스와 같은 곳을 instruction을 가리키고 있다가 만약에, 자식 프로세스가 수행하면서 부모와 다른값들이 stack,data에 저장 되야 한다면(write) 그 때 fork()로 부모 프로세스의 주소 공간을 복사하여 자기만의 공간을 만든다 ==> Copy-on-write (COW) - 내용이 바뀔때 그 때 복사

exec()를 안할수도 있고, exec()을 하면 완전 딴 프로그램이 될 수도 있다

fork() 와 exec()는 system call 이기 때문에 운영체제가 해줘야함



### 프로세스 종료

<img width="738" alt="2018-01-01 2 44 08" src="https://user-images.githubusercontent.com/19322354/34465789-4f3f79f4-ef02-11e7-929e-1e99abe8544a.png">



### fork() - System Call

<img width="822" alt="2018-01-01 2 49 39" src="https://user-images.githubusercontent.com/19322354/34465810-0c774baa-ef03-11e7-84c1-0fcb5ec5dfc0.png">

만약 pid = fork(); 를 해서 자식 프로세스가 만들어지면 자식 프로세스가 pc에 pid = fork()를 가리키기 때문에 main() 부터 시작하는게 아니라 자식 프로세스는 pid = fork()부터 시작함

여기서 fork()는 부모면 양수를 리턴하고 자식이면 0을 리턴하기 때문에 이걸로 자식과 부모를 구분함



### exec() - System Call

<img width="745" alt="2018-01-01 2 49 46" src="https://user-images.githubusercontent.com/19322354/34465809-0c4baf4a-ef03-11e7-8960-d75c51639b0a.png">

```d
int main()
{
  printf("\n Hello, I am child! Now I'll run data \n");
  execlp("/bin/date", "/bin/date", (char *)0);
  printf("\n Hello, I am parent!\n");
}
```

exec()를 꼭 자식 프로세스를 만들고 새로운 프로그램으로 바꾸는데 쓰지 않고, 위에 처럼 써도 되는데,

위에 처럼 쓰면 printf("\n Hello, I am child! Now I'll run data \n"); 이 실행되고 execlp 때문에 새로운 프로그램으로 바뀌게 되는데,  이럴 경우 새로운 프로그램으로 바뀐 것이기 때문에 밑에  printf("\n Hello, I am parent!\n"); 부분은 실행 되지 않는다.

   

### wait() - System Call

<img width="823" alt="2018-01-01 3 36 40" src="https://user-images.githubusercontent.com/19322354/34466020-ad01b14a-ef09-11e7-893e-c5c78d0c7632.png">



### exit() - System Call

<img width="830" alt="2018-01-01 3 36 53" src="https://user-images.githubusercontent.com/19322354/34466021-b059d200-ef09-11e7-941b-3b2c3ddfe6a7.png">



### 프로세스와 관련한 System Call

- fork()    -  자식 프로세스가 부모 프로세스의 주소 공간을 복사 하는 것 ! (COW도 알 아 둘 것!)



- exec()   -  fork()만 한 상태는 부모와 자식이 아예 똑같은 상태이다. exec()를 통해 자식은 부모와는 					   다른 프로세스가 되는 것인 것이드아!



- wait()    -  I/O 와 같은 오래 걸리는 작업을 할 때 프로세스는 자식 프로세스를 만들고, I/O 관련 일을 시키고, 자기는 wait(block) 상태가 된다음 자식 프로세스가 일을 끝날 때까지 기다린다. 자식 프로세스가 일을 끝맞치면 다시 ready 상태로 돌아간드아!



- exit()    -  이놈이 바로 아주 무서운 프로세스 종료시키는 놈이다. 원래 같으면 statement 끝에 자동으로 컴파일러가 해주기 때문에 안넣어줘도 되는데,  프로세스가 진행될떄 죽이는 방법은 리눅스에서 kill , break가 있고, 만약 부모와 자식 프로세스가 있는데 , 부모 프로세스를 종료시키면, 자식 부터 종료되며 부모가 마지막에 종료된다.





### Process 간 협력

<img width="807" alt="2018-01-01 4 06 16" src="https://user-images.githubusercontent.com/19322354/34466151-bc66eea8-ef0d-11e7-8773-0a30693d1b4d.png">



### message passing

<img width="756" alt="2018-01-01 4 07 05" src="https://user-images.githubusercontent.com/19322354/34466163-dba34d8e-ef0d-11e7-8b06-fb027462642c.png">

프로세스간의 정보를 전달 하려면 Message Passing을 해야하는데 이때, 두 가지 방법이 있는데,

하나는 메시지를 주려는 process를 명시하는 **direct** 와 Mailbox에 넣어서 가져가고 싶은 프로세스 아무나 가져가라는 식의 **indirect** 방식이 있다. 



<img width="772" alt="2018-01-01 4 07 12" src="https://user-images.githubusercontent.com/19322354/34466164-dc8cc784-ef0d-11e7-8ea6-1900967c3319.png">

프로세스 간의 일부 공유하는 메모리 공간을 만들어 두고 메시지를 전달 받는 방법이 **Shared Memory**

이것도 커널에게 부탁해서 해야함