# HTTP 메서드
HTTP API를 만들어보자!
예시)![image](https://github.com/SAMEZ-0129/HTTP_Web_Basic_Study/assets/81644075/f9b39b0b-cbd8-4b55-b743-cabe0b2e368e)
API URL 설계
- 회원 목록 조회 /read-member-list
- 회원 조회 /read-member-by-id
- 회원 등록 /create-member
- 회원 수정 /upadte-member
- 회원 삭제 /delete-member
-> 이건 과연 좋은 URI 설계일까??
가장 중요한건 **리소스 식별**

리소스의 의미는 뭘까?: 회원을 등록하고 수정하고 조회하는게 리소스가 아니다! 회원이라는 개념 자체가 리소스다.
리소스를 어떻게 식별하는게 좋을까?: 회원을 등록하고 수정하고 조회하는 것을 모두 배제, 회원이라는 리소스만 식별하면 된다 -> 회원 리소스를 URI에 매핑한다
- 회원 목록 조회 /members
- 회원 조회 /members/{id} 
- 회원 등록 /members/{id}
- 회원 수정 /members/{id}
- 회원 삭제 /members/{id}
어떻게 구분하지??

URI는 리소스만 식별한다, 리소스와 해당 리소스를 대상으로 하는 **행위**를 분리한다: 리소스=회원, 행위=조회,등록,삭제,변경
해당 행위를 분리하는 방식을 HTTP 메소드가 대신 해준다, 우리는 리소스만 식별할 수 있으면 된다

## GET,POST
주로 가장 많이 사용하는 메서드: ![image](https://github.com/SAMEZ-0129/HTTP_Web_Basic_Study/assets/81644075/49a2be9d-61b4-4ec1-b360-cb2463067697)

### GET 메서드
- 리소스 조회한다
- 서버에 전달하고 싶은 데이터는 query(쿼리 파라미터, 쿼리 스트링)를 통해서 전달한다
- 메시지 바디를 사용해서 데이터를 전달할 수 있지만, 최근 스펙에서는 지원하지만 아직 많지 않아서 권장하지 않음
![image](https://github.com/SAMEZ-0129/HTTP_Web_Basic_Study/assets/81644075/2da351e2-830e-4222-95fb-bc6256afc700)
![image](https://github.com/SAMEZ-0129/HTTP_Web_Basic_Study/assets/81644075/4b114901-a41c-4d22-8e84-d52b86cddd4d)
요청 형식을 준수하면서 서버로 GET(요청)을 날린다
![image](https://github.com/SAMEZ-0129/HTTP_Web_Basic_Study/assets/81644075/8b72f7c2-5d8b-489a-91eb-1882eb8e0fcf)
서버에서 응답을 받으면 클라이언트로 전송될 데이터의 형식에 맞춰 데이터를 전송한다

### POST 메서드
- 요청 데이터를 처리함
- 메시지 바디를 통해 서버로 요청 데이터 전달(클라이언트 -> 서버로 데이터 전달해서 처리를 해달라고 함)
- 서버는 요청 데이터를 처리함, 메시지 바디로 통해 들어온 데이터를 처리하는 모든 기능을 수행
- 전달된 데이터로 주로 신규 리소스 등록, 프로세스 처리에 사용함
![image](https://github.com/SAMEZ-0129/HTTP_Web_Basic_Study/assets/81644075/6c2e46fd-3e14-4291-90c9-a2e0b67cd2c8)
전달할 데이터를 어떻게 사용할 것인지 미리 서버와 약속을 한다(새로 저장할거야, 프로세스를 바꿀꺼야...등)
![image](https://github.com/SAMEZ-0129/HTTP_Web_Basic_Study/assets/81644075/e7197140-f7da-4fcf-a94d-b436d5823677)
이후 POST를 통해 서버로 데이터를 전달한다
![image](https://github.com/SAMEZ-0129/HTTP_Web_Basic_Study/assets/81644075/e91f513d-8bfa-4f8a-afd4-c3cea39700ab)
서버에서 응답을 처리한 후 클라이언트로 처리한 요청을 알려준다

스펙: POST 메서드는 대상 리소스가 리소스의 고유 한 의미 체계에 따라 요청에 포함 된 표현을 처리하도록 요청합니다. (구글 번역)
예를 들어 POST는 다음과 같은 기능에 사용됩니다.
HTML 양식에 입력 된 필드와 같은 데이터 블록을 데이터 처리 프로세스에 제공
예) HTML FORM에 입력한 정보로 회원 가입, 주문 등에서 사용
게시판, 뉴스 그룹, 메일링 리스트, 블로그 또는 유사한 기사 그룹에 메시지 게시
예) 게시판 글쓰기, 댓글 달기
서버가 아직 식별하지 않은 새 리소스 생성
예) 신규 주문 생성
기존 자원에 데이터 추가
예) 한 문서 끝에 내용 추가하기
정리: 이 리소스 URI에 POST 요청이 오면 요청 데이터를 어떻게 처리할지 리소스마다 따로 정해야 함 -> 정해진 것이 없음

요약:
1. 새 리소스를 생성(등록)하는데 사용한다
2. 요청 데이터 처리: 단순히 데이터를 생성하거나, 변경하는 것을 넘어 프로세스를 처리해야 하는 경우에서도 사용한다
3. 다른 메서드로 처리하기 애매한 경우: 예로 들어 JSON으로 조회 데이터를 넘거야 하는데, GET 메서드를 사용하기 어려운 경우
(리소스만으로 URI를 설계하기 어려울 경우 동사(행위)로 URI를 설계할 수 있음 -> 컨트롤 URI 라고 부름)

### PUT,PATCH,DELETE
PUT
- 리소스를 대체한다: 리소스가 있으면 대체, 리소스가 없으면 생성, 쉽게 덮어쓰기의 개념
- 클라이언트가 리소스를 식별한다!: 클라이언트가 리소스 위치를 알고 URI 지정(POST 와 차이점)
- 해당 리소스를 **완전히 대체한다** <- 중요!
예로들어, 'username':'young', 'age':'20'인 리소스가 있는데 age만 50으로 변경하고 싶어서 PUT으로 요청할 경우
![image](https://github.com/SAMEZ-0129/HTTP_Web_Basic_Study/assets/81644075/2905fdbc-b76a-477f-906d-5cc8868a1e6d)
해당 /members/100 위치에 데이터는 members 필드가 **삭제**되고 age:50 만 남게 되는 것!

PUT으로 리소스를 수정하기 너무 곤란한데? -> PUT은 리소스를 수정하기 위해 사용하지 않는다! 수정을 위해 사용하는 것은 PATCH

PATCH
위 예시와 동일하게 age 만 변경하고 싶어해서 PATH 메서드로 username 필드 없이 보낼 경우 PATH는 해당 필드만 변경해준다
PATCH 메서드를 지원하지 않는 서버도 일부 있음 = POST로 사용할 수 대체 가능하긴함

DELETE
리소스를 제거할 때 사용한다 

### HTTP 메서드의 속성
안전(safe)
호출해도 리소스를 변경하지 않는다 = 안전하다
리소스를 변경하지 않지만, 계속 호출해서 로그가 쌓여 장애가 발생하면요?? -> 리소스에 해당 리소스 변경만 신경쓰기에 그런 부분까지 고려하지 않는다

멱등(Idempotent)
쉽게 설명: 한 번 호출하든, 두 번 호출하든 100번 호출하든 결과가 똑같아야 한다
멱등 메서드: GET, PUT(결과를 대체함, 여러번 요청해도 최종 결과가 동일하기에 멱등함), DELETE
멱등은 어디 써먹음? -> 자동 복구 매커니즘, 서버가 TIMEOUT 등으로 정상 응답을 못주었을 때, 클라이언트가 같은 요청을 다시 해도 되는가?의 판단 근거 **(같은 요청을 두번 해도 결과가 동일하다는 것을 알기 때문)**
재요청 중간에 다른 곳에서 리소스를 변경하면? -> 멱등은 외부 요인으로 중간에 리소스가 변경되는 것 까지 고려하지는 않음

캐시가능(cacheable)
응답 결과 리소스를 캐시해서 사용해도 되는가?(클라이언트에 저장해서 사용해도 되나?)
GET,HEAD,POST,PATCH = 캐시가능 
실제로는 GET,HEAD 정도만 캐시로 사용함, POST,PATCH는 본문 내용까지 캐시 키로 고려해야 하가에 구현이 쉽지 않음