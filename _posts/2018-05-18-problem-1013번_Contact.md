---
layout: post
title: "1013번_Contact"
categories:
  - problem
tags:
  - problem
---

[1013](https://www.acmicpc.net/problem/1013)

문제의 주어진 조건
(100+1+ | 01)+
이 정규 표현식 형태로 되어 있기 때문에
정규표현식을 DFA로 변환해서 문제를 풀수 있다

형식언어때 배웠던것을 기억해보면

정규표현식을 이용해서 NFA를 그리고 NFA를 DFA로 변환한다.

결정적 유한 오토마타
DFA란 한 상태에서 다른 상태로 넘어갈 때 입력 값이 반드시 하나만 존재하는 것으로
프로그래밍이 가능하다

비결정적 유한 오토마타
NFA란 한 상태에서 다른 상태로 넘어갈때 입력 값이 하나 이상 존재하는 것으로
프로그래밍이 불가능하다.

[참고](http://blog.naver.com/PostView.nhn?blogId=noma000&logNo=220898432958&parentCategoryNo=&categoryNo=35&viewDate=&isShowPopularPosts=true&from=search)
