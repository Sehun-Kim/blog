---
title: "[JAVA] Stream & Lambda"
layout: post
date: 2018-10-29
image:
headerImage: false
tag:
- Java
- Stream
- Lambda
category: blog
author: sehunkim
description: Java 8부터 추가 된 Stream이란?
star: false # true로할 경우 제목에 highlight 처리가 됨
sitemap :
  changefreq : daily
  priority : 1.0
---

## Summary:
Java 8부터 추가된 **Stream** `API`와 **Lambda** `API`가 어떻게 쓰이는지 간단하게 정리해보겠다.
> 참고자료 :
> - [람다식 - 기본 문법, 타겟 타입과 함수적 인터페이스](http://palpit.tistory.com/671)
> - [Java Study Group](https://rebeccacho.gitbooks.io/java-study-group/content/chapter14.html)
> - [JAVA 8 스트림 튜토리얼](https://wraithkim.wordpress.com/2017/04/13/java-8-%EC%8A%A4%ED%8A%B8%EB%A6%BC-%ED%8A%9C%ED%86%A0%EB%A6%AC%EC%96%BC/)
> - [Java8 Stream은 loop가 아니다.](https://www.popit.kr/java8-stream%EC%9D%80-loop%EA%B0%80-%EC%95%84%EB%8B%88%EB%8B%A4/)
> - [람다가 이끌어 갈 모던 Java](https://d2.naver.com/helloworld/4911107)
> -  [Java 8과 함수형 프로그래밍](https://medium.com/@goinhacker/java-8%EA%B3%BC-%ED%95%A8%EC%88%98%ED%98%95-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D-154e6d8830f1)
> - [JAVA 스트림 API](http://iloveulhj.github.io/posts/java/java-stream-api.html)

---

### <a id="1"></a>Lambda 예제 코드
`Calculator`클래스는 `calc()`라는 메소드를 가지고 있다. `calc()`는 매개변수 `n`을 받아 1을 증가시켜 리턴한다.

{% highlight java %}
class Calculator {
  Calculator() {};

  public int calc(int n) {
    return n + 1;
  };
}
{% endhighlight %}

{% highlight java %}
class Driver {
  public static void main(String[] args) {
    int n = 2;
    Calculator cal = new Calculator();

    System.out.println(cal.calc(n)); // 3
  }
}
{% endhighlight %}


## Lambda
Lambda란 메소드를 하나의 식(Expression)으로 표현한 것이다. **익명메소드(함수)** 생성 문법이다. 메소드를 지칭하는 명칭(메소드명)없이 구현부만으로 선언하는 것이다. 하지만 JAVA의 메소드는 **메소드 자체로 혼자 선언하여 쓰일 수 없다.** 무조건 Class 구성멤버로 선언되어야 한다.
> 우리가 자주 쓰는 main() 메소드도 클래스멤버다. 어플리케이션을 실행할 때 JVM이 알아서 static main()를 찾아서 실행하는 것 뿐이다.

```
람다의 기본식 : (매개변수) -> {실행문}
```
람다식을 통해서 생성되는 것은 메소드 자체가 아니다. 람다식을 통해서 생성되는 것은 실행문(메소드)을 가진 객체이다.람다식이 생성하는 객체는 일반적인 객체가 아닌 인터페이스를 구현한 **익명구현객체**다.

#### interface와 익명클래스
[상단의 예제코드](#1)의 Calculator를 interface로 바꾸어 설명하겠다.
{% highlight java %}
interface Calculator {
  int calc(int n);
}
{% endhighlight %}

`Calculator`라는 인터페이스는 `calc()`라는 추상메소드를 가지고 있다. `Calculator`를 객체화하는 방법은 두 가지이다.

- **implements한 class**를 생성
- **익명 클래스**로 생성

인터페이스는 인터페이스 자체로 구현체(객체)를 만들 수 없지만, **인터페이스의 추상 메소드를 생성과 동시에 {}으로 감싼 곳에 @Override 하여** 구현할 수 있다. 하단은 `calc()` 메소드를 가진 `Calculator` 인터페이스를 **익명클래스** 방식으로 인스턴스화한 것이다.

{% highlight java %}
class Driver {
  public static void main(String[] args) {
    int n = 2;
    // 생성과 동시에 구현할 메소드를 override 함
    Calculator cal = new Calculator() {
      @Override
      int calc(int n) {
        return n + 1;
      }
    }

    System.out.println(cal.calc(n)); // 3
  }
}
{% endhighlight %}

#### 1. 람다의 기본식
람다식에서 생성되는 *'익명구현객체는 기반이 되는 interface의 타입을 갖는다.'* 이것을 **타겟타입**이라 한다. 우리가 람다식으로 바꿀 메소드는 `calc()`이므로 람다식의 타겟타입은 `Calculator`이다.

##### Calculator의 calc()를 람다의 기본식으로 구현
{% highlight java %}
class Driver {
  public static void main(String[] args) {
    int n = 2;
    // (매개변수) -> {구현로직}
    Calculator cal = (int n) -> {return n + 1;};

    System.out.println(cal.calc(n)); // 3
  }
}
{% endhighlight %}

##### 매개변수의 타입을 자동으로 인식하기 때문에 변수 타입을 삭제할 수 있다.
{% highlight java %}
class Driver {
  public static void main(String[] args) {
    int n = 2;
    // (매개변수) -> {구현로직}
    Calculator cal = (n) -> {return n + 1;};

    System.out.println(cal.calc(n)); // 3
  }
}
{% endhighlight %}

##### 매개변수가 하나 일 때 ()를 생략할 수 있다. 두 개이상 혹은 없을 때는 ()가 필요함
{% highlight java %}
class Driver {
  public static void main(String[] args) {
    int n = 2;
    // (매개변수) -> {구현로직}
    Calculator cal = n -> {return n + 1;};
    // 매개변수가 없는 경우 무조건 ()가 필요함. () -> {return "hi";}

    System.out.println(cal.calc(n)); // 3
  }
}
{% endhighlight %}

##### 로직이 한 줄안에 끝나는 경우 `{}`과 `return`을 제거할 수 있다.
{% highlight java %}
class Driver {
  public static void main(String[] args) {
    int n = 2;
    Calculator cal = n -> n + 1; // 간단하게 변경

    System.out.println(cal.calc(n)); // 3
  }
}
{% endhighlight %}

#### 2. 타겟 타입
###### [JAVA 표준 함수적 인터페이스 API]
![Screenshot](http://oracle.moazine.com/images_sabo/Oracle/39/img15_2.jpg)

컴파일러는 람다식을 해석하여 자동으로 **익명구현객체**로 만든다. 이 때 람다식의 타겟 타입이 될 인터페이스는 *2개 이상의 추상 메소드를 가지면 안된다.* 그렇게 되면 컴파일러가 해당 람다식이 타겟 타입의 어떤 메소드를 구현한 것인지 알 수 없기 때문이다.

`@FuntionalInterface`는 이것을 명시적으로 선언하고 강제하는 어노테이션이다. 이런식으로 선언된 인터페이스를 람다식의 **함수적 인터페이스** 라 한다.

##### @FuntionalInterface 함수적 인터페이스 선언
{% highlight java %}
@FuntionalInterface // 어노테이션을 붙히고 2개 이상의 추상메소드를 만들면 컴파일 에러가 발생한다.
interface Calculator {
  int calc(int n);

  int sum(int a, int b); // 에러발생
}
{% endhighlight %}


##### 람다식의 매개변수는 final 키워드가 붙지 않더라도 불변하는 상수로 취급.
{% highlight java %}
class Driver {
  public static void main(String[] args) {
    int n = 2;
    Calculator cal = n -> ++n; // 매개변수를 변경시키려 하면 에러발생

    System.out.println(cal.calc(n)); // 3
  }
}
{% endhighlight %}


#### 3. 활용예시
자바에서 변수의 역할을 할 수 있는 것은 **Primitive 타입(int, long, boolean, 등), Object 타입(Object를 상속받는 모든 것)이다.** 람다식을 만들 수 있는 **타겟타입**도 변수가 될 수 있으므로, 람다식과 같이 활용하면 메소드도 매개변수처럼 사용할 수 있다.(정확히는 메소드를 구현한 함수적 인터페이스를 변수로 사용하는 것)

[예제](#1)의 조건을 살짝 변형시켜보자.
```
함수적 인터페이스 'Calculator' 와 정수 'n'을 매개변수로 받아 'calc()''의
실행 결과를 출력해주는 'printCalc()''라는 메소드가 있다.

'calc()'의 결과는 n + 1일 수도, n - 1, n * 2일 수도 있다.
```

- **printCalc()**
{% highlight java %}
public static void printCalc(int n, Calculator cal) {
  System.out.println(cal.calc(n));
}
{% endhighlight %}

우리는 `calc()`와 `printCalc`를 변경시키지 않고 매개변수에 어떤 람다식을 넣어주느냐에 따라 리턴되는 값들을 다르게 할 수 있다.
- **Calculator**
{% highlight java %}
@FuntionalInterface
interface Calculator {
  int calc(int n);
}
{% endhighlight %}

- **Driver**
{% highlight java %}
class Driver {
  public static void main(String[] args) {
    int n = 2;

    // int n과 구별하기 위해 람다식에는 v라는 변수명을 사용.
    printCalc(n, v -> v + 1); // n + 1, 출력값 3
    printCalc(n, v -> v - 1); // n - 1, 출력값 1
    printCalc(n, v -> v * 2); // n * 2, 출력값 4
  }

  public static void printCalc(int n, Calculator cal) {
    System.out.println(cal.calc(n));
  }
}
{% endhighlight %}

---

### <a id="2"></a>Stream 예제
String 타입의 값을 Integer로 변환하기 위해서는 ```Integer.parseInt()``` 메소드를 사용해야 한다. 하단의 예제의 최종 결과값은 문자열 List에 있는 값을 더한 것이다. 때문에 List의 String값을 Integer 값으로 모두 변환하여 더해주어야한다.

**Parser** class의 ```strToIntList()```메소드는 `for loop`를 돌며 ```parseInt()```를 해주고 변환된 값들을 다시 ```List<Integer>```에 담아 반환해 주는 역할을 한다.

- **Driver**
{% highlight java %}
class Driver {
  public static void main(String[] args) {
    // 문자열 List
    List<String> strList = Arrays.asList("1,2,3,4,5,6".split(","));
    // 문자열 List를 Integer List로 변환
    List<Integer> intList = Parser.strToIntList(strList);

    int sum = 0; // 총 합

    // 덧셈
    for (Integer value : intList) {
      sum += value;
    }
    System.out.println(sum); // 21
  }
}
{% endhighlight %}


- **Parser**
{% highlight java %}
class Parser {
    public static List<Integer> strToIntList(List<String> strList) {
        List<Integer> intList = new ArrayList<>();

        for (String value : strList) {
            intList.add(Integer.parseInt(value));
        }
        return intList;
    }
}
{% endhighlight %}

## Stream

---
