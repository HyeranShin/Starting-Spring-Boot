# 7장 RESTful 게시판 만들어 보기
## 7.1 REST란?
REST란 REpresentational State Transfer의 약자로, HTTP의 창시자 중 한 사람인 로이 필딩(Roy Fielding)이 2000년에 발표한 박사 학위 논문 <Architectual Styles and the Design of Network-based Software Architectures>에서 소개했음. 로이 필딩은 기존의 웹 아키텍처가 HTTP 본래으 우수성을 잘 활용하지 못한다고 생각했기 때문에 그 장점을 최대한 활용할 수 있는 아키텍처로 REST를 소개했음<br/>
REST는 잘 표현된 HTTP URI로 리소스를 정의하고 HTTP 메서드로 리소스에 대한 행위를 정의함. 리소스는 JSON, XML과 같은 여러 가지 언어로 표현할 수 있음<br/>
REST의 특징을 지키는 API를 'RESTful하다'라고 표현하기도 함<br/>

<b>리소스</b><br/>
리소스는 서비스를 제공하는 시스템의 자원을 의미하는 것으로 URI(Uniform Resource Identifier)로 정의됨. 즉, REST API의 URI는 리소스의 자원을 표현해야만 함<br/>
REST API의 URI를 설계할 때 일반적으로 다음과 같은 규칙을 적용함
1. URI는 명사를 사용
예를 들어 회원 목록을 조회하는 REST API는 다음과 같이 표현할 수 있음<br/>
<b>GET</b>&nbsp;&nbsp;&nbsp;&nbsp;/members<br/>
<b>GET</b>은 HTTP 메서드로 URI의 리소스를 조회하는 것을 의미함. members라는 명사를 통해 회워 목록임을 알 수 있음
2. 슬래시(/)로 계층 관계를 나타냄
예를 들어 개라는 목록에는 진돗개나 불독, 사모예드와 같은 서로 다른 여러 가지의 종이 있음. 따라서 개와 진돗개의 계층 관계는 다음과 같이 나타냄
<b>GET</b>&nbsp;&nbsp;&nbsp;&nbsp;/dogs/jindo<br/>
3. URI의 마지막에는 슬래시를 사용하지 않음
URI으 끝에 슬래시를 넣더라도 실행하는데는 문제가 없음. 그렇지만 슬래시 때문에 다음 계층이 있는 것으로 오해할 수가 있음. 따라서 명확한 의미를 전달하려면 URI의 마지막에 슬래시를 넣지 않아야 함
4. URI는 소문자로만 작성
URI 문법 형식을 나타내는 RFC 3986에서는 URI 스키마와 호스트를 제외하고는 대소문자를 구별하도록 규정하기 때문임. 물론 소문자만이 아니라 대문자 혹은 대소문자를 사용하는 것도 가능하지 않냐고 이야기할 수 있음. 대소문자를 섞어서 사용하면 URI를 기억하기 어렵고 URI를 호출할 때 잘못 쓰기 쉬움. 대문자로만 구성할 경우 이러한 문제는 없지만 일반적으로 URI에 대문자는 잘 사용하지 않음
5. 가독성을 높이기 위해 하이픈(-)을 사용할 수는 있지만 밑줄(_)은 사용하지 않음

<b>HTTP 메서드</b><br/>
HTTP에는 여러 가지 메서드가 있는데, REST 서비스에서는 CRUD에 해당하는 네 개의 메서드를 사용함. CRUD란 Create, Read, Update, Delete의 약자로 소프트웨어의 기본적인 데이터 처리 기능을 나타냄

|HTTP 메서드|의미|역할|
|-|-|-|
|POST|Create|리소스 생성|
|GET|Read|해당 URI의 리소스 조회|
|PUT|Update|해당 URI의 리소스 수정|
|DELETE|Delete|해당 URI의 리소스 삭제|
