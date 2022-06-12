# OSI 7계층

### 참고 자료
1. https://github.com/gyoogle/tech-interview-for-developer/blob/master/Computer%20Science/Network/OSI%207%20%EA%B3%84%EC%B8%B5.md
2. https://velog.io/@jeongs/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-OSI-7-%EA%B3%84%EC%B8%B5-%EA%B7%B8%EB%A6%BC%EA%B3%BC-%ED%95%A8%EA%BB%98-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0

<br>
<img src="https://user-images.githubusercontent.com/81568304/173191676-475787db-6481-4502-b19a-180c9997056e.png">
<br><br><br>


### 7계층은 왜 나눌까?
<p>
통신이 일어나는 과정을 단계별로 알 수 있고, 특정한 곳에 이상이 생기면 그 단계만 수정할 수 있기 때문이다.
</p>

### 1. 물리 (Physical)
<p>
<img src="https://user-images.githubusercontent.com/81568304/173191962-579d07c0-4abc-4ef7-97cc-26dd75548d7e.png">
<strong>리피터, 케이블, 허브 등</strong>
<br/>
단지 데이터 전기적인 신호로 변환해서 주고 받는 기능을 진행하는 공간
<br/>
즉, 데이터를 전송하는 역할만 진행한다. 0, 1의 나열을 아날로그 신호로 바꾸어 전선으로 흘려 보내고, 아날로그 신호를 0, 1의 나열로 해석하는 역할을 하는 것이 Physical Layer가 하는 역할이다.
</p>
<br/>

### 2. 데이터 링크 (Data Link)
<p>
<img src="https://user-images.githubusercontent.com/81568304/173192198-cc5a30a2-454d-428e-83c8-0ce5bb64df02.png">
<img src="https://user-images.githubusercontent.com/81568304/173192219-74f7de2a-33b1-42ef-9f30-d0d91ea734b3.png">
<strong>브릿지, 스위치 등</strong>
<br>
물리 계층으로 송수신 되는 정보를 관리하여 안전하게 전달되도록 도와주는 역할을 한다.
<br>
이 계층에서 전송되는 데이터 단위를 프레임이라 부르며, 직접 연결된 컴퓨터와의 통신만을 다루므로, 이웃의 컴퓨터를 넘어가는 통신은 데이터 링크 계층에선 관여하지 않는다.
<br>
Mac 주소를 통해 통신한다. 프레임에 Mac 주소를 부여하고 아래와 같은 역할들을 수행한다.

- 프레이밍 : Physical Layer를 통해 받은 신호를 조합해 Frame 단위의 데이터 유닛으로 만들어 처리
- 흐름제어 : 데이터를 송수신 시, 너무 많거나 적은 데이터를 송수신하지 않도록 흐름제어
- 오류제어 : 프레임 전송 시 발생한 오류를 복원하거나 재전송
- 접근제어 : 매체 상 통신 주체가 여러 개 존재할 때, 데이터 전송 여부 결정
- 동기화 : 프레임 구분자 (특별한 bit 패턴)
</p>
<br>

### 3. 네트워크 (Network)
<p>
<img src="https://user-images.githubusercontent.com/81568304/173192532-d5a50a7c-6971-4f53-b6ad-d64bee4d9135.png">
<strong>라우터, IP</strong>
<br>
데이터를 목적지까지 가장 안전하고 빠르게 전달하는 기능을 담당한다.
<br>
이 계층에서 전송단위는 패킷이라 부른다.
<br>
라우터를 통해 이동할 경로를 선택하여 IP 주소를 지정하고, 해당 경로에 따라 패킷을 전달해준다.
<br>
라우팅, 흐름제어, 오류제어, 세그먼테이션 등을 수행한다.
</p>
<br>

