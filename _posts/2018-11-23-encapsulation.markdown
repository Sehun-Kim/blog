---
title: "[Java] 객체지향 4대 특성 - 캡슐화"
layout: post
date: 2018-11-23
image:
headerImage: false
tag:
- Java
- 객체지향의 4대 특성
- 캡슐화
category: blog
author: sehunkim
description: 객체지향의 4대 특성 중 캡슐화에 대해 알아보자
star: false # true로할 경우 제목에 highlight 처리가 됨
sitemap :
  changefreq : daily
  priority : 1.0
---

## 캡슐화란?
**정보은닉이다.**

정보은닉을 하게되면 객체의 내부로직을 알 수 없게 되고 접근할 수 있는 부분이 제한되기 때문에 다른 코드와의 `결합도`가 낮아진다.때문에 코드의 변경에 유연하게 대처할 수 있다.
> 의존성이 낮아진다. => 객체지향 5원칙 중 DIP(의존성역전원칙)

`속살을 드러내지 말고 interface만 제공해라~`

##### 정보은닉을 하는 이유는 **변경**에 유연하게 대처하기 위해서다.

### 정보은닉을 위한 방법
#### 1. 표준을 따르게한다. (공통 상위클래스/ 인터페이스)
- **상속 사용**

**`하위클래스` is kind of `상위클래스`**

```java
class Car {
  int speed;

  Car(int speed) {
    this.speed = speed;
  }

  public void driving() {
    System.out.println(speed + "km로 달리고 있습니다.");
  }
}

class SportsCar extends Car {
  SportsCar() {
    super(200);
  }
}

class Sedan extends Car {
  Sedan() {
    super(100);
  }
}

class Driver {
  public static void main(String[] args) {
    Car sportsCar = new SportsCar();
    Car sedan = new Sedan();

    sportsCar.driving(); // 200km로 달리고 있습니다.
    sedan.driving(); // 100km로 달리고 있습니다.
  }
}
```
- **인터페이스 사용**

**`구현체` is able to `interface`**

인터페이스의 목적은 **구현강제** (표준준수)에 있다.

```java
interface 날수있는 {
    void fly();
}

class 박쥐 extends 포유류 implements 날수있는 {
    @Override
    public void fly(){
        System.out.println("박쥐가 난다.")
    }
}
```
**박쥐 class**는 **포유류 class**를 **확장**하고 **날수있는** 기능을 구현한다.

```java
interface 헤엄칠수있는 {
    void swim();
}

class 팽귄 extends 조류 implements 헤엄칠수있는 {
    @Override
    public void swin(){
        System.out.println("팽귄이 수용한다.")
    }
}
```
**팽귄 class**는 **조류**를 상속받고 있지만 **날수있는**기능은 없고 **헤엄칠수있는** 기능이 있다.

#### 2. 필드자체가 아닌 메소드를 이용해 노출 (getter메소드)

```java
class Person {
  private String name;
  private int age;

  Person(String name, int age) {
    this.name = name;
    this.age = age;
  }

  public String getName() {
    return this.name;
  }

  public int getAge() {
    return this.age;
  }
}

class Driver {
  public static void main(String[] args) {
    Person dom = new Person("sehun", 28);

    // 필드를 그대로 노출했을 때 dom.name = "sechun"; 외부에서 맘대로 변경가능하기 때문에
    // 접근제어자를 private로 만들어 외부에서 접근할 수 없게한다.
    // System.out.println(dom.name); (x)
    System.out.println(dom.getName()); // getter 메소드로 필드 값을 접근
  }
}
```

#### 3. 접근제어자 (public, private, protected, default)
![Screenshot]({{ site.url }}/assets/images/cap1.PNG)

### 상속 | 캡슐화

- 상위 클래스는 풍성할 수록 좋은가? 빈약할 수록 좋은가?

```
상위 클래스는 풍성할 수록 좋다. (재사용할 수 있는 것이 많을 때 좋다.)
상위가 풍성해지면 하위에서 반복해야 할 일이 줄어들어 '중복'을 제거할 수 있다.
팽귄 -> 조류 -> 동물
```

- 인터페이스는 풍성할 수록 좋은가? 빈약할 수록 좋은가?

```
인터페이스는 빈약(최소)할 수록 좋다.
때문에 빈약한 인터페이스를 여러개 만드는게 좋다.
```

### 현실속의 캡슐화?
- 의복 : 몸을 보호하면서 변화에 대응 (봄, 여름, 가을, 겨울)
- 화장 : 얼굴?을 보호하고싶다. 이뻐지고 싶다. 세상을 변화시키지말고 `나`를 변화시키면 된다. (**OCP**)

---
## 참고
> - [스프링 입문을 위한 자바 객체지향의 원리와 이해](http://wikibook.co.kr/java-oop-for-spring/)
> - [김종민님 유튜브강의](https://www.youtube.com/playlist?list=PLhDpFstysKKlKoYLivzTVcJxBlvrOb4rM)
> - [김종민님 강의 github](https://github.com/expert0226/oopinspring)
