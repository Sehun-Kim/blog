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
Java 8부터 추가된 **Stream** API와 **Lambda** API가 어떻게 쓰이는지 간단하게 정리해보겠다.
> 참고자료 :
> - [람다식 - 기본 문법, 타겟 타입과 함수적 인터페이스](http://palpit.tistory.com/671)
> - [Java Study Group](https://rebeccacho.gitbooks.io/java-study-group/content/chapter14.html)
> - [JAVA 8 스트림 튜토리얼](https://wraithkim.wordpress.com/2017/04/13/java-8-%EC%8A%A4%ED%8A%B8%EB%A6%BC-%ED%8A%9C%ED%86%A0%EB%A6%AC%EC%96%BC/)
> - [Java8 Stream은 loop가 아니다.](https://www.popit.kr/java8-stream%EC%9D%80-loop%EA%B0%80-%EC%95%84%EB%8B%88%EB%8B%A4/)
> - [람다가 이끌어 갈 모던 Java](https://d2.naver.com/helloworld/4911107)
> -  [Java 8과 함수형 프로그래밍](https://medium.com/@goinhacker/java-8%EA%B3%BC-%ED%95%A8%EC%88%98%ED%98%95-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D-154e6d8830f1)
> - [JAVA 스트림 API](http://iloveulhj.github.io/posts/java/java-stream-api.html)

---

### [문자열 List를 Integer List로 변환하여 List의 값들을 모두 더 하는 예제]
기본적으로 String 타입의 값을 Integer로 변환하기 위해서는 ```Integer.parseInt()``` 메소드를 사용해야 한다. 이번 예제에서는 하나의 값이 아닌 strList에 있는 모든 String값을 Integer 값으로 변환해주어야한다.

**Parser** class의 ```strToIntList()```메소드는 `for loop`를 돌며 ```parseInt()```를 해주고 변환된 값들을 다시 ```List<Integer>```에 담아 반환해 주는 역할을 한다.

- **Driver**
{% highlight java %}
class Driver {
  public static void main(String[] args) {
    List<String> strList = Arrays.asList("1,2,3,4,5,6".split(",")); // 문자열 List
    List<Integer> intList = Parser.strToIntList(strList); // 문자열 List를 Integer List로 변환

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
---
### Lambda
Lambda란 메소드를 하나의 식으로 표현한 것이다. 일종의 **익명함수** 생성 문법이다. 메소드를 명칭없이 구현부만으로 사용하는 것이다. 텍스트만으로는 이해가 잘 안되기 때문에 상단의 예제 코드를 리팩토링하며 설명하겠다.

#### 1. 메소드 추상화
기본식을 활용하여 구현하기 전에 람다식이 어떤 타입을 리턴하는지 알아야 한다.
```
람다의 기본식 : (매개변수) -> {함수로직}
```
JAVA의 메소드는 **메소드 자체로 혼자 선언하여 쓰일 수 없다.** 무조건 Class 구성멤버로 선언되어야 한다.
> 우리가 자주 쓰는 main() 메소드도 클래스멤버다. 어플리케이션을 실행할 때 JVM 알아서 static main()를 찾아서 실행하는 것 뿐이다.

때문에,


- 해당 메소드를 람다의 기본식으로 변경해 보겠다.


- 해당 코드에서 매개변수가 ```strList```하나 일 때 ()를 생략할 수 있다. 두 개이상일 때는 ()필요함


- lambda로 구현한 로직안에도 lambda식으로 변경할 수 있는 부분이 있다.


- 로직이 한 줄안에 끝나는 경우 ```{}```과 ```return```을 제거할 수 있다.
