Spring의 Mjulti Thread, Singleton, Thread-safe
==============
<br>

## 1. Multi Thread
프로그램을 실행하고 여러 Client로부터 요청이 들어 왔을 때, 모든 Client가 자신이 원하는 작업을 할 수 있는 이유는 프로그램이 Multi Thread로 동작하기 때문이다. 때문에 대다수의 웹 서버는 Multi Thread로 동작하며 Spring의 Tomcat역시 이에 속한다고 한다.
<br>
여기서 중요한 점은 스프링부트가 다중 요청을 처리하는 것이 아니라, 스프링부트에 내장되어 있는 서블릿 컨테이너(Tomcat)에서 다중 요청을 처리한다는 것이다.
<br>
**Spring의 Thread pool**
사용자들이 접속할 때마다 쓰레드를 생성하고 소멸하는 것은 OS와 JVM에 많은 부담을 안겨준다. 이러한 단점을 해결하기 위해 Tomcat은 Thread pool을 사용한다.
![image](https://user-images.githubusercontent.com/81568304/169701230-b81e1f91-4440-47a2-926a-70756c529c3b.png)
<br>
**Thread pool의 기본 플로우**
1. 첫 작업이 들어오면, core size만큼의 Thread를 생성합니다.
2. 유저 요청(Connection, Server socket에서 accept한 소켓 객체)이 들어올 때마다 작업 큐에 담아둡니다.
3. core size의 스레드 중, 유효상태(idle)인 Thread가 있다면 작업 큐에서 작업을 꺼내 Thread에 작업을 할당하여 작업을 처리합니다.
3-1. 만약 유효 상태인 Thread가 없다면, 작업은 작업 큐에서 대기합니다.
3-2. 그 상태가 지속되어 작업 큐가 꽉 찬다면, Thread를 새로 생성합니다.
3-3. 3번 과정을 반복하다 Thread 최대 사이즈에 도달하고 작업 큐도 꽉 차게 되면, 추가 요청에 대해선 connection-refused 오류를 반환합니다.
4. task가 완료되면 Thread는 다시 유효 상태로 돌아갑니다.
4-1. 작업 큐가 비어있고, core size이상의 Thread가 생성되어 있다면 Thread를 destroy합니다.
<br>

## 2. Singleton
왜 Spring에서 모든 Bean들을 Singleton 객체로 생성될까? 위의 특징을 가지는 환경에서 Singleton 패턴을 사용하지 않는다면, 수 많은 사용자들로 인해 발생하는 요청을 Multi Thread로 생성된 수 많은 Thread가 처리하는 과정마다 필요한 객체를 생성해야 할 것이다. 이는 시스템적 성능 저하 및 굉장한 메모리 낭비가 발생하는 결과를 낳을 수 있다.
<br>
결국 Spring에서 Singleton 패턴을 제공하는 이유는 위와 같은 위험을 관리하기 위해 만든 모든 Bean을 Singleton 객체로 생성하여 모든 사용자들의 Thread가 공유할 수 있도록 만든 것이다.
<br>

## 3. Thread safe
그렇다면 Spring은 과연 Thread-safe할까?
<br>
아쉽게도 Spring은 기본적으로 Thread-safe하지 않은 환경이라고 한다. Spring Container내에서 하나만 존재하도록 보장하지만 그것이 Thread safe를 말하는 것은 아니며, 이러한 부분은 오히려 개발자가 직접 핸들링해줘야 한다.
<br>
앞서 Multi Thread 환경은 요청하는 Client에 따라 Thread pool에서 Thread를 생성해 처리한다고 했는데, 여기서 Thread가 가지는 특징이 변수를 저장하는 Stack 영역은 각 Thread별로 할당되지만, 동적 할당된 객체를 저장하는 Heap 영역과 함께 Code, Data 영역은 공유한다는 것이다.
<br>
Singleton이 Multi Thread 환경에서 서비스 형태의 오브젝트로 사요되는 경우에는 stateless 방식으로 만들어져야 한다.  이때는 읽기 전용 값이라면 초기화 시점에 인스턴스 변수에 저장해두고 공유하는 것은 문제 없다. 만약 각 요청에 대한 정보나, DB 서버의 리소스로 부터 생성한 정보는 파라미터와 로컬 변수, 리턴 값을 이용하면 된다. 메소드 파라미터나, 메소드 안에서 생성되는 로컬 변수는 매번 새로운 값을 저장할 독립적인 공간이 만들어지기 때문에 싱글톤이라고 해도 문제없다.
<br>
## 참고자료
1.  [](https://fbtmdwhd33.tistory.com/256?category=870663)[https://fbtmdwhd33.tistory.com/256?category=870663](https://fbtmdwhd33.tistory.com/256?category=870663)
2.  [](https://velog.io/@sihyung92/how-does-springboot-handle-multiple-requests)[https://velog.io/@sihyung92/how-does-springboot-handle-multiple-requests](https://velog.io/@sihyung92/how-does-springboot-handle-multiple-requests)
3.  [](https://dahye-jeong.gitbook.io/spring/spring/2020-04-09-bean-threadsafe)[https://dahye-jeong.gitbook.io/spring/spring/2020-04-09-bean-threadsafe](https://dahye-jeong.gitbook.io/spring/spring/2020-04-09-bean-threadsafe)

