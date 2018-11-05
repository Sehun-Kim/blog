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
>
### index
1. [Lambda](#1)
2. [Stream](#4)

---

### <a id="1"></a>Lambda 예제
`Calculator`클래스는 `calc()`라는 메소드를 가지고 있다. `calc()`는 매개변수 `n`을 받아 1을 증가시켜 리턴한다.
- **Calculator.class**
{% highlight java %}
class Calculator {
  Calculator() {};

  public int calc(int n) {
    return n + 1;
  };
}
{% endhighlight %}

- **Driver**
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
JAVA의 **Lambda**는 메소드를 하나의 식(Expression)으로 표현한 것이다. **익명메소드(함수)** 생성 문법이라 할 수 있다. 메소드를 지칭하는 명칭(메소드명)없이 구현부만으로 선언하는 것이다. 하지만 JAVA의 메소드는 **메소드 자체로 혼자 선언하여 쓰일 수 없다.** 무조건 Class 구성멤버로 선언되어야 한다.
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


#### 3. 메소드 레퍼런스
Method Reference(참조)는 **이미 구현되어있는 메소드를 참조**해 매개변수의 정보와 리턴 타입을 알아내, 불필요한 매개변수를 제거하는 것이 목적이다.

예를 들어 정수 값 두개를 받아 둘 중 큰 값을 리턴해주는 메소드를 람다식으로 표현하려한다.

##### <a id="2"></a>코드 1
{% highlight java %}
IntBinaryOperator op = (int x, int y) -> {
  if(x > y) return x;
  return y;
};
System.out.println(op.applyAsInt(3, 7)); // 7
{% endhighlight %}

###### [JAVA 표준 함수적 인터페이스 API]
![Screenshot](http://oracle.moazine.com/images_sabo/Oracle/39/img15_2.jpg)
> 여러 표준 함수적 인터페이스가 있으니 직접 찾아보며 써보길 바란다.

앞서 설명한 **함수적 인터페이스**를 개발자가 직접 만들어 사용할 수도 있지만, 자바에서 제공하는 표준 인터페이스들 사용해 람다식을 구현할 수 있다. [상단의 코드](#2)에 있는 `IntBinaryOperator`는 `BinaryOperator` 종류의 표준 **함수적 인터페이스** 이다.

`IntBinaryOperator`는 두 개의 정수를 매개변수로 받아 정수값을 리턴해주는 추상메소드 `applyAsInt()`를 갖고있다. 람다식은 두 개의 정수를 매개변수로 받아 둘 중 큰 값을 반환해준다. 기존에 `Math` 클래스의 `max()`를 사용해도 같은 결과를 얻을 수 있다.

##### 코드 2
{% highlight java %}
IntBinaryOperator op1 = (int x, int y) -> {return Math.max(x, y);};
// 앞서 배운 것을 모두 적용할 경우
IntBinaryOperator op2 = (x, y) -> Math.max(x, y);

System.out.println(op2.applyAsInt(3, 7)); // 7
{% endhighlight %}

람다식은 이미 구현되어 있는 `Math.max()`를 참조하여 사용하고 있다. 이럴 때 굳이 매개변수 (x,y)를 명시적으로 표현하지 않고 사용할 수 있다.
##### <a id="3"></a>코드 3
{% highlight java %}
IntBinaryOperator op = Math::max;

System.out.println(op.applyAsInt(3, 7)); // 7
{% endhighlight %}

**3가지 메소드 레퍼런스**
--
##### **1. 정적메소드, 인스턴스메소드 참조**

[코드 3](#3)의 `max()`가 `Math` 클래스의 정적메소드이기에 정적(static)메소드 참조방식이 된다. 만약 인스턴스 메소드일 경우 인스턴스를 생성한 후에 `인스턴스::메소드명`으로 사용할 수 있다.

##### 코드 4
{% highlight java %}
class MyMath {
  public static int myMax(int x, int y) { return x > y ? x : y; }
}

class Driver {
  public static void main(String[] args) {
    MyMath myMath = new MyMath();
    IntBinaryOperator op = myMath::myMax;

    System.out.println(op.applyAsInt(3, 7)); // 7
  }
}
{% endhighlight %}

##### **2. 매개변수의 메소드 참조 방식**

이번에는 매개변수로 받은 인자의 메소드를 참조하는 방식이다. `Integer` 클래스에 있는 `compareTo()`를 사용해 보겠다. `compareTo()`는 매개변수로 들어온 인자가 호출한 인자보다 값이 큰 경우 -1, 같을 경우 0, 작을 경우 1을 리턴한다.

`ToIntBiFunction`은 `Function` 인터페이스 중 하나로써. 두 개의 매개변수를 받아 람다식의 로직을 사용하여 `applyAsInt()`를 사용했을 때 int 타입을 반환해주는 **함수적 인터페이스**이다.

##### 코드 5
{% highlight java %}
class Driver {
  public static void main(String[] args) {
    Integer x = 3;
    Integer y = 7;
    ToIntBiFunction<Integer, Integer> func = Integer::compareTo; // (x, y) -> x.compareTo(y);

    System.out.println(func.applyAsInt(x, y)); // -1
  }
}
{% endhighlight %}

##### **3. 생성자 참조 방식**
생성자 또한 일종의 메소드이기 때문에 메소드 레퍼런스로 쓸 수 있다.

##### 코드 6
{% highlight java %}
class Person {
  private String name;
  private Integer age;

  public Person(String name) {
    this.name = name;
    this.age = 0;
  }

  public Person(String name, Integer age) {
    this.name = name;
    this.age = age;
  }

  @Override
  public String toString() {
    return "[" + this.name + " : " + this.age + "]";
  }
}

class Driver {
  public static void main(String[] args) {
    Function<String, Person> func1 = Person::new;
    Function<String, Integer, Person> func2 = Person::new; // 넘어오는 매개변수의 갯수로 어떤 생성자를 호출할 지 찾아줌

    System.out.println(func1.apply("Dom")); // [Dom : 0]
    System.out.println(func2.apply("Dom", 28)); // [Dom : 28]
  }
}
{% endhighlight %}


#### 4. 활용예시
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

### <a id="4"></a>Stream 예제
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
JAVA의 **Stream**은 **컬렉션(Collection)의 요소를 하나씩 참조하여 람다식으로 처리할 수 있게 해주는 일종의 반복자**이다.
> 헷갈리지 말아야 할게 있는데 I/O(입출력)관련 `Input/OutputStream`과는 관련이 없다.(InputStream은 java.io 패키지, Stream은 java.util 패키지이다.)

#### JAVA의 Collection과 Iterator
![Screenshot](https://www.javatpoint.com/images/java-collection-hierarchy.png)
자바의 `Collection` 인터페이스는 `Set`, `List`, `Queue` 인터페이스 처럼 데이터를 저장하는 자료구조들의 상위에 있는 인터페이스이다. 최상위에는 `iterator()`메소드를 갖고있는 `Iterable` 인터페이스가 있다.

`iterator()`는 `Iterator<T>`를 반환한다. `Iterater`는 자료안에 자료가 있는지 없는지 확인해주는 `hasNext()` 메소드와, 자료구조에 저장되어 있는 자료를 하나씩 리턴해주는 `next()` 메소드를 갖고 있다. 즉, `Collection`을 implements한 모든 자료구조들은 `iterator()`로 반복자를 만들어 반복문을 돌릴 수 있다는 것이다.

{% highlight java %}
class Driver {
    public static void main(String[] args) {
      List<Integer> list = Arrays.asList(1,2,3,4,5,6);
      Iterator<Integer> iter = list.iterator();

      // 순서대로 1 2 3 4 5 6 출력
      while (iter.hasNext()) {
        System.out.println(iter.next());
      }
    }
}
{% endhighlight %}

다시 스트림으로 돌아와. 상단의 `Iterator`를 `Stream`으로 변형해보겠다.

{% highlight java %}
class Driver {
    public static void main(String[] args) {
      List<Integer> list = Arrays.asList(1,2,3,4,5,6);
      // Integer 데이터를 갖는 Stream 생성
      Stream<Integer> stream = list.stream();

      // forEach()를 사용해서 1 2 3 4 5 6 출력
      stream.forEach((Integer i) -> { System.out.println(i); });
      /*
      * list.stream().forEach(System.out::println);
      * 메소드 레퍼런스를 사용하면 코드를 더 줄일 수 있음
      */
    }
}
{% endhighlight %}

여기서 스트림의 첫 특징이 나온다. `forEach()`의 매개변수로 **람다식** 을 사용할 수 있다는 것이다.

#### 1. 스트림의 특징




---
## 참고
> - [람다식 - 기본 문법, 타겟 타입과 함수적 인터페이스](http://palpit.tistory.com/671)
> - [JAVA8 메소드 레퍼런스](https://imcts.github.io/java-method-reference/)
> - [Java Study Group](https://rebeccacho.gitbooks.io/java-study-group/content/chapter14.html)
> - [JAVA 8 스트림 튜토리얼](https://wraithkim.wordpress.com/2017/04/13/java-8-%EC%8A%A4%ED%8A%B8%EB%A6%BC-%ED%8A%9C%ED%86%A0%EB%A6%AC%EC%96%BC/)
> - [Java8 Stream은 loop가 아니다.](https://www.popit.kr/java8-stream%EC%9D%80-loop%EA%B0%80-%EC%95%84%EB%8B%88%EB%8B%A4/)
> -  [Java 8과 함수형 프로그래밍](https://medium.com/@goinhacker/java-8%EA%B3%BC-%ED%95%A8%EC%88%98%ED%98%95-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D-154e6d8830f1)
> - [JAVA 스트림 API](http://iloveulhj.github.io/posts/java/java-stream-api.html)
