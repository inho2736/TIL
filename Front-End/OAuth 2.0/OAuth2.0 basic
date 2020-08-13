#### 개요

User	myApp	Google

유저는 구글에 가입되어 있고, 이 것을 통해 내 앱에서 구글의 서비스를 이용하고 싶다.

근데 그렇다고 유저가 내 앱에게 구글 아이디와 비밀번호를 넘겨줄 순 없다.

유저는 불안하고, 앱은 그걸 유출 막을 방법도 찾아야 하고, 구글은 회원정보가 남한테 있으니 짜증난다.



그래서 아이디와 비밀번호 대신, 다른 인증방법인 accessToken을 넘겨주는 것을 고안해 냈고 그 것을 "oAuth" 라고 한다.



accessToken 장점 

- 아이디 비번 아니고 다른 인증법
- 플랫폼의 모든 서비스를 이용할 권한 x, 내 앱이 이용할 것에 대한 권한



#### 역할

User = Resource Owner

- 자원의 소유자 

myApp = Client

- 자원을  사용할 주체

Google = Resource Server 

- client 가 활용할 자원(데이터)를 가지고 있음

(공식문서에는 Authorization Server라고 인증서버를 따로 구별함)



#### 등록

등록(Register) 이란?

- Client 가 Resource Server에 서비스 이용을 알리는 것

  

등록에서

Client ID

Client Secret

Authorized redirect URIs - 서버가 권한을 줄 때 '이 주소로 전달해 주세요' 라고 예약



#### Resource Owner의 승인

< 구글로 로그인 하기> 버튼을 누르면 

https://resource_server/?client_id=1&scope=B,C&redirect_uri=https://client/callback 링크로 이동

그럼 resource server가 아이디가 등록되어 있는지, 리다이렉트 주소가 맞는지 확인 후 Resource Owner에게 로그인 시키고, B와 C 기능에 대한 동의를 얻음.

그러면 Resource server 는 user1이  B와 C 기능에 동의했음을 데이터로 저장



#### Resource Server의 승인

>Resource Server는 Client가 등록된 Client가 맞는지 확인하기 위해서 Resource Owner을 통해서 Client에게 Authorization code를 전달합니다. 이 값을 받은 Client는 이 값과 Client secret의 값을 Resource Server로 전송해서 Client의 신원을 Resource Owner에게 증명합니다. 

위에서 Resource server가 그 데이터를 저장 후 , 해당 Resource Owner에 대한 authorization code(3)를 만들어 함께 저장. 

그리고 헤더에 Location : https://client/callback?code=3 을 넣어 Owner도 모르게 해당 주소에 접속하게 함. 그러면 Client가 그 주소를 통해서 authorization code를 알게 됨.

이후 Client가 Resource server에 https://resource_Server/token?grant_type=authorization_code&code=3&redirect_uri=https://client/callback&client_id=1&client_secret=2 를 전달

Resource server는 저 정보들을 전부 확인 후,  AccessToken 발급.



#### AccessToken

Resource server가 authorization code를 확인하고 나서는 그 데이터를 지워버림. 다시 확인 안할라고. Client도 지워버림.

대신 Resource server는 AccessToken를 생성해 저장하고 Client에 전송. Client도 이를 저장.

이후 Client가 토큰을 Resource server에 전달하면, 서버는 ' 이건 B와 C를 이용하는데 동의한 user1에 대한 것이구나' 하고 알게됨.



#### API 호출

api 주소로 가져오는데 , 주소 뒤에 parameter로 토큰을 부여하는 방식은 간단하나 위험.

그래서 헤더로 토큰을 전송하는 것이 더 바람직



#### Refresh Token

![image-20200810144337685](/home/inho/.config/Typora/typora-user-images/image-20200810144337685.png)

출처 > rfc oAuth 2.0  https://tools.ietf.org/html/rfc6749#section-1.5