# HTTP

## 모든 것이 HTTP
Hyper Text Transfer Protocol
하이퍼 텍스트 문서를 전달하는 방식(프로토콜)

현재는 인터넷에 거의 모든 데이터 형식을 전송하는데 사용한다
서버간에 데이터를 주고 받을 때도 대부분 HTTP를 사용한다

현재 가장 많이 사용하는 버전은 HTTP/1.1 -> 이후 버전 2나 3은 주로 성능개선임
동일한 1.1버전이라도 세부 업데이트가 년도별로 이루어지고 있음, 최근 버전은 RFC7230~7235(2014)

### 기반 프로토콜
- TCP 기반으로 작동: HTTP/1.1, HTTP/2
- UDP 기반으로 개발됨: HTTP/3
- 현재 1.1이 주로 많이 사용됨, 2와 3도 점차 증가하는 추세

사이트가 어떤 프로토콜을 사용하고 있는지 확인할 수 있는 방법
naver.com 접속 -> f12를 통해 브라우저 개발자 도구 진입 -> 네트워크 탭에서 페이지 로드 시 Protocol 부분에서 h2는 HTTP/2, h3는 HTTP/3, http/1.1과 같이
문서가 어떤 프로토콜을 활용하고 있는지 확인할 수 있음

### HTTP 특징
- 클라이언트 서버 구조
  - Request Response 구조
  - 클라이언트가 서버에 요청을 보내고, 응답을 대기
  - 서버가 요청에 대한 결과를 만들어서 응답
  - 크라와 서버를 분리해서 작업이 이루어진 다는 점이 중요 -> 클라이언트와 서버가 독립적으로 성장할 수 있다

- 무상태 프로토콜(스테이트리스), 비연결성
  - 서버가 클라이언트의 상태를 보존하지 않음:![image](https://github.com/SAMEZ-0129/HTTP_Web_Basic_Study/assets/81644075/df1cc9aa-015e-41a8-996c-4ddebf9bd5ed),![image](https://github.com/SAMEZ-0129/HTTP_Web_Basic_Study/assets/81644075/0d1cef7c-528c-4957-a63b-a2ab6239a5af)
  - 상태 유지(stateful)을 사용할 시 중간에 서버가 바뀌면 장애가 발생할 수 있는 것임 ![image](https://github.com/SAMEZ-0129/HTTP_Web_Basic_Study/assets/81644075/929cb36a-8321-4a9f-9cfe-1213601b7e3c)
  - 무상태 (stateless)에서는 중간에 서버가 바뀌어도 요청을 수행할 수 있는 것
  - ![image](https://github.com/SAMEZ-0129/HTTP_Web_Basic_Study/assets/81644075/8796b19e-e3ce-4948-8576-ccd3d3df6e30)
  - 상태 유지를 사용하는 경우 통신하고 있는 서버에 장애가 발생하면 해당 클라이언트의 상태가 사라지기에 다시 처음부터 통신을 해야 하는 것
  - 무상태를 사용할 경우 통신하고 있는 서버에 장애가 발생해도 메시지에 상태에 대한 정보가 모두 담겨있기 때문에 다른 서버에서 통신을 이어가도 진행이 가능한 것
  - 무상태는 스케일 아웃(서버 수평 확장)하는데 매우 유리하다
  - 다만 한계도 존재함![image](https://github.com/SAMEZ-0129/HTTP_Web_Basic_Study/assets/81644075/b4583707-4ae6-4af6-9e95-16ae5f256591)
  - 추가로 단점으로 꼽자면, 데이터를 너무 많이 보냄
  - 비연결성을 활용하면 서버가 이용하는 자원을 최소한으로 줄일 수 있다(요청에 응답을 할 경우에만 서버 자원을 활용)
  - ![image](https://github.com/SAMEZ-0129/HTTP_Web_Basic_Study/assets/81644075/4312438e-01f7-447c-8aa3-742f8d624526)
  - 단점: TCP/IP 연결을 새로 맺어야 함 - 3 방향 핸드셰이크 시간이 추가로 필요, 웹 브라우저로 사이트를 요청하면 HTML 뿐만 아니라 자바스크립트, css, 이미지 등 수 많은 자원이 함께 다운로드 됨, 현재는 HTTP 지속 연결(persistent connections)로 해당 문제를 해결함
  - HTTP 지속 연결을 통해 클라이언트-서버 연결/종료를 통신할 때 마다 진행하지 않고 서버 아키텍처 설계 방법에 따라 한번의 연결로 요청과 응답을 전부 주고 받을 수 있게 할 수 있다
  - ![image](https://github.com/SAMEZ-0129/HTTP_Web_Basic_Study/assets/81644075/074c82a2-d891-4a73-8e2e-2c199332e9eb)

- HTTP 메시지를 통해 통신함
  - ![image](https://github.com/SAMEZ-0129/HTTP_Web_Basic_Study/assets/81644075/02a8d5e7-dcea-47cc-b64b-09410e2259eb)
  - 요청과 응답에 따라 구조에 차이가 있음
  - 요청 메시지의 경우도 전송할 데이터가 있는 경우 본문을 가질 수 있음
  - start-line: request-line / status-line
    - reuqst-line: method SP(공백) request-target SP HTTP-version CRLF(엔터)
    - HTTP 메서드의 종류: ![image](https://github.com/SAMEZ-0129/HTTP_Web_Basic_Study/assets/81644075/ea18a71e-3265-4d77-8874-b28a0a50f82f)
    - 요청 대상: 절대 경로(absolute-path)로 시작한다
    - status-line: HTTP-version SP status-code SP reason-phrase CRLF
    - HTTP 상태 코드: 요청 성공, 실패를 나타낸다 (200:성공, 400: 클라이언트 오류, 500:서버 내부 오류)
    - 이유 문구: 사람이 이해할 수 있는 짧은 상태 코드의 설명
  - header-field: field-name ":" OWS field-value OWS (OWS:띄어쓰기 허용)
    - 헤더 필드의 용도: HTTP 전송에 필요한 모든 부가정보(바디의 내용, 크기, 압축, 인증, 요청 클라이언트 정보, 서버 애플리케이션 정보...등), 표준 헤더 정보가 엄청 많음
  - 메시지 바디: 실제로 전송할 데이터가 포함되는 공간

- 단순함, 확장 가능: 이 기술이 크게 성공할 수 있었던 이유임, 쉽게 이해할 수 있으면서도 확장성이 뛰어나다


