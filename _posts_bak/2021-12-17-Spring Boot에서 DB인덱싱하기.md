---
layout: post
title: Spring Boot에서 DB인덱싱하기
date: '2021-12-17'
description: 'Spring Boot에서 DB인덱싱하기'
categories: ['Backend', 'SpringBoot', 'JPA', 'Tips']
tags: ['Backend', 'SpringBoot', 'JPA', 'Index']
---
# Spring Boot에서 DB인덱싱하기

### 🎊 시작하기전에..

DB 인덱싱에 대한 전반적인 이해가 필요하다.

또, 인덱스 연산은 수정이 일어날 때 굉장히 비효율적이다.

위 점 참고하고 읽어주면 좋을 것 같다.

### ♻ 인덱스 설정하는 법

엔티티 클래스 위에

`@Table(indexes = @Index(name = "인덱스 명", columnList = "인덱싱 할 컬럼"))`

를 붙여주면 끝이다.