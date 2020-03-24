# ubuntu-18.04 nginx 서버를 이용한 실시간 스트리밍 서버 구축하기.
트위치나 아프리카 같은 실시간 플랫폼에서 재생하는 파일의 핵심은 m3u8의 파일이다.
여러가지 프로토콜로 이 파일을 만들어 낸다. 
나는 이중에서 가장 심플하고 xsplit에서 바로 쉽게 custom으로 제공해주는 rtmp 서버를 구축하고,
이 파일을 hls를 통해서 m3u8 파일로 서버에 뽑아서 클라이언트에서 접근 가능하게 제공을 한 다음,
test.html에서 해당 부분을 video js 라는 라이브러리로 실행을 시켜서 마치 인터넷 방송 플랫폼을 보는 환경을 구축했다.
테스트에 필요한 도구는 
1. xsplit
2. 접속이 가능한 ubuntu 18.04 서버
그 과정들을 나열해보자.

## 1. ubuntu 18 에서의 환경 구축

필요한 nginx 서버를 설치하는데 rtmp가 어느정도 갖춰져있는 모드로 설치한다.
`sudo apt install libnginx-mod-rtmp`

nginx conf에 nginx.conf.example을 추가한다.
외부에 공개해야하는 server는 반드시 http 블록 안에 넣어 주어야 한다.
`sudo vi /etc/nginx/nginx.conf`

## 2. xsplit 브로드캐스팅 custom
https://www.xsplit.com/ko/broadcaster/manual/broadcast/custom-rtmp

이 주소를 보고 참조한다.
stream name은 실제 m3u8파일의 이름이 된다.
그 후 브로드캐스트로 방송을 실행한다.
우측하단에 you are now live streming rtmp.... 메시지가 나오면 정상적으로 송출하고 있다.

## 3. 화면에서 확인 할 html 파일 작성
마지막으로 송출 중인 화면을 볼 html을 작성한다.
일반 적인 비디오 태그는 m3u8파일을 읽어서 ts파일을 실행 시키지 못한다.
마음먹고 구현해도 좋으나 일단 apache license 인 video js 를 사용해서 m3u8을 정상적으로 실행시키자.

우리는 위의 1번을 따라했으면 `location /hls` 이 구문을 따라서 뒤에 path가 달라진다.
'http://도메인주소/{location}/{stream name}.m3u8'의 형식으로 vedio src를 넣어주면된다.






