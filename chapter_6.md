# HTTP 상태 코드
상태 코드: 클라이언트가 보낸 요청의 처리 상태를 응답에서 알려주는 기능을 한다
![image](https://github.com/SAMEZ-0129/HTTP_Web_Basic_Study/assets/81644075/4368b4c0-830f-4cd3-b711-ac3e368ab30a)

만약 모르는 상태 코드가 나타나면?
클라이언트가 인식할 수 없는 상태코드를 서버가 반환하면: 클라이언트는 상위 상태코드로 해석 처리(299? -> 2XX라는 것으로 이해하고 처리)

## 2XX 성공
200 OK: 이상 없음, 정상 상태
201 Created: 요청 성공해서 새로운 리소스가 생성됨(POST같은 걸로 자원 생성 시), 생성된 리소스는 응답 헤더에 Location 헤더 필드로 식별=URI 전달
202 Accepted: 요청이 접수되었으나 처리가 완료되지 않은 경우, 배치 처리 같은 곳에서 주로 사용
204 No content: 서버가 요청을 성공적으로 수행했지만, 응답 페이로드 본문에 보낼 데이터가 없음(예로 들어 웹 문서 편집기에서 save할 경우, 저장 버튼 결과로 아무 내용이 없어도 됨)
그 외에도 많은 상태 코드가 존재함

## 3XX 리다이렉션
요청을 완료하기 위해 유저 에이전트의 추가 조치가 필요할 때
리다이렉션이란 > 윕 브라우저는 3xx 응답의 결과에 location 헤더가 있으면, location 위치로 자동 이동하는 것 
![image](https://github.com/SAMEZ-0129/HTTP_Web_Basic_Study/assets/81644075/21f4c87b-30b9-4fe6-9adb-f992754956ad)
![image](https://github.com/SAMEZ-0129/HTTP_Web_Basic_Study/assets/81644075/e5f01e31-07db-4175-b7ff-34fd518ee966)
영구 리다이렉션
301, 308
리소스의 URI가 영구적으로 이동
원래의 URL 사용X, 검색 엔진 등에서도 변경 인지
![image](https://github.com/SAMEZ-0129/HTTP_Web_Basic_Study/assets/81644075/f0ede02d-6f5b-4242-aea8-fbf38605f1c9)
![image](https://github.com/SAMEZ-0129/HTTP_Web_Basic_Study/assets/81644075/6bb3a461-0ce5-4cf4-994f-8295bd0bbed7)

300 Multiple Choices
301 Moved Permanently:
