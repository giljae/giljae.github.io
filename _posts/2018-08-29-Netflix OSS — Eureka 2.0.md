---
layout: post
title:  "Netflix OSS — Eureka 2.0"
comments: true
tags: Netflix, Eureka
---
# What is Eureka?
Eureka는 중간 계층 서버의 로드 균형 조정 및 장애 조치를 위한 REST기반 서비스이다. Eureka는 Java 기반 클라이언트 구성 요소인 Eureka Client가 함께 제공되므로 서비스와의 상호 작용이 훨씬 쉬워진다. 또한 클라이언트에는 기본 Round Robin 알고리즘 및 기본 제공 로드 밸런싱 알고리즘이 존재한다.

# What is the need for Eureka?
AWS 클라우드에서는 IP 주소와 host name으로 작동하는 기존 로드 밸런서와 달리 서버 등록 및 등록 취소 작업을 정교하게 수행해야 하는 로드 밸런서가 필요하다. AWS는 미들 티어 로드 밸런서를 제공하지 않으므로 미드 티어 로드 밸런싱을 직접 구비할 필요가 있다.

# How different is Eureka from AWS ELB?
AWS ELB는 최종 사용자의 웹 트래픽에 노출된 Edge Service용 로드 밸런싱 솔루션이다. Eureka는 미드 티어 로드 밸런싱용이다. 이론적으로 AWS ELB 뒤에 중간 계층 서비스를 배치 할 수 있지만 EC2 클래식에서는 AWS 보안 그룹의 모든 유용성을 잃어 버리고 외부 세계에 노출될 수 있는 문제점이 존재한다.

AWS ELB는 전통적인 Proxy 기반 로드 밸런서이기도 하지만 Eureka에서는 로드 밸런싱이 인스턴스/서버/호스트 수준에서 발생한다는 점이 차이점이다. 클라이언트 인스턴스는 연동할 모든 서버에 대한 정보를 알고 있어야 한다.

Eureka를 사용하여 로드 밸런싱과 Proxy기반의 로드 밸런싱을 차별화하는 또 다른 측면은 사용 가능한 서버에 대한 정보가 Client에 Cache되므로 어플리케이션이 로드밸런싱 장애에 대해 복원력을 가질 수 있다라는 점이다.

# How different is Eureka from Route 53?
Route 53은 DNS 레코드를 호스팅 할 수 있는 DNS 서비스이다. Eureka는 내부 DNS와 유사하지만 전 세계 DNS서버와 관련이 없다. Eureka는 다른 AWS 지역의 서버에 대해 알지 못한다.(지역 분리) 정보를 보유하는 유일한 목적은 한 지역내의 로드 밸런싱을 위한 것이다.

미드 티어 서버를 Route 53에 등록하고 AWS 보안 그룹을 사용하여 공용 엑세스에서 서버를 보호 할 수 있지만 미드 티어 서버 ID는 여전히 외부 세계에 노출되어 있다. 또한 Traffic이 건강하지 않거나 존재하지 않는 서버로 라우팅 될 수 있는 전통적인 DNS 기반 로드 밸런싱은 단점이 존재한다. (서버가 언제든지 사라질 수 있는 클라우드의 경우)

# How is Eureka used at Netflix?
Netflix에서 Eureka는 미드 티어 로드 밸런싱에서 중요한 부분을 차지하지만 다음과 같은 목적으로 사용된다.
* Netflix Asgard를 사용하는 red/black 배포의 경우 Eureka는 Asgard와 상호 작용하여 문제가 발생했을때 신속하고 원할하게 이전/신규 릴리즈 전환이 가능하다.
* 여러가지 이유로 서비스에 대한 추가 어플리케이션 관련 메타 데이터를 전달하는 용도로도 사용한다.

# High level architecture
![image](https://user-images.githubusercontent.com/111643/116031704-312a6080-a699-11eb-9d96-8a9f259b9886.png)

위의 아키텍처는 Eureka가 Netflix에서 어떻게 사용되는지를 보여 주는 일반적인 방법이다. 해당 지역의 인스턴스에 대해서만 알고 있는 Eureka Cluster가 하나 존재한다.
어플리케이션 서비스를 Eureka에 등록한 다음 30초마다 갱신하기 위해 Heartbeat를 보낸다. 클라이언트가 임대를 몇 번 갱신 할 수 없으면 약 90초내에 서버 레지스트리에서 제거된다. 등록 정보와 갱신은 클러스터의 모든 유레카 노드에 복제된다. 모든 영역의 클라이언트는 레지스트리 정보(30초마다 발생)를 찾아 해당 서비스를 찾고 원격 호출을 할 수 있다.

# Non-Java services and clients
Java 기반이 아닌 서비스의 경우 서비스 언어로 Eureka의 클라이언트 부분을 구현 할 수 있고 등록을 처리하는 Eureka 클라이언트가 내장된 Java 프로그램을 실행 할 수 도 있다. Java가 아닌 클라이언트는 REST를 사용하여 다른 서비스에 대한 정보를 Query할 수 있다.

# Configurability
Eureka를 사용하면 클러스터 노드를 즉시 추가하거나 제거 할 수 있다. 시간 제한 부터 Thread pool까지 내부 구성을 조정할 수 있다.

# Monitoring
Eureka는 성능, 모니터링 및 경고를 위해 Servo를 사용하여 클라이언트와 서버 모두에서 정보를 추적한다. 데이터는 JMX 레지스트리에서 사용할 수 있으며 AWS Cloud Watch로 보낼 수 있다.

# Architecture Overview
Eureka 2.0은 클라우드 구축을 위해 설계된 Service Discovery Framework이다.

아래 그림은 일반적인 Eureka 2.0의 주요 구성 요소를 나타낸다.

![image](https://user-images.githubusercontent.com/111643/116031729-42736d00-a699-11eb-8062-af69537027ab.png)

Write Register는 Client 등록을 처리하고 내부 서비스 Registry를 관리하고 유지하는 상태 저장 시스템이다. Registry의 내용은 모든 Write Server Node간에 일관성 있게 복제된다. Write Registry의 내용은 Eureka Client가 사용하는 Read Cluster에서 읽게된다.

Read Cluster는 Cache 계층이기 때문에 Traffic Volume에 따라 쉽게 빠르게 확장 및 축소될 수 있다. Write Cluster는 Peak time의 Traffic을 처리할 만큼 충분한 용량을 미리 산정해서 준비해야 한다.

# Client Registration
Eureka Client는 Registration, Heartbeat 및 Update를 통해 Discovery 되도록 할 수 있다. Registration에는 검색 가능한 식별자 및 서비스 상태 그리고 자유로운 메타 데이터가 포함된다. 이러한 작업을 담당하는 Eureka 2.0 서버는 Write Cluster로 구성된다.

![image](https://user-images.githubusercontent.com/111643/116031754-51f2b600-a699-11eb-904b-40971007b7d1.png)

단일 Client는 여러 서비스 인스턴스를 등록할 수 있다. 가상화된 환경에서 Network stack을 100% 신뢰할 수 없기 때문에 연결의 유효성을 결정하는데에 Heatbeat를 사용한다. 연결이 끊어지면 Write Cluster Registry의 등록 항목이 추출 대기열에 들어가고 궁극적으로 Registry에서 제거됩니다. 정상 동작인 Client는 연결을 해제하기 위해서는 항상 등록 취소 요청을 보내야 한다. 등록 후에 Client는 인스턴스 데이터를 변경하면서 원하는 수의 업데이트 요청을 전송 할 수 있다.

# Registry Discovery
Eureka Client는 세트에 가입할 수 있다. 성공적인 Subscription 후에 모든 변경 사항이 서버에 의해 Client에 push된다. 이러한 작업을 담당하는 Eureka 서버는 Read Cluster를 구성한다.

![image](https://user-images.githubusercontent.com/111643/116031793-66cf4980-a699-11eb-9bdb-ee4bc0258b6e.png)

# Service registry data model
Eureka 2.0은 서로 다른 클라우드 공급자 및 데이터 센터에서 작동하도록 설계되었고 현재 또는 미래의 배치를 수용할 수 있도록 기본 데이터 모델을 확장하도록 설계되었다. 기본 데이터 센터 모델 및 Amazon AWS / VPC 클라우드가 지원된다.

![image](https://user-images.githubusercontent.com/111643/116031814-7058b180-a699-11eb-8757-6be230dee55e.png)

사전 정의 된 서비스 인스턴스 속성 세트는 메타 데이터 맵에 추가 할 수 있는 Key / Value 구조의 사용자 정의 세트를 통해 확장할 수 있다. Network Topology는 미리 정의되지 않는다. 따라서 간단한 공용/개인 IP 모델이 AWS 배포에 제공되지만 VPC에는 다중 NIC/ENI가 지원된다.

# Interest subscription model
구독 모델은 사전 정의된 클래스 집합으로 구성된다.
* application interest — 주어진 어플리케이션에 속한 모든 서비스 인스턴스
* vip interest — 특정 Eureka 가상 주소(vip)에 속한 모든 서비스 인스턴스
* instance id — 지정된 ID를 가지는 특정 인스턴스
* full registry — Registry의 모든 항목을 나타내는 특수한 관심 유형(대규모 레지스트리의 경우 막대한 트래픽을 생성할 수 있으므로 거의 사용하지 않아야 함)

어플리케이션/VIP/Instance ID interest에는 연결된 운영 Rule이 있어야 하며 허용되는 값은 다음과 같다.
* Equals — 정확히 제공한 값과 일치
* Like — 관심 값을 정규 표현식으로 취급

# Dashboard
Eureka 2.0 Dashboard는 Eureka Cluster 관리/모니터링을 위한 선택적 구성 요소이다. 특정 인스턴스로 드릴 다운하여 쉽게 문제를 해결하거나 시스템 진단을 수행할 수 있는 수준의 Dashboard를 제공한다.

# CAP theorem
CAP 정리 관점에서 Eureka의 Write Cluster는 AP 시스템이고(고가용성 및 파티션 허용). 이 선택은 클라우드 기반 검색 서비스의 기본 요구 사항에 의해 결정됩니다. 클라우드에서 특히 대형 배치의 경우 장애는 항상 발생한다. 이것은 Eureka 서버 자체, 등폭된 클라이언트 또는 네트워크 파티션에서 문제가 발생한 것일 수 있다. 이러한 모든 상황에서 Eureka는 Registry 정보를 제공하고 사용 가능한 각 노드에서 새등록을 격리하여 사용할 수 있어야 한다. Eureka는 가용성을 선택하기 때문에 이러한 시나리오의 데이터는 노드간에 일관성이 없다. 이 모델은 레지스트리 데이터가 항상 일정 수준 이상 유지되도록하기 때문에 적절한 클라이언트 측 로드 균형 조정 및 장애 조치 매커니즘으로 보완되어야 한다.(e.g. Ribbon)

sources: https://github.com/Netflix/eureka/wiki/Eureka-at-a-glance
