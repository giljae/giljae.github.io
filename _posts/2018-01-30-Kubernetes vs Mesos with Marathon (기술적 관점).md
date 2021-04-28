---
layout: post
title:  "Kubernetes vs Mesos with Marathon (기술적 관점)"
comments: true
tags: Kubernetes, Mesos, Marathon
---

이전 포스팅에서 Kubernetes와 Mesos에 대한 사업적 관점에서 비교를 했었습니다. 본 글에서는 기술적 관점에서의 비교글을 작성합니다.

# Kubernetes
Kubernetes는 컨테이너 어플리케이션의 automating deployment, scaling 및 관리를 위한 오픈 소스 시스템입니다. Kubernetes의 아키텍처는 아래와 같습니다.

![image](https://user-images.githubusercontent.com/111643/115835988-e8349b00-a451-11eb-84f0-1d831903123a.png)

Kubernetes Cluster와 관련된 구성 요소들은 아래와 같습니다.
* etcd: etcd는 분산 key-value store이고, Kubernetes에서는 Master Node의 API Server가 HTTP/JSON API를 이용하여 접근할 수 있는 구성 데이터를 저장하는 용도로 사용됩니다.
* API Server: Master Node의 Hub입니다. 다양한 Component와의 인터페이스를 원할히 해주는 역할을 담당합니다.
* Controller Manager: 작업에 대한 부하를 조절하여 클러스터의 상태를 좋게 유지하도록 합니다.
* Scheduler: Workload를 적절히 Node에 배치합니다.
* Kubelet: API Server로 부터 Pod의 specificatio을 전달 받아 Host에서 수행중인 Pod를 관리합니다.
* Master: Kubernetes Node를 제어하는 역할을 수행
* Node: Pods가 실행되는 Machine입니다.

다음은 Kubernetes와 관련된 일반적인 용어를 정리한 내용입니다.
* Pods: Kubernetes는 Pod라는 그룹으로 Container를 배치하고 예약합니다. Pod의 Container는 동일한 Node에서 실행되며 리소스를 공유합니다. (e.g. File System, Kernel Namespace, IP address)
* Deployments: Pod 그룹을 만들고 관리 할 수 있습니다. 수평적 확장이나 가용성 보장을 위해 사용됩니다.
* Label: Object에 연결된 Key-Value 입니다. Label을 사용하여 여러 Object를 단일 세트로 검색하고 업데이트 할 수 있습니다.

# Mesos + Marathon
Mesos는 Data Center에서 리소스를 동적으로 할당 하는 것을 목표로 하는 Distributed Kernel 이고, 리소스 공유 기능을 사용하는 수많은 Framework, Application Stack을 제공합니다. 각 Framework는 Scheduler와 Executor로 구성됩니다. Marathon은 Application 및 기타 Framework를 시작할 수 있는 Meta Framework입니다. Container Workload에 대한 확장 및 Self Healing 기능을 제공하는 Orchestration Platform 역할도 담당 할 수 있습니다. 아래 그림은 Mesos + Marathon의 아키텍처 입니다.

![image](https://user-images.githubusercontent.com/111643/115836190-2467fb80-a452-11eb-8321-4efa8dd989da.png)

Mesos와 Marathone의 구성요소는 아래와 같습니다.
* Mesos Master: Container 관리를 위한 Marathon, 대규모 데이터 처리를 위한 Spark와 NoSQL 데이터 베이스를 위한 Cassandra와 같은 Framework에서 Resource 공유를 가능하게 됩니다.
* Mesos Slave: 사용 가능한 Resource를 Master에게 알려주는 Agent를 실행합니다.
* Framework: Master가 Slave 노드에서 실행되는 Task를 전달 받을 수 있도록 Mesos Master에 등록됩니다.
* Zookeeper: Cluster state를 Read/Write할 수 있는 가용성 높은 Naming Registry를 제공합니다.
* Marathon Scheduler: Mesos Master로 부터 오퍼를 받아 Slave Node의 사용 가능한 CPU 및 Memory list를 제공합니다.
* Docker Executor: Marathon Scheduler에서 작업을 받고 Slave Node에서 Container를 실행합니다.

# Mesosphere DCOS
Mesosphere Enterprise DC/OS는 Mesos Distributed Kernel을 활용하여 Container 및 대용량 데이터 관리 및 사용자 인터페이스, 모니터링 도구 및 기타 기능을 제공합니다. 아래의 그림은 DCOS의 아키텍처 입니다.

![image](https://user-images.githubusercontent.com/111643/115836261-3a75bc00-a452-11eb-858a-9cae3eacc2e7.png)

DCOS는 Package Management, Container Orchestration, Cluster Management 및 기타 구성 요소로 구성됩니다.

# Kubernetes vs Mesos + Marathon
## Application Definition
* Kubernetes
** Application은 Pod, Deployment 및 Service의 조합을 사용하여 배포할 수 있습니다. Pod는 함께 배치된 Container Group이고 최소 배포 단위입니다. Deployment에는 여러 노드에 복제본이 존재할 수 있습니다. Service는 Container Workload의 “external face”이며 요청을 Round Robin 하기위해 DNS와 통합합니다.
* Mesos + Marathon
** Application은 Marathon에 의해 예약된 작업으로 Node에서 실행됩니다. Mesos의 경우에는 Application은 Marathon, Cassandra, Spark등의 Framework이며, Marathon은 Container를 Slave Node에서 실행되는 Task로 예약합니다. Marathon 1.4에서는 Kubernetes와 같은 Pod 개념을 도입하였지만 Marathon Core내의 기능은 아닙니다.

## Application Scalability Constructs
* Kubernetes
** 각각의 Application 계층은 Pod로 정의되며 YAML을 사용하여 배포에 대해 선언적으로 표현합니다. 스케일링은 수동/자동으로 수행 할 수 있습니다.
* Mesos + Marathon
** CLI or UI를 사용할 수 있고, JSON으로 정의하여 Docker Container의 저장소, 리소스, 인스턴스 수 및 실행할 명령을 지정할 수 있습니다. Marathon UI를 사용하여 스케일업을 수행 할 수 있고 Marathon Scheduler는 지정된 조건에 따라 Slave Node에 Container를 분배합니다.

## High Availability
* Kubernetes
** Pod를 Node간에 배포하여 HA를 제공합니다. Load Balance 서비스는 유해한 Pod를 탐지하여 제거 합니다. 다중 Master Node와 Worker Node는 kubectl과 client의 요청에 대해 Workload를 조정할 수 있습니다. etcd를 Clustering하고 API Server도 복제할 수 있습니다.
* Mesos + Marathon
** Zookeeper를 사용하여 Mesos 및 Marathon의 HA를 지원합니다. Zookeeper는 Mesos와 Marathon의 leader를 선출하고 Clustering 상태를 유지하도록 도와줍니다.

# Load Balancing
* Kubernetes
** Pods는 Service를 통해서 expose 됩니다. load balancing에 대해서는 여기를 참조하세요
* Mesos + Marathon
** Host port를 여러 Container port에 매핑하는 방식을 사용합니다

## Auto Scaling
* Kubernetes
** Scaling은 Deployments를 사용하여 선언적으로 정의합니다. resource metric기반의 Auto-scaling도 지원됩니다. Resource metric은 CPU, Memory 사용률과 Request, packet 및 Custom metric도 지원합니다.
* Mesos + Marathon
** Marathon은 구동중인 Container의 Instance 개수를 지속적으로 모니터링합니다. Container중 하나가 fail나면 Marathon은 다른 Slave Node로 다시 Schedule합니다. Resource metric기반의 Auto-scaling은 지원하는 component를 통해서만 사용할 수 있습니다.

## Application Upgrade and Rollback
* Kubernetes
** Rolling-update와 recreate 전략을 deployment에 모두 지원합니다.
* Mesos + Marathon
** Deployment를 사용하여 Rolling-update를 지원합니다.

## Health Checks
* Kubernetes
** liveness, readiness 두가지를 지원합니다.
* Mesos + Marathon
** HTTP, TCP 및 기타 프로토콜등 여러 프로토콜에서 Health check 기능을 사용할 수 있습니다.

## Storage
* Kubernetes
** 두 개의 Storage API를 제공합니다.
** Individual storage backends (e.g NFS, AWS EBS, Ceph, Focker에 대한 추상화 지원)
** Storage resource request (다른 저장소의 리소스 요청에 대한 추상화 제공)
** Block 또는 File을 지원하는 여러 유형의 Persistent volume을 제공합니다. (e.g iSCSI, NFS, FC, Amazon Web Services, Google Cloud Platform, MS Azure)
** emptyDir volume은 비영구적이며 Container로 파일을 read/write할 수 있습니다.
* Mesos + Marathon
** Local persistent volume(Beta version)은 MySQL와 같은 상태 저장 응용 프로그램에서 지원됩니다.
** Amazon EBS와 같은 외부 저장소 사용도 Beta version 입니다.
** 한번에 하나의 작업에만 Volume을 첨부 할 수 있기 때문에 외부 volume를 사용하는 Application은 단일 Instance로만 확장 할 수 있습니다.

# Networking
* Kubernetes
** 모든 Pod가 상호간에 통신할 수 있는 flat network model입니다. (overlay로 구현됨)
** 이 모델에는 두 개의 CIDR이 필요합니다. 하나는 Pod가 IP 주소를 얻고 다른 하나는 Service에서 사용됩니다.
* Mesos + Marathon
** Host mode or Bridge mode로 구성 할 수 있습니다.
*** Host mode
**** Host port는 Container에 의해 사용됩니다. 이로 인하여 Host에서 port 충돌이 발생할 수 있습니다.
*** Bridge mode 
**** Container port를 매핑하여 Host port에 연결됩니다. Host port는 배포시 동적으로 할당 될 수 있습니다.

# Service Discovery
* Kubernetes
** 환경 변수 혹은 DNS를 사용하여 찾을 수 있습니다. Pod가 실행될 때에 kubelet은 몇가지 환경 변수를 추가합니다. (e.g PSVCNAME_SERVICEHOST}, {SVCNAME_SERVICE_PORT}, Docker link 변수)
** DNS server는 addon으로 사용할 수 있습니다. 전체 Cluster에서 DNS를 사용하게 되면 Pod는 자동으로 부여하는 Service Name을 사용할 수 있습니다.
* Mesos + Marathon
** Service는 IP, Port와 연결된 DNS 레코드를 통해 찾을 수 있습니다.
** Service는 Mesos-DNS에 의해 자동으로 DNS에 레코드에 할당됩니다. 선택적으로 명명된 VIP도 작성 할 수 있습니다. VIP를 통한 요청은 LB처리가 됩니다.

# Performance and Scalability
* Kubernetes
** 1.6 release에서 5000 node까지 확장됩니다. 여러 Cluster를 사용하면 5000 cluster 제한을 초과하여 사용할 수 있습니다.
* Mesos + Marathon
** Mesos + Marathon 조합은 확장성이 뛰어납니다. Digital Ocean에 따르면 Mesos 및 Marathon Cluster는 10000 node로 확장됩니다.

# Synopsis
* Kubernetes
** On-premise SAN 및 Public Cloud를 포함한 다양한 Storage 옵션 제공
** 이미 Google에서 대규모로 사용되고 있음
** Container Orchestration 중에 가장 큰 규모의 커뮤니티를 가지고 있음
* Mesos + Marathon
** Amazon EBS 및 외부 저장소는 Beta version
** 상용 업체에 의해 Control 됨
** 소규모 커뮤니티

# Mesos + Marathon 대비 Kubernetes의 단점
* Kubernetes
** 단일 공급 업체가 없기에 사용에 대한 의사 결정이 복잡해 질 수 있음 (문제 발생시 누가 책임질 것인가?)
** Kubernetes는 Container Orchestration 전용으로 제작되었음
** 5000 node cluster까지 확장되고, 그 이상을 사용하려면 여러 개의 Cluster가 필요
* Mesos + Marathon
** 단일 공급 업체를 통해 버그 수정 및 패치를 제공 받음 (문제 발생시 책임짐)
** 2-tier 아키텍처를 사용하면 다른 Framework를 배포할 수 있음
** Apple, Bloomberg, Netflix내의 일부 조직에서는 10000개 이상의 node를 통해 대규모로 Mesos를 사용중 (참고: Mesosphere 블로그)
** Kubernetes는 오픈 소스 프로젝트이고 많은 참여가 일어나고 있음
** Load Balancing 및 DNS와 같은 Network 기능 제공
** Logging/Monitoting
* Kubernetes
** ELK, sysdig, cAdvisor, Heapster/Grafana/InfluxDB 와 같은 외부 도구 사용 가능
* Mesos + Marathon
** 내부적으로 집계 가능한 Log를 제공하고 모니터링은 외부 도구를 사용
** Auto-Scaling은 기본적으로 지원

# 무엇을 선택해야 할까?
Kubernetes와 Mesos + Marathon에 대한 관심도를 살펴보면 Kubernetes가 뉴스 기사, 웹 검색, 출판물 및 Github 대상 모든 측정 항목에서 70% 이상을 차지하고 있음을 알 수 있습니다.

![image](https://user-images.githubusercontent.com/111643/115837222-4ada6680-a453-11eb-94e9-eb7d618b4a67.png)

Kubernetes는 Mesos + Marathon에 비해서 이점을 제공합니다.
* 폭넓은 DevOps 및 Container 커뮤니티
* 복잡한 어플리케이션 스택에 사용하기 유리하며 더 발전된 Scheduling option을 제공
* Google에서 10년 이상의 경험을 바탕으로 제작됨

결론,

돈이 많고, 기술 내재화 하기도 싫고, 문제 발생시 책임 소재도 따지고 싶으면 Mesos + Marathon 을 선택, 그렇지 않다면 Kubernetes로 가는 것이 현실적으로 보여집니다.

다음 포스팅에서는 Kubernetes와 Amazon ECS를 비교해보도록 하겠습니다.

참고 자료:
* https://thenewstack.io/a-brief-comparison-of-mesos-and-kubernetes/
* https://platform9.com/blog/kubernetes-vs-mesos-marathon/
* https://www.stratoscale.com/blog/kubernetes/kubernetes-vs-mesos-architects-perspective/
