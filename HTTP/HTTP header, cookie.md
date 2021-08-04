### HTTP 일반 헤더

-HTTP 전송에 필요한 모든 부가정보를 담음. 메시지 바디 내용 & 크기, 압축, 인증, 요청클라이언트, 서버정보, 캐시관리정보 등 매우 많은 정보가 담김

-RFC 7230~7235 등장!! in 2014; entity -> representation로 용어 변경

-body가 실제 전달할 데이터를 가짐; 메시지 본문을 payload라고 함.

-헤더는 바디 내 데이터를 해석할 수 있는 정보를 제공함

***
 

#### Represesntation - 표현

-JSON, XML, HTML 등으로 표현하는 것 / 전송과 응답에 둘다 사용 할 수 있음

 

1)Content-Type : 데이터의 형식 

 

text/html; charset=utf-8

applicataion/json   //디폴트가 utf-8

image/png

 

2)Content-Encoding : 데이터의 압축 방식

 

데이터 전달 시 압축 후 인코딩 헤더 추가(주로 gzip), 데이터를 읽는 쪽에서 인코딩 헤더 정보로 압축 해제

 

3)Content-Language : 데이터의 자연어, 한국어인지 영어인지

 

표현 데이터의 자연 언어를 표현, ko, en, en-US

 

4)Content-Length : 데이터의 길이

 

바이트 단위 길이, Transfer-Encoding을 사용하면 content-length를 사용하면 안됨.

 
 ***

 

#### Content-negotiation

-클라이언트가 선호하는 표현 요청; request 시에만 사용

 

-Accept-Language 클라이언트가 선호하는 자연 언어  // 다중언어 지원 서버에 accept-language:ko로 보내면 한국어로 응답해줌

 

quality values(q)

accept-language: ko-KR, ko;q=0.9, en-US;q=0.8, en;q=0.7  // 0~1 기준 클수록 높은 우선순위를 가지며, 생략 시 1임.

 
-Accept 클라이언트가 원하는 미디어 타입 전달

 -가장 구체적인 것이 우선한다.

text/*, text/plain, text/plain;format=flowed, */*

 

-Accept-Charset 클라이언트가 선호하는 문자 인코딩

-Accept-Encoding 클라이언트가 선호하는 압축 인코딩

 
***
 

#### 전송방식

-단순전송 : 한번에 요청하고, 한번에 받는 기본적인 전송

-압축전송 : Content-Encoding : gzip과 같이 압축해서 날림,

-분할전송 : Transfer-Encoding : chunked와 같이 특정한 덩어리로 쪼개서 보내는 형태 / 용량이 커서 한번에 보낼 수 없을 때 사용 / Content-length는 포함하면 안됨

-범위전송 : Content-Range : bytes 1001-2000

***
 


#### 일반정보

-from 유저 에이전트의 이메일 정보 / 검색 엔진에서 주로 사용 / 요청에서 사용 / 실제로는 거의 안씀

-Referer 이전 웹 페이지 주소(뒤로가기 기준)  / 유입 경로 분석 가능 / 요청에서 사용 / referrer라는 단어의 오타, 잘못 적용된채로 표준화되어 버림;

-User-agent 클라이언트의 애플리케이션 정보(웹브라우저) / 서버입장에서는 어떤 종류의 브라우저에서 장애가 발생하는지 파악 가능 / 통계정보 생산 가능 / 요청에서 사용

-Server 요청을 처리하는 origin server(최종 도달하는 서버)의 소프트웨어 정보 / 응답에서 사용

-Date 메시지가 발생한 날짜와 시간 / 응답에서 사용

***
 

#### 특별한정보

-host 요청에서 사용하며 필수 헤더값임. 하나의 서버가 여러 도메인을 처리 해야 할 때 유용함. aaa.com, bbb.com 중, host 상 입력된 곳으로 감

-location 3xx response 결과에 따라 리다이렉션할 위치

-allow 허용 가능한 HTTP 메서드 / 잘 사용 하지 않음 / 405(method not allowed)에서 응답에 포함해야함.

-retry-after 유저가 다음 요청을 하기까지 기다려야 하는 시간 / 503(service unavailable) 서비스가 언제까지 불능인지 알려줄 수 있음 / retry-after:120(초단위) or retry-after:Fri, 31 Dec 2030 23:59:59 GMT(날짜표기) / 잘 사용 하지 않음

***
 

#### 인증

-Authorization 클라이언트 인증 정보를 서버에 전달; 

-WWW-Authenticate 리소스 접근 시 필요한 인증 방법 정의 / 401 unauthorized 응답과 함께 사용해야 함

***
 

### 쿠키

-Set-Cookie 서버에서 클라이언트로 쿠키 전달

-Cookie 클라이언트가 서버에서 받은 쿠키를 저장하고, HTTP 요청 시 서버로 전달

-쿠키 미 사용시, 모든 요청에 사용자 정보를 다 포함해야 하는데... 개발자 업무로드 + 보안적으로 좋지 않음..

-쿠키 사용 시, 서버 측에서 Set-Cookie를 통해서 user=홍길동이라고 처리 후 응답하고, 클라이언트 웹브라우저가 쿠키 저장소에 user=홍길동으로 저장 후, 요청에 쿠키 정보를 자동으로 포함함.

 

ex)set-cookie: sessionId=abcd1234; expires=Sat, 26-Dec-2020 00:00:00 GMT; path=/; domain=.google.com; Secure

 

-사용자 로그인 세션 관리, 광고 정보 트래킹에 사용

-쿠키 정보는 항상 서버에 전송되므로, 추가 네트워크 트래픽을 유발함. 따라서 세션id나 인증 토큰 등 최소한의 정보만 사용해야함. 서버에 전송하지 않고, 웹 브라우저 내부에 데이터를 저장하고 싶으면 웹스토리지(localStorage, sessionStorage) 사용 필요

-보안에 민감한 정보(주민번호, 신용카드번호 등)는 저장하면 안됨!!

***
 

#### 쿠키생명주기

-Set-Cookie: expires=Sat, 26-Dec-2020 04:39:21 GMT   

->만료일이되면 쿠키 삭제

 

-Set-Cookie : max-age=3600(sec)

->0이나 음수를 지정하면 쿠키 삭제

 

-세션쿠키는 만료 날짜를  생략하면 브라우저 종료시까지만 유지

-영속 쿠키는 만료 날짜를 입력하면 해당 날짜까지 유지

***
 

#### 도메인

-명시한 문서 기준 도메인 + 서브 도메인 포함

 

domain=example.org

->example.org 및 dev.example.org에도 처리됨

 

-example.org에서 쿠키를 생성하고 domain 지정을 생략

->example.org에서만 쿠키 접근 / dev.example.org는 쿠키 미접근


***
 

#### 경로

-경로를 포함한 하위 경로 페이지만 쿠키 접근

-일반적으로 path=/ 루트로 지정

 

path=/home이면, /home, /home/level1은 가능하고, /hello는 전달되지 않음

 
 
***


#### 보안

-Secure를 넣으면 Https인 경우에만 전송; 없을 시 http, https 모두에게 전송

-HttpOnly는 XSS 공격 방지, 자바스크립트에서도 접근 불가(원래는 접근 가능), HTTP 전송에만 사용

-SameSite는 XSRF 공격 방지, 요청 도메인과 쿠키에 설정된 도메인이 같은 경우만 쿠키 전송


***
 

