---
title: "Checked/Unchecked Exception"
layout: post
date: 2018-10-23
image:
headerImage: false
tag:
- Java
- Exception
category: blog
author: sehunkim
description: Java Exception의 종류
star: false # true로할 경우 제목에 highlight 처리가 됨
sitemap :
  changefreq : daily
  priority : 1.0
---


## Description:

자바 프로그래밍을 하다보면 예외를 처리하지 않아도 되는 부분과, 꼭 예외처리를 해주어야 하는 부분이 있다. 또, 의도적으로 예외를 발생시켜야하는 상황이 있는데. 어떤 것이 다르고 어떻게 처리해야 하는지 정리해보겠다.

> 참고 : [예외처리에 대한 짧은 생각](http://www.nextree.co.kr/p3239/)

## 에러와 예외:
`Error`는 보통 시스템에 비정상적인 상황이 발생하였을 때를 칭함. 모든 비정상적인 상황을 에러라 칭하지만, 자바에서는 어플리케이션 레벨에서 개발자가 처리할 수 없는 부분을 말한다.
```
ex) VirtualMachineError, StackOverFlowError
```

`Exception`은 개발자가 구현한 로직안에서 발생한다. 때문에 예외는 발생을 예측할 수 있다. ***예외는 구분하고 처리해야한다.***
```
ex) NullPointException, IllegalArgumentException
```
## 예외의 상속구조
![Screenshot](http://www.ntu.edu.sg/home/ehchua/programming/java/images/Exception_Classes.png)

최상위에 모든클래스가 상속받는 Object 클래스가 있고, 모든 예외가 상속을 받는 Throwable 클래스가 있다. 그 자식으로 `Error`와 `Exception`으로 자식이 분기된다.

예외도 `Unchecked Exception`과 `Checked Exception`으로 나뉜다. 나뉘는 기준은 `RuntimeException`을 상속받는 Exception과 아닌 Exception으로 나뉜다.

#### 트랜잭션(Transaction)

예외의 종류에대해 설명하기 앞서 트랜젝션에 대해 알아야한다. `Transaction`이란 하나의 작업단위를 뜻하며 DB를 공부하다보면 자주 접하게 될 단어이다.

은행에서 계좌에서 현금을 인출하는 과정을 예로 이해할 수 있다.
```
[예금인출 트랜잭션]
1. 예금 잔액을 확인한다.
2. 해당 인출금에서 잔액을 뺀다.
3. 인출금을 뺀 나머지를 다시 예금잔액으로 저장한다.
4. 인출금이 지급된다.
```
하나의 트랜잭션이 일련의 개별작업으로 구성되어있는데 만약 이중에 한 작업이라도 실패한다면. 처음으로 돌아가(Roll-Back)야 할 것이다. Roll-Back이란 트랜잭션이 진행되었다가 문제상황이 발생하였을 때 그 상황이 저장되는 것이 아니라 트랜잭션을 시작하는 시점으로 돌아가는 것이다.
> ex) 잔액보다 많은 금액을 인출하려 하거나, 인출금을 뺀 나머지 잔액이 저장되지 않거나.

트랜잭션상 문제가 생겼을 때 예외마다 처리되는 방식이 다르다.

## Checked Exception, Unchecked Exception
![Screenshot](https://t1.daumcdn.net/cfile/tistory/266F083552E5B45E2D)

- Unchecked Exception

`Unchecked Exception`은 명시적인 처리가 필요없는 예외이다. `RuntimeException`을 상속받는 예외들이 포함된다. **Runtime** 즉, 예외가 발생하는 시점이 컴파일이 될 때가 아닌 어플리케이션 실행 중이다.

트랜잭션상에서 `Unchecked Exception`이 발생할 경우 Roll-Back된다.

- Checked Exception

`Checked Exception`은 컴파일하는 시점에 발생하는 예외이다. 때문에 무조건 예외가 발생할 수 있는 코드를 컴파일 하기전에 try catch로 감싸주어야 어플리케이션을 실행할 수 있다.

트랜잭션상에서 `Checked Exception`이 발생할 경우 Roll-Back하지 않고 바로 예외를 던진다. 때문에 별도의 처리를해주어야 한다.


## 예외의 처리방식
예외가 발생하였을 때 처리하는 방법은 보통 세 가지이다.

- **직접처리** : 예외가 발생하는 부분을 try 블럭으로 감싸 catch에서 예외처리를 하는 것

{% highlight java %}
public static void main(String[] args) {
  int[] arr = {1, 2, 3, 4, 5}; // length 5

  try {
    System.out.println(arr[5]); // IndexOutOfBoundException 발생
  } catch(IndexOutOfBoundException e) {
    System.out.println("잘못된 인덱스를 입력하셨습니다.")
  }  
}
{% endhighlight %}

- **예외던지기** : `throws` 키워드를 사용하여 예외가 발생한 부분이 아닌 예외가 발생하는 로직을 호출한 부분으로 예외를 넘긴다.

{% highlight java %}
public static void main(String[] args) {
  int[] arr = {1, 2, 3, 4, 5}; // length 5

  try {
    printNum(arr, 5) // 해당 메소드에서 예외가 발생하면 throws로 예외를 넘겨 받는다.
  } catch(IndexOutOfBoundException e) {
    System.out.println("잘못된 인덱스를 입력하셨습니다.")
  }
}

public static void printNum(int[] arr, int idx) throws IndexOutOfBoundException {
  System.out.println(arr[idx]); // IndexOutOfBoundException 발생
}
{% endhighlight %}


- **예외전환**

예외가 발생하였을 때 `의미있는 이름`으로 바꾸는 경우와 `Checked Exception을 Unchecked Exception으로 바꾸어` 쓰고싶을때 사용한다. Checked Exception의 경우 무조건 try catch 문을 사용해서 처리해주어야 한다. 때문에 예외를 다른 곳으로 던지고 싶을때 RuntimeException으로 변환하여 던져우어야한다. 두 경우 모두 `throw` 키워드로 예외를 발생시킨다.

{% highlight java %}
// RuntimeException을 상속받아 LongIndexException을 새로 정의해줌
class LongIndexException extends RuntimeException {

  LongIndexException() {
    super();
  }

  LongIndexException(String message) {
    super(message);
  }

}

class Main {

  public static void main(String[] args) {
    int[] arr = {1, 2, 3, 4, 5}; // length 5

    try {
      printNum(arr, 5) // 해당 메소드에서 예외가 발생하면 throws로 예외를 넘겨 받는다.
    } catch(IndexOutOfBoundException e) {
      throw new LongIndexException("잘 못된 인덱스를 입력하셧습니다.");
      // 예를 들어 잘못된 인덱스를 입력하였다는 것을 명시적으로 표시하기 위해
      // LongIndexException class를 직접 만들어 사용할 수 있다.
    }
  }

  public static void printNum(int[] arr, int idx) throws IndexOutOfBoundException {
    System.out.println(arr[idx]); // IndexOutOfBoundException 발생
  }

}
{% endhighlight %}
