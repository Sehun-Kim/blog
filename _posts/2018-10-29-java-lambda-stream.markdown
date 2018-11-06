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
description: Java 8부터 추가 된 Stream과 Lambda에 대해 알아보자.
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

### 1. 람다의 기본식
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

### 2. 타겟 타입
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


### 3. 메소드 레퍼런스
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

###### <a id="6"></a>[JAVA 표준 함수적 인터페이스 API]
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

#### 3가지 메소드 레퍼런스

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


### 4. 활용예시
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

### 1. 스트림의 특징
스트림은 컬랙션의 반복자와 다른 특징을 몇 가지 갖는다. [예제 코드](#4)를 변경 시켜가며 설명하겠다.
일단 반복자가 있는 부분을 스트림으로 바꾸어보자.

<a id="5"></a>
- **Driver**
{% highlight java %}
class Driver {
    public static void main(String[] args) {
        // 문자열 List
        List<String> strList = Arrays.asList("1,2,3,4,5,6".split(","));
        // 문자열 List를 Integer List로 변환
        List<Integer> intList = Parser.strToIntList(strList);

        // 총 합
        int sum = intList.stream().mapToInt(Integer::intValue).sum();
        System.out.println(sum); // 21
    }
}
{% endhighlight %}


- **Parser**
{% highlight java %}
class Parser {
    public static List<Integer> strToIntList(List<String> strList) {
        return strList.stream()
                .map(Integer::parseInt)
                .collect(Collectors.toList());
    }
}

{% endhighlight %}

#### 람다식을 사용할 수 있다.
`Stream` 객체의 메소드들은 모두 람다식을 매개변수로 가질 수 있다. 람다식에서 배웠던 [**표준 함수적 인터페이스**](#6)가 모두 포함된다. Stream의 메소드마다 각기 다른 함수적 인터페이스를 쓸 수 있으니 찾아서 적용해 보자.

이제 반복자를 모두 스트림으로 변경한 [코드](#5)를 보자

{% highlight java %}
strList.stream().map(Integer::parseInt).collect(Collectors.toList());
{% endhighlight %}
문자열 `List`를 정수형 `List`로 변환해주는 `strToIntList()`의 메소드가 한 줄로 줄었다.

1. 컬랙션은 `.stream()`를 사용하여 `Stream`타입의 객체로 바꿔줄 수 있다.
2. `map()`은 `Function<T, R>` 함수적 인터페이스(람다식)를 매개변수로 받아 `Stream`의 데이터를 하나씩 람다식으로 처리해서 다시 `Stream`객체에 담는다.
3. `collect()`는 스트림의 데이터를 모아 새로운 객체를 만들어 리턴한다. 상단의 코드에선 `Collectors.toList()`를 사용해서 `List` 객체를 만들어 리턴한다.

![Screenshot](https://t1.daumcdn.net/cfile/tistory/2462BC3658DC5B7603)
`Stream`은 객체 스트림(`Stream<T>`) 이 외에도 기본타입에 특화된 `IntStream`, `LongStream`, `DoubleStream` 등과 같은 **기본타입스트림** 라이브러리를 갖고있다.

{% highlight java %}
int sum = intList.stream().mapToInt(Integer::intValue).sum();
{% endhighlight %}

우린 `intList`에 있는 값의 총 합을 얻고 싶다. `IntStream`에 스트림의 모든 값을 더 해주는 `sum()`가 있기 때문에 `intList`를 `IntStream`로 바꾸어 사용해야한다.

1. `IntStream.of()`로 직접 생성과 초기화하여 `IntStream`을 만드는 방법도 있지만, `mapToInt()`로 기본 스트림을 `IntStream`으로 바꿔줄 수 있다.
> Stream.of("1","2","3","4","5","6").stream.`mapToInt(Integer::parseInt)`.sum(); 방식으로 String 스트림을 IntStream으로 바꿀 수 있다.`

2. **단말연산** `sum()`으로 `IntStream`의 모든 데이터를 더한 값을 얻는다.

#### 중간/단말연산을 갖고있다.
코드를 다시 리팩토링하면 한 줄로 바꿀 수 있다.
- **Driver**
{% highlight java %}
class Driver {
    public static void main(String[] args) {
        // 문자열 List
        List<String> strList = Arrays.asList("1,2,3,4,5,6".split(","));

        // 총 합
        int sum = strList.stream().mapToInt(Integer::parseInt).sum();
        System.out.println(sum); // 21
    }
}
{% endhighlight %}

스트림은 계속 스트림을 반환하며 연산을 이어서 할 수있게 하는 **중간연산**과 스트림을 종료시키고 결과를 반환하는 **단말연산** , 두 가지 종류의 메소드들을 갖고있다.

- **중간연산**

![Screenshot]({{ site.url }}/assets/images/stream_1.jpg)
중간연산자들은 `Stream`객체를 다시 가공해서 `Stream` 객체로 만드는 연산을 한다.
{% highlight java %}
List<String> strList = Arrays.asList("3", "1", "4", "2", "5", "5");

strList.stream() // 문자열 스트림 생성
  .map(Integer::parseInt) // 문자열 스트림을 정수형 스트림으로 변환
  .sorted() // 정렬
  .distinct() // 중복제거
  .limit(3) // 갯수를 3개로 제한
  .collect(Collectors.toList()); // 리스트로 변환 => {1, 2, 3}
{% endhighlight %}
중간 연산자들은 연속해서 사용할 수 있고, 중간연산 이후엔 다른 스트림이 반환 된다.  원본인 `strList` **Collection의 값들은 바뀌지 않는다.**

- **단말연산**

![Screenshot]({{ site.url }}/assets/images/stream_2.jpg)
단말연산자는 스트림을 받아 결과값을 만들고 더 이상 스트림을 반환하지 않는다.(스트림을 닫는다) 만약 연산을 계속 이어하고 싶다면 스트림을 다시 만들어 작업을 이어가야 한다.

{% highlight java %}
Stream.of("3", "1", "4", "2", "5", "5") // 문자열 스트림 생성
        .map(Integer::parseInt) // 문자열 스트림을 정수형 스트림으로 변환
        .sorted() // 정렬
        .distinct() // 중복제거
        .limit(3) // 갯수를 3개로 제한
        .collect(Collectors.toList()) // 리스트로 변환 => {1, 2, 3}
        .stream() // 다시 정수형 값을 갖는 스트림으로 변환
        .filter(x -> x > 1) // 1보다 큰 값만 갖도록 필터링함 {2, 3}
        .forEach(System.out::println); // 2와 3만 출력됨
{% endhighlight %}

- **연산의 순서**

스트림은 **지연된(lazy)** 연산을 한다. 단말연산이 없으면 연산을 실행하지 않고, 단말연산이 수행되기 전에 중간연산이 실행되지 않는다.
> 결과가 필요하기 전까지 실행되지 않는다는 뜻이다.

{% highlight java %}
Stream.of("3", "1", "4", "2", "5", "5")
        .map(x -> {
            System.out.println("map : " + x);
            return Integer.parseInt(x);
        })
        .filter(x -> {
            System.out.println("filter : " + x);
            return x > 1;
        });
{% endhighlight %}
이 코드를 실행하면 **단말연산**이 없기 때문에 아무 것도 출력되지 않는다.

{% highlight java %}
Stream.of("3", "1", "4", "2", "5", "5")
        .map(x -> {
            System.out.println("map : " + x);
            return Integer.parseInt(x);
        })
        .filter(x -> {
            System.out.println("filter : " + x);
            return x > 1;
        })
        .forEach(x -> {
            System.out.println("forEach : " + x);
        });
{% endhighlight %}
이 코드를 실행하면 예상한 결과와 달라 놀라울 것이다. 우리는 보통 스트림이 해당 연산을 처리한 후에 전체 스트림을 넘겨 다시 작업할 것이라 생각한다.
> map에서 전부 출력하고, filter에서 전부출력하고, forEach에서 filter에서 걸러진 나머지 값들을 출력할 거라고 생각할 것이다.

```
map : 3
filter : 3
forEach : 3
map : 1
filter : 1
map : 4
filter : 4
forEach : 4
map : 2
filter : 2
forEach : 2
map : 5
filter : 5
forEach : 5
map : 5
filter : 5
forEach : 5
```
하지만 스트림의 요소들이 개별적으로 **중간연산**들을 통과해서 **단말연산**에 도달하는 순으로 진행이 된다. 때문에 연산의 순서를 바꿈으로 연산이 실행되는 횟수를 줄일 수 있을 것이다.

### 2. 병렬처리
스트림은 `parallel()`을 활용해 연산을 병렬로 처리할 수 있다.

{% highlight java %}
Stream.of("1", "2", "3", "4", "5", "5")
        .filter(x -> {
            System.out.println("filter : " + x);
            return !x.equals("5");
        })
        .mapToInt(x -> {
            System.out.println("map : " + x);
            return Integer.parseInt(x);
        })
        .forEach(x -> {
            System.out.println("forEach : " + x);
        });
{% endhighlight %}
상단의 코드를 실행하면 결과값은 앞서 배운 연산순서대로 각 인자가 순서대로 연산들을 통과한다.
```
filter : 1
map : 1
forEach : 1
filter : 2
map : 2
forEach : 2
filter : 3
map : 3
forEach : 3
filter : 4
map : 4
forEach : 4
filter : 5
filter : 5
```

{% highlight java %}
Stream.of("1", "2", "3", "4", "5", "5")
        .parallel() // 병렬스트림으로 변경
        .filter(x -> {
            System.out.println("filter : " + x);
            return !x.equals("5");
        })
        .mapToInt(x -> {
            System.out.println("map : " + x);
            return Integer.parseInt(x);
        })
        .forEach(x -> {
            System.out.println("forEach : " + x);
        });
{% endhighlight %}

`parallel()`로 스트림을 병렬스트림으로 바꾼 뒤 연산들을 수행하면 연산은 순서없이 병렬로 처리된다.
```
filter : 2
map : 2
forEach : 2
filter : 5
filter : 5
filter : 4
filter : 1
map : 1
filter : 3
map : 3
forEach : 1
map : 4
forEach : 3
forEach : 4
```
스트림안에 데이터가 매우 많을 때, 병렬스트림과 연산의 순서를 활용하여 더 빠르게 연산할 수 있을 것이다.


---
## 참고
> - [람다식 - 기본 문법, 타겟 타입과 함수적 인터페이스](http://palpit.tistory.com/671)
> - [JAVA8 메소드 레퍼런스](https://imcts.github.io/java-method-reference/)
> - [Java Study Group](https://rebeccacho.gitbooks.io/java-study-group/content/chapter14.html)
> - [JAVA 8 스트림 튜토리얼](https://wraithkim.wordpress.com/2017/04/13/java-8-%EC%8A%A4%ED%8A%B8%EB%A6%BC-%ED%8A%9C%ED%86%A0%EB%A6%AC%EC%96%BC/)
> - [스트림 연산자정리](http://demoversion.tistory.com/27)
> - [Java8 Stream은 loop가 아니다.](https://www.popit.kr/java8-stream%EC%9D%80-loop%EA%B0%80-%EC%95%84%EB%8B%88%EB%8B%A4/)
> -  [Java 8과 함수형 프로그래밍](https://medium.com/@goinhacker/java-8%EA%B3%BC-%ED%95%A8%EC%88%98%ED%98%95-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D-154e6d8830f1)
> - [JAVA 스트림 API](http://iloveulhj.github.io/posts/java/java-stream-api.html)
