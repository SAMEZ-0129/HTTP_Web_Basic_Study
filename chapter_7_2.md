# HTTP 헤더 2
## 캐시 기본 동작

캐시가 없을 때
- 데이터가 변경되지 않아도 계속 네트워크를 통해서 데이터를 다운로드 받아야 한다
- 인터넷 네트워크는 매우 느리고 비싸다
- 브라우저 로딩 속도가 느리다
- 사용자 경험의 저하

캐시가 있을 때
- 캐시 덕분에 캐시의 유효기간 동안 네트워크를 사용하지 않아도 데이터를 불러올 수 있다
- 비싼 네트워크 사용량을 줄일 수 있다
- 브라우저 로딩 속도가 빠르다
- 사용자 경험 향상
- 캐시 유효 시간이 초과하면, 서버를 통해 데이터를 다시 조회하고 캐시를 갱신한다 = 이때 네트워크 다운로드 발생
유효기간이 초과했다고 해도 요청하려는 데이터가 기존 만료된 데이터와 동일할 경우 굳이 다시 다운로드 해야 하나? -> 서버에서 검증을 통해 해결 가능

## 검증 헤더와 조건부 요청
기존에 전달했던 데이터와 서버에 요청하는 데이터가 동일하다는 것을 검증한다 -> 검증 헤더
Last-Modified: 2020-11-10 10:00:00 같은 헤더를 통해 마지막 데이터의 수정일 확인
클라이언트에서 요청 시 위 정보를 포함해서 요청 -> 서버에서 전달할려는 데이터의 수정일과 요청하는 데이터의 수정일을 비교해서 만료된 캐시라도 사용 가능하도록 응답
![image](https://github.com/SAMEZ-0129/HTTP_Web_Basic_Study/assets/81644075/ba41a9bf-bf19-470f-b865-3b555afd79c4)
응답을 주긴 하지만 기존에 요청한 데이터를 바디에 담아서 전달할 필요가 없기에(캐시 활용) 전송해야 되는 데이터의 양을 줄일 수 있다

클라이언트에서 기존 캐시가 유효하다는 요청을 위와 같이 받으면 기존 캐시를 갱신한다

해당 검증 헤더와 조건부 요청 헤더(if-modified-since = 이 일자 기준으로 변경됨?)를 같이 보내서 기존 캐시를 재사용할 수 있는지 파악 가능
![image](https://github.com/SAMEZ-0129/HTTP_Web_Basic_Study/assets/81644075/31a71ef5-45f8-446a-b533-51a7afa9d5c0)

검증 헤더
- 캐시 데이터와 서버 데이터가 같은지 검증하는 데이터
- Last-Modified, ETag

조건부 요청 헤더
- 검증 헤더로 조건에 따른 분기
- If-Modified-Since: Last-Modified 사용
- If-None-Match: ETag 사용
- 조건이 만족하면 200 OK
- 조건이 만족하지 않으면 304 Not Modified(304는 리다이렉션 응답인데? -> 캐시로 리다이렉션 하라는 뜻)
![image](https://github.com/SAMEZ-0129/HTTP_Web_Basic_Study/assets/81644075/31f0f03b-eef4-4abd-9a41-160cce22e631)

단점
![image](https://github.com/SAMEZ-0129/HTTP_Web_Basic_Study/assets/81644075/123153d1-6349-4a3e-8535-84f03fe787f3)

이때 사용 가능한 것이 ETag
![image](https://github.com/SAMEZ-0129/HTTP_Web_Basic_Study/assets/81644075/d770392a-d087-4d04-a2e3-58a0391742fa)
동일한 파일은 동일한 해시 값으로 나옴(컨텐츠가 실제로 변경되었는지 확인할 수 있음)
![image](https://github.com/SAMEZ-0129/HTTP_Web_Basic_Study/assets/81644075/0b757f7e-170f-4c99-a665-eccb1f6cb860)
서버가 일종의 블랙박스 역할을 하는 것

## 캐시 제어 해더
Cache-Control: 캐시 지시어
:max-age 캐시 유효 시간, 초 단위
:no-cache 데이터는 캐시해도 되지만, 항상 원(origin) 서버에 검증하고 사용
:no-store 데이터에 민감한 정보가 있으므로 저장하면 안됨(메모리에서 사용하고 최대한 빨리 삭제)

Pragma:no-cahce (HTTP 1.0 하위 호환, 이제는 사용하지 않음)

Expires: 캐시 만료일 지정(하위 호환)
캐시 만료일을 정확한 날짜로 지정
지금은 더 유연(정확히 설정 가능한) cache-control의 max-age 사용함, 같이 사용하면 Expires는 무시함
![image](https://github.com/SAMEZ-0129/HTTP_Web_Basic_Study/assets/81644075/a68adecf-2627-4c6b-9abc-dbf8dda4eadc)

## 프록시 캐시
![image](https://github.com/SAMEZ-0129/HTTP_Web_Basic_Study/assets/81644075/9d051fc4-82b4-4707-97a8-ce6bc63387fb)
![image](https://github.com/SAMEZ-0129/HTTP_Web_Basic_Study/assets/81644075/172da115-23e2-4b1f-8d0b-8d683f37a11e)

cache-control: public
= 응답이 public 캐시에 저장되어 됨
cache-control: private
= 응답이 해당 사용자만을 위한 것임, private 캐시에 저장해야 함(기본값)
cache-control: s-maxage
= 프록시 캐시에만 적용되는 max-age
age:60(HTTP헤더)
origin 서버에서 응답 후 프록시 캐시 내에 머문 시간(초)

## 캐시 무효화
확실한 캐시 무효화 응답 방법: Cache-control: no-cache, no-store, must-revalidate + Pragma: no-cache(과거 호환성 위함)
![image](https://github.com/SAMEZ-0129/HTTP_Web_Basic_Study/assets/81644075/949c648d-ab42-4132-a47d-958cca412a60)

cache-contro:no-cache랑 must-revalidate는 같은 역할 아님?(원서버 검증) 왜 둘다 써야함?
-> no-cache는 일부 서버 프로세스의 경우(항상 그런건 아님) 브라우저에서 프록시 캐시 서버로 요청 후 프록시 서버에서 원 서버로 요청 할 때
순간 네트워크 단절로 인해 원 서버에 접근이 불가하면 뭐라도 보여주는게 좋겠지 라는 느낌의 판단으로 과거 데이터를 허용하는 200 OK 응답을 날림.
만약 돈과 관련된 중요한 정보일 경우 이는 매우 치명적일 수 있음!

이때 must-revalidate를 사용하면, 프록시 캐시 <> 원 서버 간의 네트워크 단절이 이루어지면 200 OK를 클라이언트로 보내는 것이 아닌 
**504 Gateway Timeout**을 전송함
