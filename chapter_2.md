# URI와 웹 브라우저 요청 흐름
## URI(Uniform Resource Identifier)
URI는 로케이터(locator), 이름(name) 또는 둘다 추가로 분류될 수 있다.
*표준 스펙: https://www.ietf.org/rfc/rfc3986.txt - 1.1.3. URI, URL, and URN

![image](https://github.com/SAMEZ-0129/HTTP_Web_Basic_Study/assets/81644075/a296b7e3-2bd1-496b-b4fc-4e5c5570c635)

URI는 해당 리소스의 위치 = 그래서 로케이터라고 부름.
URN은 해당 리소스의 이름(해당 리소스 그 자체) = 그래서 이름임.

![image](https://github.com/SAMEZ-0129/HTTP_Web_Basic_Study/assets/81644075/a5c31782-87c0-424a-8448-5a1a7667f5fb)
URN은 거의 사용하지 않음.

URI의 단어 뜻
- Uniform: 리소스 식별하는 통일된 방식
- Resource: 자원, URI로 식별할 수 있는 모든 것(제한 없음)
- Identifier: 다른 항목과 구분하는데 필요한 정보

URL - Locater: 리소스가 있는 위치를 지정(표현?)
URN - Name: 리소스에 이름을 부여
위치는 변할 수 있지만, 이름은 변하지 않는다. 
URN 이름만으로 실제 리소스를 찾을 수 있는 방법이 보편화 되지 않아서 해당 기술이 거의 사라진 상태
본 강의에서는 URI를 URL과 같은 의미로 이야기 할 예정

URL 분석
https://www.google.com/search?q=hello&hl=ko
-> 해당 사이트로 이동 시 구글에서 hello의 검색 결과가 나옴

URL 문법 구조
scheme://[userinfo@]host[:port][/path][?query][#fragment]

프로토콜(https):
  스키마는 주로 프로토콜(https)을 사용한다.
  프로토콜은: 어떤 방식으로 자원에 접근할 것인가를 정하는 약속과 규칙
    ex) http, https, ftp 등등
  http는 80포트, https는 443 포트를 주로 사용하고, 포트는 생략이 가능하다
  https는 http에 보안이 추가된 기능

userinfo@:
   거의 사용하지 않음
   URL에 사용자 정보를 포함해서 인증하는 용도

호스트명(www.google.com):
  호스트명
  도메인명 또는 IP주소를 직접 사용할 수 있음

포트 번호(443):
  해당 경로 접속을 위한 포트
  일반적으로 생략, 생략 시 http는 80, https는 443으로 지정

경로/path(/search):
  해당 리소스가 있는 경로
  계층적 구조로 이루어져 있음
  예시) /home/file1.jpg, /members, /members/100, /items/iphone12

쿼리 파라미터(q=hello&hl=ko):
  key=value 형태로 데이터가 들어감
  ?로 시작, &로 파라미터들을 추가 가능
  query parameter, query string 등으로 불림, 웹서버에 제공하는 파라미터, 문자 형태로 넘어감

fragment
  html 내부 북마크 등에서 사용하는 용도
 서버에 전송하는 정보 X
 크게 중요한 내용 아님

## 웹 브라우저 요청 흐름
![image](https://github.com/SAMEZ-0129/HTTP_Web_Basic_Study/assets/81644075/f6b9cb20-f6c7-4f47-95ff-f2fd9d41b110)

해당 URL로 이동 시 DNS서버(구글 서버)를 조회함 -> 포트 정보 연결 -> http 요청 메시지를 생성
![image](https://github.com/SAMEZ-0129/HTTP_Web_Basic_Study/assets/81644075/e3edf254-51ab-40cb-ae4e-1548986b1ed6)
요청 메시지는 위 이미지처럼 이루어진다

![image](https://github.com/SAMEZ-0129/HTTP_Web_Basic_Study/assets/81644075/8419bfc9-a88e-4d6d-b7e4-e90400159f98)
1. 웹 브라우저에서 HTTP 메시지 생성
2. 소켓 라이브러리를 통해 해당 메시지의 정보를 바탕으로 전달(IP, PORT 정보), 3 way handshake 방식 -> 데이터 전달
3. IP, PORT정보가 담긴 패킷을 메시지에 씌우고 인터넷으로 전달됨

![image](https://github.com/SAMEZ-0129/HTTP_Web_Basic_Study/assets/81644075/b84ee186-26db-438a-b1cb-4398b2229669)
간략하게 표현한 인터넷으로 전송되는 메시지 이미지

서버에서 해당 메시지를 받고 응답 메시지를 생성한다
서버에서 전달하는 응답 메시지에는 전송 방식, 상태 코드...등 기존에 서버로 보낼 때 처럼 구조가 씌워져 있고 내부 데이터에 요청한 페지이의 데이터가 들어있는 것
클라이언트에서 해당 응답을 전달 받으면 데이터를 웹 브라우저가 랜더링 해서 화면에 보여지는 방식


  


