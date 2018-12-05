# Maven

**빌드도구**

프로젝트를 빌드(자동화해서)하는데 도움을 주는 도구

ant -> maven -> gradle 순으로 발전



### maven의 기본 디렉토리 구조

- src/main/java : 배포할 자바 소스 코드(production code) 디렉토리
- src/main/resources : 배포할 설정 파일 디렉토리
- src/test/java : 테스트 자바 소스 코드(test code) 디렉토리
- src/test/resources : 테스트시 필요한 설정 파일 디렉토리
- src/main/webapp : 웹 자원(html, css, javascript)을 관리하는 디렉토리





##### 패키징(현재 프로젝트를 빌드해서 배포본을 만드는 것)

- jar
- war (web으로 배포할 때)



### 라이브러리에 대한 의존성을 메이븐으로 어떻게 관리할까?

1. maven central repository 에서 라이브러리를 검색한다.
2. 일단 <dependencies> </dependencies> 라는 태크로 의존성들을 묶어준다.
3. 그리고 태그 사이에 의존성을 넣어준다.



메이븐은 pom.xml에 있는 설정 내용을 바꾸면 `원격저장소`의 의존성의 artifactId와 version을 토대로 원격저장소에서 검색해서 `groupId/artifactId` 같은 디렉토리 구조로 로컬저장소에 저장한다. (만약 groupId가 .으로 나누어져 있다면 나누어진 만큼 디렉토리를 분리한다.)

- 각 jar 파일도 pom.xml가 존재하고 각 의존관계를 파악하여 maven이 필요한 의존관계를 다 다운로드해준다.

설정파일(pom.xml)은 부모 설정파일(effective POM)을 상속하게 된다.

- 여기서 <build> 태그를 보면 디렉토리 구조를 알 수 있다.





#### maven은 컴파일 기능도 해준다

배포할때는 eclipse같은 도구를 쓸 수 없기 때문에 컴파일해줄 수 있어야한다.

```bash
$ mvn compile # production
$ mvn test # test code 프로덕션코드를 컴파일하고 테스트코드를 컴파일하고 테스트를 실행
$ mvn package # war파일로 패키징해줌
```



#### plugin

- maven의 모든 기능을 담당하는 것
- phases에 맞춰 실행되기도 하지만 독립적으로도 실행가능

default는 jdk 1.5이다. 우리가 쓰는 jdk 버전으로 바꾸고 싶으면 Compiler 버전을 변경해야한다.

pom.xml 파일에서 부모의 설정을 재정의해야하는데  <plugin>에 <artifactId>maven-compiler-plugin</artifactId>를 바꾸어주면 된다.



`plugin`의 **명령어**는 `goal`이라고 한다. (testCompile, package)



터미널에서 플러그인들에 저장되어 있는 명령어로 실행할 수 잇다.

```bash
$ mvn compiler:testCompiler
```





## maven은 빌드 단계(Phase)가 있다.

![maven lifecycle with plugin.png](https://firebasestorage.googleapis.com/v0/b/nextstep-real.appspot.com/o/lesson-attachments%2F-KhznCj3wRoDrQIEqsXG%2Fmaven%20lifecycle%20with%20plugin.png?alt=media&token=a54892dd-8e39-446b-9eb9-b5922caeb885)



phase끼리 의존관계가 있기 때문에 각 단계 전에 선행이 되어야한다.

또한 phase와 연결된 플러그인의 goal들이 있기 때문에 이 골들이 작업을 한다.

![maven lifecycle.jpg](https://firebasestorage.googleapis.com/v0/b/nextstep-real.appspot.com/o/lesson-attachments%2F-KhznCj3wRoDrQIEqsXG%2Fmaven%20lifecycle.jpg?alt=media&token=3becefef-9820-4592-a413-a1003e238009)

```bash
$ mvn [plugin]
```

1. comile

2. test

3. package
4. install : 내 로컬저장소에 jar파일을 배포
5. deploy : 원격저장소에 jar파일 배포

- clean target 디렉토리가 다 사라진다.



tip : plugin을 통해 오픈소스의 java 코드를 알 수있다.



> 참고 https://www.slipp.net/wiki/pages/viewpage.action?pageId=10420233





## Maven Wrapper

이게 없으면 maven을 로컬에 설치하고 PATH에 등록을 해야 사용이 가능하다.



이게있으면 설치과정없이 자동으로 실행하게 도와주는 도구다.

