# OAuth 
- 인터넷 사용자들이 비밀번호를 제공하지 않고 다른 웹사이트 상의 자신들의 정보에 대해 웹사이트나 애플리케이션의 접근 권한을 부여할 수 있는 공통적인 수단으로서 사용되는, 접근 위임을 위한 개방형 표준이다. (위키백과)

- 외부 어플리케이션에서 사용자 인증을 위해서 Naver, Google 등의 사용자 인증 방식을 사용하는데, 이 때 외부 어플리케이션은 Naver, Google 등의 특정 자원 및 서비스를 접근, 사용할 수 있는 권한을 인가받음.

- OAuth 1.0 A 버전을 이후 OAuth 2.0 임.

<img src="https://user-images.githubusercontent.com/75015048/153121311-61fbd16a-57b9-4b2a-b15e-ff7102a6aae8.png" width="80%" />

## OAuth 역할

- **리소스 소유자(Resource Owner, User)** : 애플리케이션이 계정에 액세스할 수 있도록 승인하는 사용자(**Client가 제공하는 서비스를 통해 로그인하는 실제 유저**), 사용자 계정에 대한 애플리케이션의 액세스는 부여된 권한 범위(예: 읽기 또는 쓰기 액세스)로 제한

- **클라이언트(Application, Client)** : 사용자의 승인을 받고 API에서 승인을 확인한 뒤 이를 토대로 사용자의 계정에 액세스하려는 응용 프로그램
- **리소스 서버(Resource Server)** : 보호된 사용자 계정을 호스팅

- **권한 부여 서버(Authorization Server)** : 사용자 의 ID를 확인한 다음 응용 프로그램 에 액세스 토큰을 발급하는 서버, 애플리케이션 개발자의 관점에서 서비스의 API는 리소스 및 권한 부여 서버 역할을 모두 수행 API 서버

## OAuth protocol Flow
<img src="https://user-images.githubusercontent.com/75015048/153142439-c8fcc79e-9133-4566-87b2-754e0f493d7f.png" width="80%" />

- application이 user에게 서비스 리소스에 액세스하기 위한 권한 부여를 요청

- user가 요청을 승인한 경우 application은 권한 부여를 받음.
- application은 자신의 신원에 대한 인증과 권한 부여를 제시하여 authorization server (API)에 access token을 요청
- application 신원이 인증되고 권한 부여가 유효한 경우 authorization server (API)는 응용 프로그램에 대한 access token을 발급하고, 승인이 완료됩니다.
- application 은 resource server (API) 에서 자원(서비스)을 요청하고 인증을 위한 access token을 제시합니다.
access token이 유효한 경우 resource serve (API)가 application에 리소스를 제공

## Client 등록
애플리케이션에서 OAuth를 사용하기 전에 서비스에 애플리케이션을 등록해야 합니다. 이는 서비스 웹사이트(구글, 네이버 ...)의 개발자 또는 API 부분에 있는 등록 양식을 통해 수행되며, 서비스 웹 사이트에서 다음 정보(및 애플리케이션에 대한 세부 정보)를 입력해야합니다.
- Application Name
- Application Website (Homepage URL)
- Redirect URI or Callback URL (Authorization callback URL)

- **아래는 깃허브 OAuth 등록 창 Example**

  <img src="https://user-images.githubusercontent.com/75015048/153144719-664d58a6-636d-4fa4-ad45-8b4dce0a8382.png" width="50%" />

### Client ID and Client Secret
Client 등록을 통홰 아래 세 가지 정보를 부여받음.
- Client ID : 클라이언트 웹 어플리케이션을 구별할 수 있는 식별자
- Client Secret : Client ID에 대한 비밀키로 절대 노출되선 안됨.
- Authorized **redirect URL** : Authorization Code를 전달 받을 리다이렉트 주소 (Client 어플리케이션에서 인증 데이터를 받을 주소)

- **아래는 구글 OAuth Client 등록 후 발급 받은 데이터 정보**
```json
{"web":
    {
        "client_id":"본인의 클라이언트 ID"
        ,"project_id":"본인의 프로젝트 이름"
        ,"auth_uri":"https://accounts.google.com/o/oauth2/auth"
        ,"token_uri":"https://oauth2.googleapis.com/token"
        ,"auth_provider_x509_cert_url":"https://www.googleapis.com/oauth2/v1/certs"
        ,"client_secret":"본인의 client_secret"
        ,"redirect_uris":["http://localhost:8080/auth/google/callback"]
        ,"javascript_origins":[
            "http://localhost:8080"
        ]
    }
}
```

# 권한 부여 (Authorization Grant)
크게 4가지로 
1. 인증 코드 (Authorization Code)
2. 암시적 ( Implicit )
3. 사용자 암호 자격 증명 ( Resource Owner Password Credentials )
4. 클라이언트 자격 증명 ( Client Credentials )

