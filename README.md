## Web-Server
__클라이언트 <-> 서버 <-> 백앤드 <-> DB__
  |클라이언트|서버|백앤드|DB|
  |:---:|:---:|:---:|:---:|
  |Android|Apache2|PHP|MariaDB|
  |iOS|Nginx|NodeJS|MongoDB|
  |Web|Tomcat|Django||
  |Game|Express|ASP||
  |||Spring||
  |||GSP||
  
__프로토콜 정리__

  |Protocol|Port|유형|접근방식|
  |:---:|:---:|:---:|:---:|
  |HTTP|80|Client||
  |HTTPS|443|Client||
  |SSH|22|관리자|터미널|
  |SFTP|22|관리자|파일전송|
  |FTP|21|관리자|파일전송|
  |MySQL|3306|관리자|DB|
  
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
  1. root로 접근   
    -$ sudo su   
  2. apt 업데이트   
    -$ apt-get update   
  3. MariaDB 설치   
    '''   
    $ apt install mariadb-server   
    $ Y    
    $ systemctl status mariadb (mariadb 상태 확인)    
    $ mysql_secure_installation (mysql이 맞다. mariadb 아니라고 당황하지 마시길)    
    $ root 계정 비밀번호 물음 시, 계정 비밀번호 입력    
    $ Y ( set root password? )    
    $ 비밀번호, 비밀번호 확인 입력   
    $ Y ( remove anonymous users? )   
    $ Y ( disallow root login remotely? )   
    $ Y ( remove test dtabase and access to it? )   
    $ Y ( Reload privilege tables now? )   
    $ sudo mysql -u root -p ( mariadb 비밀번호 설정. 그리고 입력된 mysql이 맞다. mariadb 아니라고 당황하지 마시길 )   
    $ exit ( MariaDB 모니터에서 빠져나오는 커맨드 )   
    '''
  4. Nginx 설치   
    -$ apt-get install nginx      
  5. Nginx 상태 확인   
    -$ systemctl status nginx (Nginx의 상태를 보여주는 커맨드)      
    -$ q (Nginx 상태창에서 나오게하는 커맨드)      
    
  6. Nginx를 위해 Port(포트) 열어주기    
    - VirtualBox       
    - 해당 우분투 서버 '설정(S)'      
    - 고급(D)      
    - 포트 포워딩(P)      
    - '새 포트 포워딩 규칙 추가' 클릭      
    - 호스트 포트 & 게스트 포트 = 80 으로 추가 (80번은 HTTP를 위한것)      
    - '새 포트 포워드 규칙 추가' 클릭    
    - 호스트 포트 & 게스트 포트 = 22 으로 추가 (22번은 SSH를 위한것)    
  7. 웹사이트에 '127.0.0.1' 이동
  8. 웹사이트에 'Welcome to Nginx' 텍스트 출력됨
  9. Node.js설치   
    -$ apt-get install nodejs (nodejs 설치 커맨드)    
    -$ y (설치 승낙)    
    -$ node -v (Node.js 버전확인. 나는 2021-10-17기준으로 v10.19.0이 다운되어있다)    
    -$ curl -sL https://deb.nodesource.com/setup_14.x -o nodesource_14_setup.sh (아마 디렉토리가 없다고 뜰꺼다)    
    -$ bash nodesource 까지만 타이핑 -> Tab키 누르기 -> 엔터  (여기까지가 PPA등록이다)     
    -$ apt-get install nodejs (nodejs 한번 더 설치)     
    -$ node -v (2021-10-17기준으로 v14.18.1 버전 설치 완료 확인)     
    -$ npm -v (2021-10-17기준으로 npm버전은 6.14.15 버전 설치 완료 확인)    
  10. MariaDB 외부접속    
    $ mariadb -u root -p
    $ create user '사용자이름'@'%' identified by '비밀번호'; (클라이언트 생성 + 비밀번호 설정, 작은 따욤표(')가 있다)
    $ create database 데이터베이스이름; (DB 새로 하나 생성, 작은 따옴표(')가 없다)
    $ grant all privileges on 데이터베이스이름.* to '사용자이름'@'%';
    $ flush privileges; (변경사항 저장)
    $ exit (mariadb에서 로그아웃)
    $ vi /etc/mysql/mariadb.conf.d/50-server.cnf
    $ 'bind-address - 127.0.0.1' 부분에 커서를 옮기고 'i'를 누른다
    $ # (i를 누르면 insert모드로 변환되는데 맨 앞에 #을 붙히면 해당 라인은 주석처리가 된다)
    $ ESC
    $ :wq! (vi 편집기에서 탈출)
    $ service mariadb restart (mariadb 재시작)
    



  9. date를 입력하면 UTC기준으로 나오는데, 한국 기준으로 변경해보자
  > sudo timedatectl set-timezone "Asia/Seoul"
  10. 순정상태 백업
  > VirtualBox에서 파일 -> 이미지 내보내기.
  >  -> 여기서 이미지라는게 우분투 서버 그 자체인 것 같다.
  >  하여 '이미지 내보내기'는 현재 우분투 서버 현재 상태를 그래도 복사하여 저장하는 것 같음.
  11. 패키지 확인
  > dpkg -l | grep  ssh  (-1은 숫자 1이 아니라 소문자 L이며, |은 엔터 위 '원화' 또는 '\'의 shift버전)
  12. ssh접속
  > 위 과정 중 로그에서 'openssh-server'이 찍히지 않는다면, 아래와 같은 커맨드를 입력해서 설치한다.
  > sudo apt -y install openssh-server
  > 이후에 ssh접속 커맨드를 입력해준다
  > sudo systemctl start ssh
  13. 한글팩 추가
  > sudo apt -y install language-pack-ko
  14. 한글팩 활성화
  > sudo update-locale LANG=ko_KR.UTF-8 LC_MESSAGES=POSIX
  > 세션 변경(로그인 다시 하면 됨)
  15. root권한으로 변경
  > sudo -i // root권한에서 빠져나오려면 'logout' 커맨드를 입력하면 된다
  16. root 비밀번호 설정
  > sudo passwd root
  17. sudo를 매번 쓰기 귀찮을 때?
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
  __HTTPS__
  - 'Let's Encrypt'에서 3개월짜리 무료 라이센스를 받을 수 있는데, 3개월 후엔 자동으로 인증서 갱신을 통해 평생 무료로 쓰자
  > 'Let's encrypt' url = https://letsencrypt.org/ko/

참고한 링크
1. https://victorydntmd.tistory.com/231 
2. https://velog.io/@ksso730/Nginx-Apache-%EB%B9%84%EA%B5%90
3. https://www.youtube.com/watch?v=QeBqwwbsBbM
