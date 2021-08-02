### 상황에 따른 API 설계

 

#### 클라이언트 -> 서버 데이터 전달 방식

 

1)쿼리파라미터를 통한 데이터 전송

-get, 검색시 사용  

 

2)메시지 바디를 통한 데이터 전송

-post, put, patch / 회원가입, 상품주문, 리소스등록, 리소스 변경  

 
 *** 

 

#### 클라이언트 -> 서버 데이터 전송 상황

-정적 데이터 조회 / 동적 데이터 조회 / HTML form을 통한 데이터 전송 / HTTP API를 통한 데이터 전송  

 

1)정적 데이터 조회

-이미지, 문서 조회. 정적 데이터는 일반적으로 쿼리파라미터 없이 조회 가능  

 

2)동적 데이터 조회

-검색, 게시판 목록, 필터  

 

3)HTML form 데이터 전송(=Control URI)

-get, post만 지원. ajax 기술쓰면 확장 가능 하긴 함.

-등록폼, 수정폼 가져오기는 get사용, 등록요청/수정요청은 post

-본 방식에서는 uri가 계속 겹치기 때문에, delete의 경우 어쩔 수 없이 delete라는 것을 뒤에 붙여서 uri를 처리함.

-HTTP 메소드로 해결하기 정말~ 애매한 경우 control URI를 사용함~

-get, post만 지원하는 제약을 해결하기 위해 동사로된 리소스 경로 사용 /new, /edit, /delete 등..

 

/members

/members/new

/members/{id}

/members/{id}/edit 

/members/{id}/delete

 

-get은 form 태그 사용한 단순 조회에만 사용하는게 바람직함.

->게시물 검색 시 사용

-get으로도 전송은 할 수 있지만, 데이터를 쿼리파라미터에 넣어서 서버에 전달하기에 리소스 수정 변경에는 사용하면 취약함!

 

-post content-type : application/x-www-form-urlencoded  -> key&value 형태의 데이터

->form의 내용을 key & value 형태로 전송 해줌 / 전송 데이터를 url encoding 처리

 

-파일 전송 시 multipart/form-data;

-다른 종류의 여러 파일과 폼의 내용을 함께 전송 가능

 

4)HTTP API(=RESTful API) 데이터 전송; HTML form을 쓰지 않는 대부분의 상황

-서버 to 서버(백엔드 통신), 앱클라이언트(아이폰, 안드로이드), 웹클라이언트(ajax, react, vueJS), post&put&patch로 메시지 바디 통해 데이터 전송, get은 조회며 쿼리파라미터로 데이터 전달, content-type은 application/json으로 표준화해서 사용! 거의 대부분!
-게시글 수정 시 put 사용, 전체 다시 보내는 개념 / 회원정보 일부 수정은 patch 사용

-HTTP API와 Restful API는 실질적으로 다르지만 실무에서는 거의 같은 의미로 사용함.

 

 *** 


#### URI 설계 개념
-리소스 기준으로만 1차 설계 -> 안되는 것만 컨트롤러 URI로 2차 처리!
-참조용 : https://restfulapi.net/resource-naming/

1)Document
-단일 개념 : 파일하나, 객체인스턴스, DB row
-/members/100, /files/star.jpg

2)Collection
-서버가 관리하는 리소스 디렉토리
-서버가 리소스의 URI를 생성하고 관리
-대부분에 해당됨
-/members

3)Store
-클라이언트가 관리하는 자원 저장소
-클라이언트가 리소스의 URI를 알고 관리
-예외적으로 파일이나, 일부 게시판에 적합
-/files

4)Controller, Control URI
-document, collection, store로 해결 안될 때 사용
-동사를 직접 사용!