- [an introduction to oauth 2](https://www.digitalocean.com/community/tutorials/an-introduction-to-oauth-2) 를 참고 했더니 Implicit 와 Password Grant 권한 부여 유형은 안전하지 않은 것으로 간주되어 사용을 안하는 것이 좋다고 한다. 
- 템플릿을 이용해서 만드는 경우 백엔드에서 처리하여 인증코드 방식을 이용
- 프론트엔드와 백엔드가 나눠진 경우는 프론트엔드에서 사용자 토큰을 받은 뒤 백엔드에서 프론트 엔드에서 보낸 사용자 토큰을 우리 사이트에서 사용할 JWT 토큰으로 반환해주는 방식 이용한다고 한다.


# Spring Oauth 2.0
- depency 추가
- [mvnrepository spring-boot-starter-oauth2-client](https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-oauth2-client)

```bash
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-oauth2-client</artifactId>
</dependency>

implementation group: 'org.springframework.boot', name: 'spring-boot-starter-oauth2-client'
```
- application.yml 파일
```yml
spring:
  security:
    oauth2:
      client:
        registration:
          google:
            client-id: <client id value>
            client-secret: <client secret value>
            scope:
            - email
            - profile
            ...
```

- Config 클래스

```java
@Configuration
@EnableWebSecurity // 스프링 시큐리티 필터가 스프링 필터 체인에 등록 
public class SecurityConfig extends WebSecurityConfigurerAdapter{
	
    ...
	
	@Override
	protected void configure(HttpSecurity http) throws Exception {
		http.csrf().disable();
		http.authorizeRequests()
		  ...
		  .and()
			  .formLogin()
			  .loginPage("/loginForm")
			  .loginProcessingUrl("/login") 
			  .defaultSuccessUrl("/")
		  .and()
		  	.oauth2Login()
		  	.loginPage("/loginForm")
		  	.userInfoEndpoint()
		  	.userService( DefaultOAuth2UserService 구현체 );
            // 구글 로그인이 완료된 뒤의 후처리(DefaultOAuth2UserService) 코드X, (액세스 토큰 + 사용자프로필정보 O) 
	}
	
}
```

- **DefaultOAuth2UserService** 를 상속받은 클래스에서 **loadUser() 인자인 OAuth2UserRequest(userRequest)** 의 데이터를 찍은 것

```
{
    sub=103643394868242208269,
    name=박지수, 
    given_name=박지수, 
    picture=https://lh3.googleusercontent.com/a/AATXAJzVt88Y0b7tzAY2d3P2p76-h-qMXwz4LvyRC8-R=s96-c,email=jsmini3814@gmail.com, 
    email_verified=true, 
    locale=ko
}
```

- 기존의 **UserDetails** 와 **OAuth2User** 를 Implements 한 구현체를 이용해 일반 로그인과 OAuth 로그인 둘다 가능한 클래스 구현 

```java
  OAuth2User 를 파헤치면  -> OAuth2AuthenticatedPrincipal { getAttributes() } -> AuthenticatedPrincipal { getName() } 

    @Override 
public Map<String, Object> getAttributes() {
	return null;
}

@Override
public String getName() {
	return null;
}
```
- 다른 일반 로그인인지 OAuth로 받은 정보인지 데이터베이스에 보관 할 수 있도록 설계한다.

# 참고
[An Introduction to OAuth 2 - Mitchell Anicas](https://www.digitalocean.com/community/tutorials/an-introduction-to-oauth-2)

[OAuth, OAuth2](https://redbinalgorithm.tistory.com/443)


[OAuth Core 1.0 개정판 A](https://oauth.net/core/1.0a/)

[OAuth Core 1.0](https://oauth.net/core/1.0/)

[rfc5849 - OAuth](https://datatracker.ietf.org/doc/html/rfc5849)

[OAuth 란 무엇일까](https://showerbugs.github.io/2017-11-16/OAuth-%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%BC%EA%B9%8C)

[OAuth 개념 및 동작 방식 이해하기](https://tecoble.techcourse.co.kr/post/2021-07-10-understanding-oauth/)

[Web2 - OAuth2, Egoing Lee](https://www.inflearn.com/course/web2-oauth2/)

[Spring OAuth2](https://docs.spring.io/spring-security/reference/servlet/oauth2/index.html)

[Passport stratege for Google OAuth 2.0](http://www.passportjs.org/packages/passport-google-oauth2/)

[스프링부트 시큐리티 & JWT - 최주호](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8-%EC%8B%9C%ED%81%90%EB%A6%AC%ED%8B%B0/dashboard)

[codingspecialist/Springboot-JWT-React-OAuth2.0-Eazy](https://github.com/codingspecialist/Springboot-JWT-React-OAuth2.0-Eazy)

[\[React\] - 카카오 소셜 로그인](https://velog.io/@seize/React-%EC%B9%B4%EC%B9%B4%EC%98%A4-%EC%86%8C%EC%85%9C-%EB%A1%9C%EA%B7%B8%EC%9D%B8#8-kakaoauthlogin-%ED%95%A8%EC%88%98%EB%A5%BC-%EC%82%AC%EC%9A%A9%ED%95%98%EC%97%AC-%EC%B9%B4%EC%B9%B4%EC%98%A4-%EB%A1%9C%EA%B7%B8%EC%9D%B8-%ED%8C%9D%EC%97%85%EC%B0%BD-%EC%B6%9C%EB%A0%A5-%EB%B0%8F-%EB%A1%9C%EA%B7%B8%EC%9D%B8-%EC%B2%98%EB%A6%AC)

[zkdlu  - OAuth2.0 + JWT를 사용한 토큰 기반 서버 인증 구현하기](https://zkdlu.tistory.com/12)
