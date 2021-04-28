---
layout: post
title:  "Netflix OSS 및 Spring Boot"
comments: true
tags: Netflix, Spring boot
---
Netflix의 Backend 및 Mid-tier 어플리케이션의 대부분은 Java를 사용하여 구축되었고, Micro Service를 위해 필요한 Ribbon, Eureka, Hystrix등 클라우드 인프라 라이브러리 및 시스템을 구축했다.

2015년도에 Spring Cloud Netflix는 1.0 버전이 나왔고, Spring Boot를 사용하여 Netflix OSS 구성 요소를 결합하기 위한 커뮤니티 노력의 일환이었다. Netflix는 2018년 부터 [Spring Cloud Netflix](https://spring.io/projects/spring-cloud-netflix)를 통한 커뮤니티의 산출물을 이용하여 Java 프레임워크로 Spring Boot로 전환하였다.

![image](https://user-images.githubusercontent.com/111643/116035938-d268e500-a6a0-11eb-83c7-23bf0a21a32c.png)

Netflix가 내부 구성 요소 구축에 많은 투자를 했음에도 불구하고 Spring Boot를 채택하는 이유는 무엇일까? 2010년 초에 Netflix는 클라우드 인프라 핵심 요구사항을 안정성, 확장성, 효율성 및 보안을 최우선으로 여겼다. 이에 대한 대안이 없기에 자체 솔루션을 제작하게 되었다. 2018년에는 그때와 상황이 달라졌다 Spring 제품은 Netflix의 고유한 소프트웨어를 도입하였고 이를 통해 진화하고 확장되었다. Spring은 데이터 엑세스(Spring Data), 보안 관리(Spring Security), 클라우드 공급자와의 통합 등., 편리한 경험들을 제공하고 있다.

Spring의 진화 방향과 기능은 Netflix의 방향과 일치한다고 한다. Spring은 문서화가 잘되어 있고, 오랫동안 지속되어왔고, Netflix의 핵심 원칙인 “highly aligned, loosely coupled” 원칙과 잘 부합된다고 한다.

Netflix의 Spring Boot 전환은 Netflix가 단독으로 수행하는 작업이 아니다. Pivotal과 함께 협력하고 있다고 한다. Netflix OSS와 Spring Boot는 Netflix 외부에서 시작되었고, 이제 Netflix 내부에서 채택하고 사용한다고 한다.

Netflix의 Spring Cloud 도입 후 커뮤니티에 더 많은 기여가 일어날 수 있길 희망한다.

References
* https://medium.com/netflix-techblog/netflix-oss-and-spring-boot-coming-full-circle-4855947713a0
