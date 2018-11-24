---
title: "[Java] 객체지향 4대 특성 - 다형성"
layout: post
date: 2018-11-23
image:
headerImage: false
tag:
- Java
- 객체지향의 4대 특성
- 다형성
category: blog
author: sehunkim
description: 객체지향의 4대 특성 중 다형성에 대해 알아보자
star: false # true로할 경우 제목에 highlight 처리가 됨
sitemap :
  changefreq : daily
  priority : 1.0
---

## 다형성이란?

**사용편의성** 증진을 위해 사용

### 오버라이딩과 오버로딩?
![Screenshot]({{ site.url }}/assets/images/033.png)

- ride 올라타다. (재정의 한다.)
- load 적재하다. (중복 정의한다.)

#### 코드
- **Animal**

```java
public class Animal {
	public String name;

	public void showName() {
		System.out.printf("안녕 나는 %s야. 반가워\n", name);
	}
}
```

- **Penguin**

```java
public class Penguin extends Animal {
	public String habitat;

	public void showHabitat() {
		System.out.printf("%s는 %s에 살아\n", name, habitat);
	}

	//오버라이딩 - 재정의: 상위클래스의 메서드와 같은 메서드 이름, 같은 인자 리스트
	public void showName() {
		System.out.println("어머 내 이름은 알아서 뭐하게요?");
	}

	// 오버로딩 - 중복정의: 같은 메서드 이름, 다른 인자 리스트
	public void showName(String yourName) {
		System.out.printf("%s 안녕, 나는 %s라고 해\n", yourName, name);
	}
}
```

- **Driver**

```java
public class Driver {
	public static void main(String[] args) {
		Penguin pororo = new Penguin();

		pororo.name = "뽀로로";
		pororo.habitat = "남극";

		pororo.showName(); // 어머 내 이름은 알아서 뭐하게요?
		pororo.showName("sehun"); // sehun 안녕, 나는 뽀로로라고 해
		pororo.showHabitat(); // 뽀로로는 남극에 살아

		Animal pingu = new Penguin();

		pingu.name = "핑구";
    pingu.showName(); // 어머 내 이름은 알아서 뭐하게요?
  }
}
```

#### ```pororo```와 ```pingu``` 객체의 T 메모리 매핑과정
> JVM의 메모리를 추상화한 T 메모리란 용어에 대해 알고 싶으면 [이 글]({{ site.url }}/JVM)을 읽어볼 것

![Screenshot]({{ site.url }}/assets/images/poly1.PNG)
main()에서 ```pororo```는 **Penguin** 타입의 객체이고, ```pingu```는 **Animal** 타입의 객체이다. ```pingu```는 ```heap 영역```에서 **Animal**을 가리키고 있고, ```pororo```는 **Penguin**을 가리키고 있다.

showName()은 **Animal**에 있는 메소드를 **Penguin** 이 오버라이딩한 것이기에, Animal 타입으로 showName()을 호출해도 **Penguin** 이 가진 메소드가 덮어씌어져서 호출되게 된다.

> 다형성은 개발자의 편의를 위해 필요하다. (근데 오버로딩은 다형성이 맞는지 모르겠다..)

```
ex)  더하는 프로그램을 만들 때, 오버로딩이 없으면 *addChar(char, char), addInt(int, int), addLong(long, long), 등등* 같은 일을 하면서 다른 인자를 받는 것들을 여러 이름으로 새로 만들어야한다.
```
---

### Tip : Call By

기본 자료형이던 참조자료형이던 **해당 변수가 가지고 있던 것이 전달된다.**

이 것을 전달 받았을 때 값(프리미티브 타입)인지 주소인지 해석하는 것.

#### 1. Call By Value

- 대입문 사용 시 : =
- 인자 전달 시 : public void print(String str) {}
- 메소드 리턴 시 : return x;

#### 2. Call By Reference

- ```stack 영역```에서 변수는 ```heap 영역```의 객체 주소를 값으로 갖고 있다.

---
## 참고
> - [스프링 입문을 위한 자바 객체지향의 원리와 이해](http://wikibook.co.kr/java-oop-for-spring/)
> - [김종민님 유튜브강의](https://www.youtube.com/playlist?list=PLhDpFstysKKlKoYLivzTVcJxBlvrOb4rM)
> - [김종민님 강의 github](https://github.com/expert0226/oopinspring)
