---
title: "[Spring] Request 흐름과 WAS에 대한 아주 짧은 이야기"
layout: post
date: 2019-01-06
image:
headerImage: false
tag:
- Java
- Spring
- Tomcat
- JDBC
- ORM
category: blog
author: sehunkim
description: Spring은 Servlet이다? Tomcat은 WAS다?, JDBC와 ORM은?
star: false # true로할 경우 제목에 highlight 처리가 됨
sitemap :
  changefreq : daily
  priority : 1.0
---

#### Client로부터 Server까지 Http Request 요청의 흐름
![Screenshot]({{ site.url }}/assets/images/webserver.png)

###### Client(Browser) - WebServer(Apache) - WAS(Tomcat) - Servlet(ServletDispatcher) - DI Container - Controller


## Web Server
> Apache, nginx

흔히 웹 서버라 하면 웹 어플리케이션이 설치되어 웹 서비스를 제공하는 서버라고 생각한다. 큰 의미로는 맞는 말이지만, 좁은 의미로 사용한다면 정적 컨텐츠를 제공하는 것이 웹서버이다.

사용자가 웹 서버로 요청을 보내면 먼저 웹서버가 요청을 보고 동적컨텐츠를 제공해야한다면 **WAS** 로 정적컨텐츠를 제공해야한다면 웹 서버에서 직접 응답을 한다.(예시일뿐 모두가 그렇다는 뜻은 아니다) 정적 컨텐츠의 예를 들자면 **변화하는 내용이없는 html 문서**, css, imgae 같은 파일을 말한다.

## CGI?
**Web Server**(Java 기준 Apache)는 이미 저장되어 있는 정적인 컨텐츠(Html)를 응답한다. 만약 사용자가 직접 정보를 찾거나(검색) 기록(저장)을 하려면 웹서버의 문서를 직접 고치는 작업이 필요했다. 때문에 사용자의 입력과 요청 url에 따라 동적으로 변하는 페이지가 필요하게 되었고, 웹서버에서 동적인 처리를 담당할 표준인 **CGI**(Common Gateway Interface)가 등장하게 되었다.

CGI는 서버와 응용 프로그램간에 데이터를 주고받기 위한 방법이나 규약을 뜻한다.
```
1. 웹 브라우저의 HTML의 폼을 통해 요청이 웹 서버로 전달
2. 웹 서버는 요청에 들어 있는 주소가 CGI 프로그램에 대응되는지 확인
3. 대응될 경우 그 프로그램을 실행, 환경 변수와 표준 입력의 형태로 요청을 전달한다.
4. 웹 서버는 CGI 프로그램이 표준 출력으로 돌려 보낸 내용을 그대로 응답으로 돌려 준다.
```

***CGI*** 의 가장 큰 단점은 멀티프로세스 방식이란 것이다. 멀티프로세스 방식은 요청이 올 때 마다 프로세스를 생성한다. 프로세스는 자원을 상당히 많이 차지하므로 이를 해결하기 위해 요청에 대응하여 프로세스가 아닌 스레드를 생성하는 자바의 Servlet이 등장하였다.

## Servlet?
서블릿은 자바에서 만든 확장된 CGI다. 앞서 설명한 기본 CGI와는 다르게 멀티스레드 방식으로 동작한다. 서버가 시작되고 서블릿을 만들게 되면 메모리에 저장해두고, 같은 서블릿을 사용하여 요청을 처리한다.

처음 웹서버로 요청이 전달 되었을때 동적인 컨텐츠를 응답해야 할 경우 동적인 컨텐츠를 전담하는 **Web Application Server** 로 요청을 전달한다.

## WAS?
> Apache Tomcat, jetty

Web Application Server와 Web Server의 가장 큰 차이는 동적처리를 하냐 안하냐 이다. 쉽게 말하면 사용자가 입력폼을 이용하여 값을 넣으면 그 값에 따라 응답하는 내용이 변하는 것이다. 이런 역할을 담당하는 것이 **Servlet** 이고, 서블릿의 관리를 하는 것이 WAS(서블릿 컨테이너)이다.

WAS는 동적컨텐츠뿐만 아니라 정적컨텐츠도 처리할 수 있고(웹서버의 역할도 할 수 있다는 뜻) DB를 조회하는 역할도 담당한다.

JAVA의 대표적인 Servlet Container(WAS)는 Apache Tomcat으로 각 서블릿을 실행하고 관리하는 역할을 대신해준다. 요청마다 스레드를 만들고, 통신을 위한 소켓을 연결하고, 서블릿의 생성과 소멸 주기 관리를 모두 Container가 담당한다.

## Spring MVC
서블릿도 Interface(표준)이므로 구현체가 따로 존재한다. Spring MVC는 웹 어플리케이션을 만드는데 특화된 서블릿 구현체이다.

결국 사용자가 스프링 어플리케이션이 있는 내 웹서버에 요청을 보내게 되면 WebServer -> WAS(Tomcat) -> ServletDispatcher 순으로 요청을 전달 받게 된다. **Spring MVC** 는 ServletDispatcher라는 Servlet으로 요청이 오면 녀석이 요청의 url을 분석하여 해당 요청을 수행할 수 있는 Spring Bean(Controller)에 요청을 보내준다.

Spring MVC에도 서블릿 내에서 객체의 생명주기를 관리하고 실행해줄 역할을 할 Spring(혹은 DI) Container가 존재한다. 해당 [컨테이너]({{ site.url }}/springbean-lifecycle/)에 대한 설명은 이 글에 자세히 되어있다.

```
톰캣이 실행되면 ServletDispatcher를 생성하고 등록한다.
ServletDispatcher가 초기화 될 때 DI Container도 생성되고 초기화된다.
```

---

### JDBC와 Driver의 관계
![Screenshot]({{ site.url }}/assets/images/1541914244823.png)
```
JDBC == 인터페이스
Driver == 구현체
```

JDBC는 자바 프로그램이 DB와 연결하기 위한 표준 API로써 각 DB마다 구현체(Driver)가 존재한다.


### ORM과 JPA, hibernate의 관계
> [ORM이란?](http://www.incodom.kr/ORM)

**Object Relational Mapper** 은 자바 객체와 엔티티를 그대로 연결해주는 mapper이다. Java의 표준 orm 인터페이스 중 유명한 것은 JPA가 있고 hibernate와 EclipseLink가 인터페이스의 유명한 구현체이다.
```
JPA == 인터페이스
Hibernate == 구현체
```
> **정정**
> myBatis는 ORM이 아닌 SQL Mapper이다.
> SQL Mapper는 객체(Object)와 SQL 문을 매핑하여 데이터를 객체화하는 기술을 뜻한다.
> 객체와 관계를 매핑하기보다는 SQL문의 질의 결과와 객체를 매핑시켜주기 때문에 ORM과는 다른 기술이라고 할 수 있다.
> 대표적인 프레임워크로는 iBatis, MyBatis, Oracle SQLJ 가 있다.  

또 JPA와 Spring Data JPA는 조금 다르다.

Spring Data JPA는 **JPA를 좀 더 쓰기 편하게 추상화 시킨 모듈이다.** 바로 EntityManager를 노출해서 사용하는 것이 아니라. Repository라는 인터페이스를 노출해 사용하게 해 좀 더 사용을 간편하게 해준다.


ORM의 밑단에는 DB연결을 위한 JDBC가 동작하고 있다.
