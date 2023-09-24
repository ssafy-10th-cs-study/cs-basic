7주차
=

# 로그인

- HTTP의 특징 = 상태없음(stateless) → 그러면 로그인 상태 유지는 어떻게 하는것인가?
- 세션 : 서버와 클라이언트의 연결이 활성화된 상태를 의미
- 세션 ID : 웹 서버 또는 DB에 저장되는 클라이언트에 대한 유니크한 ID

## 세션 기반 로그인

![image](https://github.com/ssafy-10th-cs-study/cs-basic/assets/118447769/a034690c-ee0b-4d1b-88af-f72e74912553)  


- 세션 아이디는 DB에 저장될 수도 있고, 웹 서버에 저장될 수도 있다.
- DB에 저장
    - VARCHAR로 저장
    - 직렬화, 역직렬화에 대한 비용이 발생할 수 있다.
- 웹서버에 저장
    - 사용자가 많아지면 서버 과부화 → 많은 메모리가 소모될 수 있다.

## 토큰 기반 로그인

- 유저의 상태(state)를 token에 넣어두고 서버는 stateless한 상태를 유지
- MSA의 경우, 연쇄적 에러를 막기 위해 토큰과 관련된 서버를 분리한다.
- 토큰은 주로 JWT토큰이 활용된다.

    1. 인증로직 >> JWT토큰생성(access 토큰, refresh 토큰)
    2. 사용자가 이후에 access 토큰을 HTTP Header - Authorization 또는 HTTP Header Cookie에 담아 인증이 필요한 서버에 요청해 원하는 컨텐츠를 가져온다.


### JWT란?

- JSON WEB Token을 의미하며 헤더, 페이로드, 서명으로 이루어져 있다.
- JSON 객체로 인코딩되며 메세지 인증, 암호화에 사용된다.

  ![image](https://github.com/ssafy-10th-cs-study/cs-basic/assets/118447769/4ee8492f-835d-4164-9ced-cd817a010787)  

- Header
    - 토큰 유형과 서명알고리즘, base64URI로 인코딩된다.
- Payload
    - 데이터, 토큰 발급자, 토큰 유효기간, base64URI로 인코딩 된다.
- Signature
    - (인코딩된 header + payload) + 비밀키를 기반으로 헤더에 명시된 알고리즘으로 다시 생성한 서명 값
- **장점**
    - 사용자 인증에 필요한 모든 정보는 토큰 자체에 포함하기 때문에 별도의 인증 저장소가 필요없다.
    - 다른 유형의 토큰과 비교했을 때 경량화되어있다. SAML(Security Assertion Markup Language Tokens)이란 토큰이 있지만 이에 비해 훨씬 경량화되어있다.
    - 디코딩했을 때 JSON이 나오기 때문에 JSON을 기반으로 쉽게 직렬화, 역직렬화가 가능하다.
- 단점
    - 토큰이 비대해질 경우 당연히 서버과부화에 영향을 줄 수 있다.
    - 토큰을 탈취당할 경우 디코딩했을 때 데이터를 볼 수 있다.

### access 토큰과 refresh 토큰

- 로그인은 refresh 토큰과 access 토큰 두개를 기반으로 구현한다.
- access 토큰의 수명은 짧게, refresh토큰의 수명은 길게 한다.
- refresh 토큰은 access 토큰이 만료되었을 때 다시 access 토큰을 얻기 위해 사용되는 토큰
- 이를 통해 access토큰이 만료됐을 때 마다 인증에 관한 비용이 줄어들게 된다.
- 로그인을 하게 되면 access 토큰과 refresh 토큰 두 개를 얻는다.

![image](https://github.com/ssafy-10th-cs-study/cs-basic/assets/118447769/d97fde46-ff80-4166-8bbd-0a2359e9858c)  

![image](https://github.com/ssafy-10th-cs-study/cs-basic/assets/118447769/f5c1899d-9819-4a04-9562-d469963bdfa8)  
