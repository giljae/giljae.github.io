---
title:  "Spring Native 베타 소개"
tags: Spring Native
---
3/11일에 Spring Native 베타가 릴리즈되었다. GraalVM을 활용하여 Spring Java 및 Kotlin 애플리케이션을 네이티브 이미지로 컴파일하여 JVM에 구동되는 애플리케이션에 비해 시작 시간과 메모리 오버 헤드를 줄인다.

JVM 실행 파일에 비해 네이티브 이미지는 시작 시간이 더 빠르고 (100ms 미만) 메모리 소비가 적다. 하지만 네이티브 이미지를 빌드하려면 JVM을 이용하는 빌드 대비 더 많은 시간이 필요하다.

이 프로젝트는 아직 베타 버전이긴 하지만, Spring Framework, Spring Boot, Spring Security 및 Spring Cloud를 포함한 대부분의 Spring 프로젝트 모듈을 지원한다.

Spring Native는 Java 및 Kotlin 언어를 지원한다. Spring Native가 좋은 선택이 될 수 있는 상황은 아래와 같다.
* Spring Cloud Function을 사용하는 서버리스 애플리케이션
* Spring을 이용한 마이크로 서비스
* Kubernetes 환경에서의 애플리케이션

Spring Native를 사용하면 개발자는 Java Development Kit, Spring의 필수 기능 및 애플리케이션에 필요한 종속성만으로 최소한의 OS 계층과 네이티브 실행 파일로 최적화된 컨테이너 이미지를 생성할 수 있다. Spring 진영에서는 기존 Spring Boot 애플리케이션을 변형하지 않아도 적용될 수 있게 고려중이다.

일반 JVM과 Native Image의 주요 차이점은 다음과 같다.
* 애플리케이션의 정적 분석이 빌드시 수행된다.
* 사용하지 않는 것은 빌드시 제거된다.
* 리플렉션, 리소스 및 동적 프록시에 대한 구성이 필요하다.
* 클래스 경로는 빌드시 고정된다.
* 클래스 지연 로딩이 없다. → 제공된 모든것이 시작시 메모리에 로드됨
* 일부 코드는 빌드시 실행된다.
* 몇 가지 제한 사항이 존재한다.

# 사전 작업
현재 사용하고 있는 환경이 맥이라서 OSX 기준으로 설명하려 한다.

SDKMAN를 이용해 GraalVM과 maven을 설치한다.

## SDKMAN 설치
아래의 명령어를 이용해 sdkman을 설치한다.
<script src="https://gist.github.com/giljae/2a0d099da237447909f4e755777c80f8.js"></script>

## GraalVM 설치
<script src="https://gist.github.com/giljae/67482419336e269d5bddd6291202c219.js"></script>

## Maven 설치
<script src="https://gist.github.com/giljae/8c53cb3755337c0805439dc2b589a4ff.js"></script>

# Spring Native Boot strap
[Spring Initializr](https://start.spring.io/)에서 프로젝트를 부트 스트랩 할 때 애플리케이션에 Spring Native를 추가 할 수 있다.

![image](https://user-images.githubusercontent.com/111643/116084334-a701ec80-a6d8-11eb-9a9d-c6449018a3a7.png)

생성된 프로젝트에는 Spring Native 프로젝트와 애플리케이션 소스 코드를 네이티브 실행 파일로 컴파일하는데 사용되는 Spring AOT 플러그인에 대한 종속성이 포함되며 호환성과 풋 프린트가 향상된다.

아래는 Maven Script 예시이다.

<script src="https://gist.github.com/giljae/2d9e7d7683c22ad7b719bface50db59a.js"></script>

# Spring WebFlux로 REST 엔드 포인트 정의
애플리케이션을 테스트할 수 있도록 Spring WebFlux로 REST 엔드 포인트를 정의해보자.

SpringNativeExampleApplication 클래스, RouterFunction을 사용하여 REST 엔드 포인트를 추가 할 수 있다.

<script src="https://gist.github.com/giljae/bb9152ef0908cc0e2671f4476bec8f72.js"></script>

그리고 SpringNativeExampleApplicationTests 클래스에서 REST에 대한 테스트 코드를 작성해보자.

<script src="https://gist.github.com/giljae/06ae8738325f8810472e2b94cad7e72a.js"></script>

# 애플리케이션을 jar로 실행하기
프로젝트의 Spring Native 종속성은 Spring AOT 플러그인을 통해 JAR로 실행하는 경우에도 시작 시간과 메모리 소비를 최적화 한다.

터미널 창을 열고 프로젝트 디렉토리로 이동 후 아래의 명령을 실행한다.
<script src="https://gist.github.com/giljae/f6b18ccc92f6f7d75b66097b6cc7012b.js"></script>

정상적으로 구동되었는지 테스트해보자.
<script src="https://gist.github.com/giljae/c28f31ad783023a3d81780ee41085367.js"></script>

jar로 정상 동작되는것을 확인했으니., 이제 네이티브 이미지로 실행해보도록 한다.

# 애플리케이션을 Native 이미지로 실행하기
이제 GraalVM과 함께 Spring Native를 활용하여 네이티브 이미지를 빌드하고 실행하자.

네이티브 이미지를 빌드하는 것은 Spring Boot 플러그인을 사용하면 매우 간단하다. 여기서는 Docker를 이용하지 않고, Native Image를 빌드하는 것으로 설명한다.

이미 pom.xml에 네이티브 이미지 생성 plugin을 작성했기에, 프로젝트 디렉토리에서 아래의 명령어를 실행한다.
<script src="https://gist.github.com/giljae/3899b330f88be902379c473708e0ec82.js"></script>

많은 시간이 흐른 후, 빌드가 성공하면 프로젝트내 /target 디렉토리 밑에 spring-native-example이라는 파일이 생성된다.

/target 디렉토리로 이동하여 아래의 명령어를 실행해서 애플리케이션을 구동해보자.

<script src="https://gist.github.com/giljae/7dd6383291be4ae848ce852350372412.js"></script>

정상적으로 구동되었는지 테스트 해보자.
<script src="https://gist.github.com/giljae/e010183709326c563a0df32675fa0f6c.js"></script>

# 결론
본 글에서는 Spring Boot 애플리케이션을 빠르게 부트 스트랩하고 Spring Native 및 GraalVM을 사용하여 네이티브 실행 파일로 컴파일하는 방법을 살펴보았다.
위에서 사용된 코드는 [이곳](https://github.com/giljae/sandbox/tree/master/spring-native-example)에 올려두었다.
Spring Native 프로젝트에 대한 자세한 정보를 보려면 [공식 문서](https://docs.spring.io/spring-native/docs/current/reference/htmlsingle/)를 참조하길 바란다.

