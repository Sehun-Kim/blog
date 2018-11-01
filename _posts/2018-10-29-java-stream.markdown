---
title: "JAVA Stream & Lambda"
layout: post
date: 2018-10-29
image:
headerImage: false
tag:
- Java
- Stream
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
> 참고
> https://rebeccacho.gitbooks.io/java-study-group/content/chapter14.html
> https://wraithkim.wordpress.com/2017/04/13/java-8-%EC%8A%A4%ED%8A%B8%EB%A6%BC-%ED%8A%9C%ED%86%A0%EB%A6%AC%EC%96%BC/
> https://www.popit.kr/java8-stream%EC%9D%80-loop%EA%B0%80-%EC%95%84%EB%8B%88%EB%8B%A4/
> https://d2.naver.com/helloworld/4911107
> https://medium.com/@goinhacker/java-8%EA%B3%BC-%ED%95%A8%EC%88%98%ED%98%95-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D-154e6d8830f1
> http://iloveulhj.github.io/posts/java/java-stream-api.html

### Lambda
Lambda란 메소드를 하나의 식으로 표현한 것이다.
