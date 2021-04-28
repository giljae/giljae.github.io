---
title:  "Servie Mesh 알아 보기"
tags: Service Mesh
---
지난 몇년간 Micro Service Architecture는 많이 발전되어 왔습니다. 그리고 현 시점에 몇 가지 새로운 개념과 패턴이 등장하고 있습니다. 이 중 “Service Mesh” 개념은 많은 인기를 얻고 있습니다. 본 글에서는 Service Mesh와 관련된 주요 개념에 대해서 설명합니다.

# Service Mesh의 등장 배경
현재까지 대부분의 사람들은 마이크로 서비스가 SOA/ESB와 같은 이전 아키텍처에서 가진 문제점들의 해답이라고 생각합니다. 그러나 실제 마이크로 서비스를 구현할 때, ESB가 지원하는 대부분의 기능들이 마이크로 서비스 수준에서 구현 가능하다는 것을 알 수 있습니다.

![image](https://user-images.githubusercontent.com/111643/116032598-f1fd0f00-a69a-11eb-8be6-cfba27739692.png)

예를 들어서 여러 가지 다운 스트림 서비스를 호출하고 이 기능을 다른 서비스로 노출해야 하는 시나리오가 있습니다. 위의 그림에서 볼 수 있듯이 ESB 아키텍처(왼쪽)를 사용하면 서비스 간 통신 중에 Circuit Breaker, Timeout 및 Service Discovery등과 같은 기능을 구축하기 위해 ESB내에 내장된 기능을 쉽게 활용 할 수 있습니다.

Micro Service를 사용하여 동일한 시나리오는 구현한다고 하면, 중앙 집중 방식의 ESB가 아니라 Code Level에서의 마이크로 서비스가 제공됩니다. 따라서 마이크로 서비스를 하기 위해서는 이러한 모든 기능이 구현되어야 합니다.

![image](https://user-images.githubusercontent.com/111643/116032619-faede080-a69a-11eb-971b-568765fcbd81.png)

위의 그림과 같이 상호간 통신하는 마이크로 서비스는 다음과 같이 구성됩니다.
* Business Logic, Process 및 서비스 구성
* Network Functions: OS의 네트워크 스택위에 구축(본 기능을 통해 기본 서비스 호출, 탄력성 및 안정성 패턴 적용, Service Discovery등을 적용)

ESB 대비하여 위의 그림처럼 마이크로 서비스를 구현하기 위해서 필요한 노력을 생각해보면 생각보다 심플하지 않습니다. 비즈니스 로직에 초점을 맞추기보다는 서비스 간 통신 기능을 구현하는 데 많은 시간을 투자해야 합니다. 또한 Polyglot 형태의 여러 프로그래밍 언어를 지원해야 할 경우에는 각 언어별로 노력을 들여야 하기 때문에 다중 기술을 사용하여 마이크로 서비스를 구현하는 것은 쉽지 않습니다.

> 마이크로 서비스 아키텍처 구현에서 가장 복잡한 부분은 서비스 자체를 구축하는 것이 아니라 서비스 간의 통신입니다.

마이크로 서비스간 커뮤니케이션의 요구 사항은 매우 일반적이기에 이러한 모든 작업을 다른 Layer에서 Offloading 하는 것에 대해서 생각해 볼 수 있습니다. 그래서 “Service Mesh”가 등장했습니다.

# Service Mesh란 무엇인가?
Service Mesh는 서비스간 커뮤니케이션 인프라입니다. Service Mesh를 사용하게 되면 아래의 특징을 가질 수 있습니다.
* 마이크로 서비스는 다른 마이크로 서비스와 직접 통신하지 않습니다.
* 모든 마이크로 서비스간 통신은 Service Mesh(or Sidecar Pattern)를 통하게 됩니다.
* Service Mesh는 탄력성, Service Discovery등과 같은 일부 네트워크 기능을 기본적을 지원 합니다.
* Service Mesh를 이용하면 개발자는 비즈니스 로직에 더 집중 할 수 있으며 네트워크 통신과 관련된 대부분의 작업은 Service Mesh로 Offloading 하게 됩니다.
* 예를 들어서, 마이크로 서비스가 다른 서비스를 더이상 호출하지 않을 때 Circuit Breaker에 대해 걱정할 필요가 없습니다. 이 또한 Service Mesh의 일부로 제공되고 있습니다.
* Service Mesh는 프로그래밍 언어에 제약을 받지 않습니다. 마이크로 서비스는 항상 HTTP1.x/2.x, gRPC등과 같은 표준 프로토콜을 사용하기 때문에 이를 기반으로 마이크로 서비스를 작성 할 수 있습니다.

![image](https://user-images.githubusercontent.com/111643/116032794-44d6c680-a69b-11eb-9e6f-332ab3349ca4.png)

위의 그림에서 언급된 서비스 상호 작용 및 책임에 대해서 설명합니다.

## 비즈니스 로직
서비스 구현에 필요한 기능을 의미합니다.

## 기본 네트워크 기능
대부분의 네트워크 기능을 Service Mesh로 Offloading 할지라도 특정 서비스는 Service Mesh / Sidecar Pattern Proxy와 연결하기 위해 기본적인 네트워크 상호 작용 기능을 포함해야 합니다. 따라서 서비스 구현은 주어진 네트워크 라이브러리(ESB와 다르게 간단한 추상화를 사용)를 사용하여 네트워크 호출을 해야 합니다. (Service Mesh 전용)

## 어플리케이션 네트워크 기능
Circuit Breaker, Timeout, Service Discovery등과 같이 네트워크에 밀접하게 결합된 어플리케이션 기능이 존재합니다. 초기 마이크로 서비스를 구현할 경우에는 ESB Layer에서 제공되는 네트워크 기능을 무시하고 각 마이크로 서비스 수준에서 모든 기능을 처음부터 구현했습니다. 현 시점에서 분산형 Mesh와 비슷한 공유 기능을 갖는 것이 중요하다는 사실을 깨닫기 시작했습니다. 즉 서비스 코드/비즈니스 로직과 명시적으로 분리된 Service Mesh의 기능을 사용 할 수 있도록 합니다.

## Service Mesh 제어 기능
모든 Service Mesh Proxy는 Control Plane에 의해 중앙에서 관리됩니다. Access 제어, Service Discovery등과 같은 Service Mesh 기능을 지원할 때 매우 유용합니다.

# Service Mesh 기능
Service Mesh는 어플리케이션 네트워크의 기능을 제공하는 반면 일부 네트워크 기능은 여전히 마이크로 서비스 수준 자체로 구현됩니다. Service Mesh에서 어떤 기능이 제공되어야 하는지에 대한 규칙은 없습니다. 아래는 Service Mesh에서 가장 일반적으로 제공되는 기능입니다.
* Resiliency for inter communications: Circuit Breaker, Timeouts, Retries, Fail injection, fault handling, load balancing, failover
* Service Discovery: 전용 Service Registry를 통해 Service Endpoint를 검색
* Routing: 서비스 비즈니스 기능과 관련된 라우팅 제공
* Observability: Metrics, monitoring, distributed logging, distributed tracing 제공
* Security: TLS 제공 및 Key 관리
* Access Control: Blacklist/Whitelist 기반 Access 제어
* Deployment: Container 배포 지원
* Interservice communication protocols: HTTP1.x, HTTP2, gRPC 제공

# Servie Mesh 구현체
[Linkerd](https://linkerd.io/) 및 [Istio](https://istio.io/)와 같은 오픈소스 구현체가 존재합니다. [여기](https://abhishek-tiwari.com/a-sidecar-for-your-service-mesh/)에서 둘 간의 차이를 확인하세요.

# Service Mesh 장단점
## 장점
* 마이크로 서비스 코드 외부에서 구현되기에 다양한 프로그래밍 언어도 지원하고 재사용 가능합니다.
* Ad-hoc 솔루션을 사용한 마이크로 서비스 아키텍처의 대부분의 문제를 해결합니다: Distributed tracing, logging, security, access control등
* 다양한 프로그래밍 언어 지원: 특정 언어가 네트워크 어플리케이션 기능을 구축 할 수 있을지 또는 라이브러리가 지원되는지에 대해 걱정이 없습니다.

## 단점
* 복잡성: Service Mesh를 사용하면 런타임 인스턴스 수가 증가합니다.
* Extra hop 추가: 각 서비스 호출은 Service Mesh의 Sidecar Proxy를 통해서 호출되어야 합니다.
* Service Mesh는 서비스 간 통신 문제를 다루지만 복잡한 라우팅, Mediation등의 기능을 제공하진 않습니다.

# 결론
Service Mesh는 마이크로 서비스 아키텍처의 구현과 관련하여 주요 과제중 일부를 해결 합니다. 그리고 다양한 마이크로 서비스 구현 기술 집합을 선택할 수 있고 서비스 간 네트워크 기능을 지원해주므로써 개발자는 비즈니스 로직에 더 집중 할 수 있습니다. 그러나 Service Mesh는 비즈니스 로직과 관련된 서비스 통합 문제를 해결하지는 못합니다.

References: https://medium.com/microservices-in-practice/service-mesh-for-microservices-2953109a3c9a
