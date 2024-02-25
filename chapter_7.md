# HTTP 헤더 
## HTTP 헤더 개요
헤더는 field-name":" OWS field-value OWS 형식으로 이루어져 있음
헤더의 용도
- HTTP 전송에 필요한 모든 부가정보: 메시지 바디의 내용, 메시지 바디 크기, 압축, 인증, 요청 클라이언트...등
- 표준 헤더는 엄청 많기에 필요 시 직접 찾아보길
- 필요 시 임시의 헤더 추가 가능하다 (helloworld:hihi)

과거 스펙:![image](https://github.com/SAMEZ-0129/HTTP_Web_Basic_Study/assets/81644075/48321461-4edd-412f-8c6e-c89356f53d80)
이후 스펙의 변경: 엔티티 -> 표현(representation)

최신 스펙:![image](https://github.com/SAMEZ-0129/HTTP_Web_Basic_Study/assets/81644075/40c9be43-b2a2-410f-812c-96408ba44016)
표현이라는 말로 변경된 이유: 실제 전달할 데이터들이 어떻게 **표현**될 것인지에 대한 정의

## 표현(Representation)
Content-Type: 표현 데이터의 형식-미디어 타입, 문자 인코딩, 전송될 데이터의 형식
Content-Encoding: 표현 데이터의 압축 방식-표현 데이터를 압축하기 위해 주로 사용, 데이터를 전달하는 곳에서 압축 후 인코딩 헤더 추가(받는 쪽에서 인코딩 된 데이터를 풀 수 있기 위해)
Content-Language: 표현 데이터의 자연 언어(ko,en,jp...등), 예시로 애플 홈페이지를 갔을 때 영어 사이트로 들어갔는데 한국어로 바꿀지 물어보는 팝업 나타남
Content-Length: 표현 데이터의 길이(바이트 단위) *Transfer-Encoding(전송 코딩)을 사용하면 Content-Length를 사용하면 안됨
표현 헤더는 전송, 응답 둘다 사용한다

## 협상(Content Negotiation): 클라이언트가 선호하는 표현 요청(클라이언트가 원하는 형식으로 요청하는 것)
- Accept: 클라이언트가 선호하는 미디어 타입 전달
- Accept-Charset: 클라이언트가 선호하는 문자 인코딩
- Accept-Encoding: 클라이언트가 선호하는 압축 인코딩
- Accept-Language: 클라이언트가 선호하는 자연 언어
예로 들어 클라이언트(한국)-서버(미국 웹사이트)끼리 통신을 한다고 가정해보자
서버는 한국어를 지원하지만 기본 디폴트 값은 영어로 제공하고 있다
클라이언트가 요청을 했을 때 Accept-Language가 적용되어 있지 않다면 그냥 기본 값인 영어로 응답이 온다
Accept-Language가 적용되어 있다면, 클라이언트가 요청을 했을 때(GET /event ... Accept-Language:ko) 서버는 해당 언어(를 지원한다면)로 응답을 보내준다

협상과 우선순위 1(Quality Values)
![image](https://github.com/SAMEZ-0129/HTTP_Web_Basic_Study/assets/81644075/183baef4-f4b3-48d8-bc62-94a001a0c71b)
Quality Values 값을 통해 선호 값에 우선순위를 지정할 수 있다
0~1 사이의 값을 가지며, 클 수록 높은 우선순위를 가진다(예시: ko=0.9,en=0.6,jp=0.5 > 한국어로 보여주고 지원하지 않으면 영어, 지원하지 않으면 일본어로)

협상과 우선순위 2
구체적인 것이 우선한다
![image](https://github.com/SAMEZ-0129/HTTP_Web_Basic_Study/assets/81644075/2da65f34-d681-42a9-ad74-35994daeba9d)
위 예시의 우선순위는 다음과 같다 ![image](https://github.com/SAMEZ-0129/HTTP_Web_Basic_Study/assets/81644075/3a833345-c028-4ca2-9e1a-684bc6d51ea8)

협상과 우선순위 3
구체적인 것을 기준으로 미디어 타입을 맞춘다
![image](https://github.com/SAMEZ-0129/HTTP_Web_Basic_Study/assets/81644075/02f1e79c-c95d-44c6-a4c0-61da47138f6a)

## 전송 방식
단순 전송: content-length 구체적인 요청 값을 알고 있을 때 요청을 전송하면 해당 응답을 받음

압축 전송: content-encoding을 통해 압축된 요청을 전송한다

분할 전송: transfer-encoding을 통해 너무 긴 요청 값을 분할해서 전송함. 요청이 너무 길어서 처리하는데 시간이 오래 걸릴 경우 분할해서 전송하면 미리 받아보면서 처리가 가능(content-length를 넣으면 안됨!)

범위 전송: Range, Content-length 예로들어 중간에 응답받다가 연결이 끊킬 경우 처음부터 다시 받기 아까우니 범위 전송을 통해 특정 부분부터 데이터를 요청할 수 있음

