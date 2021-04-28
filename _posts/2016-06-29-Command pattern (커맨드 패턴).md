---
title:  "Command pattern (커맨드 패턴)"
tags: 커맨드패턴
---

옆에 계신 정모 부장님이 요 근래에 Batch 관련 가이드를 준비하고 계신다. 모듈당 N개의 Batch 프로그램이 있다고 하면, 각각 Executable 형태로 제공하는 것이 좋을 것인가? 아니면 관리적 측면에서 Grouping 하여 제공하는 것이 좋을 것인가에 대한 의견이 분분하다.

이런 경우에는 각 모듈별로 하나의 Batch 프로젝트를 구성하고 수행해야 할 Job Method 호출을 캡슐화(Encapsulation) 하는 것에 대해서 공감대가 형성되었다.

![image](https://user-images.githubusercontent.com/111643/115677463-75f98300-a38b-11eb-9328-ad8237494526.png)

![image](https://user-images.githubusercontent.com/111643/115677483-7a25a080-a38b-11eb-86b4-5b39e17c864c.png)

http://kapilnevatia.blogspot.kr/2011/11/command-pattern.html
