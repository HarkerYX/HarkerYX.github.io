---
layout: post
title: Innerjoin과 where의 차이
date: '2021-12-05'
description: 'Innerjoin과 Where의 차이'
categories: ['Database', 'SQL']
tags: ['Database', 'SQL', 'InnerJoin', 'Where']
---
# Innerjoin과 Where의 차이

명확성, 의도를 보여주기 좋음

Join은 관계에 관한 것이고, Where는 Filter, Filter는 전체를 분할하는 것

### 🎊 시작하기 전에...

우선, 이 생각을 하게 된 계기는 갑자기 Inner Join은 Where를 사용한 것과 같은 결과를 보이는데 왜 Inner Join을 사용해야할까? 라는 궁금증이 생겨 조사해보았다.

### ❓ Join을 왜 써야할까?

사실 Join을 사용했을때와 Where를 사용했을 때 Optimizer가 제대로 동작한다면, **동일한 퍼포먼스**를 보인다. 하지만, **코드의 의도**를 보여주기 위해 즉, **명확성을 위해**서 Join을 사용해야 한다.

그리고 Join과 Where는 방향성 자체가 다른데, Join은 **관계**에 관한 것이고, Where는 **전체에서 일부를 분할**할 때 사용하는 것이다.

