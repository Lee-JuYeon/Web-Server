# Web-Server
## OS
  Ubuntu 20.04.3
  1. 우분투 데스크탑 
  > https://ubuntu.com/download/desktop
  > 'Ubuntu 20.04.3 LTS' 다운로드
  
  2. 우분투 서버 
  > https://ubuntu.com/download/server 
  > 'Option 2: Manual server installation'을 클릭해서 'Download ubuntu server 20.04.3 LTS' 다운 
  
  3. PuTTY 
  > https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html
  > 본인은 'putty.exe' 64-bit x86을 받았다
  
## Ubuntu Server
  1. date를 입력하면 UTC기준으로 나오는데, 한국 기준으로 변경해보자
  > sudo timedatectl set-timezone "Asia/Seoul"
  2. 순정상태 백업
  > VirtualBox에서 파일 -> 이미지 내보내기.
  >  -> 여기서 이미지라는게 우분투 서버 그 자체인 것 같다.
  >  하여 '이미지 내보내기'는 현재 우분투 서버 현재 상태를 그래도 복사하여 저장하는 것 같음.
  3. 패키지 확인
  > dpkg -l | grep  ssh  (-1은 숫자 1이 아니라 소문자 L이며, |은 엔터 위 '원화' 또는 '\'의 shift버전)
  4. ssh접속
  > 위 과정 중 로그에서 'openssh-server'이 찍히지 않는다면, 아래와 같은 커맨드를 입력해서 설치한다.
  > sudo apt -y install openssh-server
  > 이후에 ssh접속 커맨드를 입력해준다
  > sudo systemctl start ssh
  5. 한글팩 추가
  > sudo apt -y install language-pack-ko
  6. 한글팩 활성화
  > sudo update-locale LANG=ko_KR.UTF-8 LC_MESSAGES=POSIX
  > 세션 변경(로그인 다시 하면 됨)
  7. root권한으로 변경
  > sudo -i // root권한에서 빠져나오려면 'logout' 커맨드를 입력하면 된다
  8. root 비밀번호 설정
  > sudo passwd root
  9. sudo를 매번 쓰기 귀찮을 때?
  > su - (입력후에 다른 sudo 커맨드 사용 할 때에,  sudo를 붙히지 않아도 된다)
  
  
  
  
## Apache2 vs Nginx
  ### 무엇이 다른가?
  __◼ Apache__
  1. Client 접속마다 Process 혹은 Thread를 생성하는 구조.
    = Client 갯수 ⬆ -> CPU 사용량 ⬆, Memory 사용량 ⬆ 
  2. 프로세스가 'Blocking' 될 때 이전에 요청된 프로세스가 완료될 때까지 대기상태.
    = 해결방안으로는 Keep Alive를 활성화하면 되지만, KeepAlive는 대량 접속 시 효율이 급격히 떨어짐.

  ⚠ 'Keep Alive'이 뭐야?
  > HTTP 프로토콜의 특성상 한 번 통신이 이루어지면 접속을 끊어 버리지만, 
  > KeepAlive On상태에서는 KeepAliveTimeOut시간 동안 접속을 끊지 않고 다음 접속을 기다린다.
  > 즉, 한 번 연결된 Client와 통신을 유지하고 있기 때문에, 
  > 다음 통신 시에 Connection을 생성하고 끊는 작업이 필요 없게 된다.

  __◼ Nginx__
  1. 'Event-Driven'방식. 
  2. 한 개 또는 고정된 Process만 생성, 그 내부에서 비동기로 효율적인 방식으로 Task를 처리.
    = Client 갯수 ⬆ -> 추가적인 생성비용(CPU, Memory) 🙅🏻‍♂️
  3. 'Context Switching' 비용이 적다.
  4. 비동기 이벤트 기반으로 요청.
  5. Apache와 달리 CPU와 관계없이 I/O들을 전부 Event  Listener로 미루기 때문에 흐름 끊김이 없다.
  6. 보안에 대해 우수하다.
    = 앞 단의 Nginx로 reverse proxy로 사용하고 뒷 단에는 WAS를 설치하여,
    외부에 노출되는 인터페이스에 대해 Nginx WAS 부분만 노출가능하다. 
    IF) 사용자가 직접적인 WebServer로의 접근을 한다라고 하면 문제가 발생할 수 있기 때문에
        직접적이지 않고 한 단계를 더 거침으로써 보안적인 부분을 처리할 수 있다.
  7. Backend-Service 장애 대응처리.
    = Backend-service에 대해 max fails, fail timeout시 백업 서버로 진입할 수 잇도록 처리가능.
  8. Module로 구성
  9. Single-Threaded(Worker Process)
  

  ⚠'Context Switching'이 뭐야?
  > Context는 Thread가 Task(작업)을 진행하는 동안 작업정를 보관하는 것을 말한다. 
  > OS는 A작업을 진행할 때 A Thread의 Context를 읽어오며,
  >  B Thread로 전활 할 때에는 A Thread의 Context를 저장하고 B Thread의 Context를 읽어오는 반복작업을 한다.
  >  즉, Thread 갯수가 많아질 수록 Context Switching 작업은 더 빈번하게 일어나며 성능 저하를 야기한다.
  
  __Apacche & Nginx 장점__

  ||Apache2|Nginx|
  |:---:|:---:|:---:|
  |성능||👍|
  |보안||👍|
  |모듈|👍||
  |안정성|👍||
  |확장성|👍||
  |호환성|👍||
  |유지보수|👍||

  __Nginx 사용__
  - 로드 밸런서
    - 효율성
    - 안정성(누군가 죽어도 다른애가 있다)
  - Reverse Proxy
    - 보안
    - 유연성 (외부에서 직접적으로 모르기에)
    - 레이턴시 감소
      - 압축
      - SSL (암호화)
      - 캐시
  

참고한 링크
1. https://victorydntmd.tistory.com/231 
2. https://velog.io/@ksso730/Nginx-Apache-%EB%B9%84%EA%B5%90
3. https://www.youtube.com/watch?v=QeBqwwbsBbM
