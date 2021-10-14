# Web-Server
##### OS
__Ubuntu 20.04.3__

##### Apache2 OR Nginx
######  무엇이 다른가?
__Apache__
1. Client 접속마다 Process 혹은 Thread를 생성하는 구조.
  = Client 갯수 ⬆ -> CPU 사용량 ⬆, Memory 사용량 ⬆ 
2. 프로세스가 'Blocking' 될 때 이전에 요청된 프로세스가 완료될 때까지 대기상태.
  = 해결방안으로는 Keep Alive를 활성화하면 되지만, KeepAlive는 대량 접속 시 효율이 급격히 떨어짐

__여기서 잠깐❗ 'Keep Alive'이 뭐야?__
> HTTP 프로토콜의 특성상 한 번 통신이 이루어지면 접속을 끊어 버리지만, 
> KeepAlive On상태에서는 KeepAliveTimeOut시간 동안 접속을 끊지 않고 다음 접속을 기다린다.
> 즉, 한 번 연결된 Client와 통신을 유지하고 있기 때문에, 
> 다음 통신 시에 Connection을 생성하고 끊는 작업이 필요 없게 된다.

__Nginx__
1. 'Event-Driven'방식. 
2. 한 개 또는 고정된 Process만 생성, 그 내부에서 비동기로 효율적인 방식으로 Task를 처리.
  = Client 갯수 ⬆ -> 추가적인 생성비용(CPU, Memory) 🙅🏻‍♂️
3. 'Context Switching' 비용이 적다
4. 비동기 이벤트 기반으로 요청
5. Apache와 달리 CPU와 관계없이 I/O들을 전부 Event  Listener로 미루기 때문에 흐름 끊김이 없다.

__여기서 잠깐❗ 'Context Switching'이 뭐야?__
> Context는 Thread가 Task(작업)을 진행하는 동안 작업정를 보관하는 것을 말한다. 
> OS는 A작업을 진행할 때 A Thread의 Context를 읽어오며,
>  B Thread로 전활 할 때에는 A Thread의 Context를 저장하고 B Thread의 Context를 읽어오는 반복작업을 한다.
>  즉, Thread 갯수가 많아질 수록 Context Switching 작업은 더 빈번하게 일어나며 성능 저하를 야기한다.
__Apacche & Nginx 장점__

||Apache2|Nginx|
|:---:|:---:|:---:|
|성능|⬇|⬆|
|모듈|⬆|⬇|
|안정성|⬆|⬇|
|확장성|⬆|⬇|
|호환성|⬆|⬇|
|유지보수|⬆|⬇|


참고한 링크
1. https://victorydntmd.tistory.com/231 
2. https://velog.io/@ksso730/Nginx-Apache-%EB%B9%84%EA%B5%90