### 4. 전송 (Transport)
<p>
<img src="https://user-images.githubusercontent.com/81568304/173192678-cb24180b-c3ca-4319-9467-c50aaab0a955.png">
<strong>TCP, UDP</strong>
<br>
TCP와 UDP 프로토콜을 통해 통신을 활성화한다. 포트를 열어두고, 프로그램들이 전송을 할 수 있도록 제공해준다.
<br>
즉, Transport 계층에서는 사용자들이 신뢰성있는 데이터를 받을 수 있도록 전송 속도를 조절하거나, 오류가 발생하면 맞춰주는 역할을 한다.

- TCP : 신뢰성, 연결지향적
- UDP : 비신뢰성, 비연결성, 실시간
</p>
<br>

### 5. 세션 (Session)
<p>
<strong>API, Socket</strong>
<br>
1계층 ~ 4계층의 주된 기능은 데이터를 전달하는 것이다.
<br>
5계층부터는 데이터를 송수신하는 양쪽 컴푸터 내의 프로세스들 간의 통신 프로토콜이라 볼 수 있다.
<br><br>
<strong>1) 세션</strong>
<br>

- 네트워크 환경에서 사용자 간 또는 컴퓨터 간의 대화를 위한 논리적 연결
	- 물리적 연결 : 케이블을 통해 직접 연결되는 통신
	- 논리적 연결 : 물리적 연결 외에 IP(논리주소), MAC  address(물리주소) 등을 통해 통신
- 프로세스들 사이에 통신을 수행하기 위해서 메시지 교환을 통해 서로를 인식 한 이후부터 통신을 마칠 때까지의 기간.

<br>
세션 계층에서는 응용 프로그램 간의 통신을 하기 위한 세션을 운영체제를 통해 확립, 유지, 중단하는 작업을 수행한다.
<br>
즉, 응용 프로그램들 간의 접속을 설정, 유지하고 끊어질 경우 데이터를 재전송하거나 연결을 복구한다.
<br><br>
<strong>2) 동기화</strong>
<img src="https://user-images.githubusercontent.com/81568304/173193214-e6b771cd-99a0-4ca4-8825-83caddb0df96.png">
세션 계층의 중요한 기능에는 동기화가 있다. 동기란 통신 양단에서 서로 동의하는 논리적인 공통처리 지점으로써, 동기점을 설정하기 위해 사용된다.

동기점이 설정된다는 의미는 그 이전까지의 통신은 서로 완벽하게 처리했다는 것을 뜻한다.

이를 통해 송수신 중 오류가 발생하면, 처음부터가 아닌 동기화 이후부터 다시 재전송한다.
<br>

<strong>세션 계층 역할 요약</strong>

- 사용자 위주의 논리적인 연결 서비스 제공
- 전송모드 설정 (반이중, 전이중, 단방향, 직렬, 동기, 비동기)
- 대화와 동기를 위한 데이터 교환 관리
- 토큰 (Token : 특정 서비스 요구 권리)
</p>
<br>

### 6. 표현 (Presentation)
<p>
<strong>JPEG, MPEG 등</strong>
<br>
Presentation 계층은 7계층(Application Layer)에서 넘겨받은 데이터를 5계층(Session Layer)이 다룰 수 있는 데이터로 바꾸고, 반대로 Session Layer에서 넘겨받은 데이터를 Application Layer가 이해할 수 있는 형태로 바꾸고 전달한다.
<br>
또 그러한 데이터를 안전하게 사용하기 위해 암호화/복호화도 진행한다.
<br>

<img src="https://user-images.githubusercontent.com/81568304/173193631-6593ce4f-8b2c-470b-a245-475c14d79765.png">
즉, 사용자들이 상위나 하위계층에서 사용하는 데이터 표현 양식과 무관하게 사용할 수 있도록 해주는 환경을 제공한다.

- 예시 : EBCDIC로 인코딩된 문서 파일을 ASCII로 인코딩된 파일로 바꿔주기, 해당 데이터가 text인지 그림인지 GIF인지 JPG인지 구분해주기
</p>
<br>



