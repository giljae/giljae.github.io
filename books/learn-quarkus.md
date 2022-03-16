---
layout: article
title: Quarkus를 익히다.
aside:
  toc: true
  heavy: true
---
본 글은 quarkus.io의 글을 인용/번역했습니다.

# Quarkus란 무엇인가?
기존 Java 스택은 클라우드, 컨테이너 및 Kubernetes가 존재하지 않았던 시절에 만들어졌고 메모리 요구사항이 큰 모놀리식 애플리케이션을 위해 설계되었다. Java 프레임워크는 현대의 요구사항을 충족하기 위해 진화해야했다.

Quarkus는 Java 개발자가 클라우드 네이티브 애플리케이션 개발을 쉽게 할 수 있도록 만들어졌다. GraalVM 및 HotSpot에 맞게 최적화된 Kubernetes Native Java Framework 이다.
Quarkus의 목표는 Java를 Kubernetes 및 Serverless 환경에서 분산 애플리케이션 아키텍처를 처리할 수 있는 프레임워크를 제공하는 것이다.

# 오픈소스
Quarkus는 Apache License 2.0을 따르는 오픈 소스 프로젝트이고 개방형 커뮤니티 모델을 따른다.

# Quarkus 특징
## Kubernetes Native
Quarkus는 Kubernetes용으로 구축되어 플랫폼의 복잡성을 이해하지 않고도 애플리케이션을 쉽게 배포할 수 있다.
