---
layout: post
title:  "HashiCorp사의 Consul, Consul Template 소개"
comments: true
tags: Hashicorp, Consul
---

HashiCorp에서 제공하는 Consul은 Service Discovery, Failure Detection, Multi Datacenter, KV Storage 기능을 제공합니다. HAproxy 혹은 Nginx와 같은 Software 기반의 Load Balancer를 Cloud 상에 구축한다고 가정해보면, 고려해야 할 사항은 아래와 같습니다.
* 부하량에 따라 LB가 Scale In/Out이 되어야 한다.
* 로드 밸런싱 대상이 되는 Backend 서비스가 Scale In/Out이 되면 LB의 Configuration이 실시간으로 변경되어야 하며, 각 LB에게 동기화가 되어야 한다.
* LB의 Health check가 수행되어야 한다.

이 정도로만 간추려봐도 LB하나를 도입하기 위해 해야할 일이 많아지게 됩니다. 분산 환경에서의 이러한 일을 처리하기 위해 예전에는 Apache Zookeeper를 이용하여 개발을 했었지만 이번에는 Consul을 이용해보기로 했습니다.

Apache Zookeeper와 HashiCorp의 Consul의 차이점은 아래와 같습니다.

# Apache Zookeeper
* Written in Java
* Strongly consistent (CP)
* Zab protocol (Paxos like)
* Ensemble of servers
* Quorum needed (majority)
* Dataset must fit in memory

Zookeeper는 현재 많은 곳에서 활용되고 있지만, 몇가지 문제점을 지니고 있습니다.

## Zookeeper : Problems
* Latency-dependent
* No multi datacenters support (very slow write)
* Client number limit (out of sockets)
* Designed to store data on the order of KB in size
* Possibly lost changes while reenabling watch
* TCP connection != service healthy (proxy)
* Change zk server is a problem

이러한 문제점들을 보완해서 나온 제품이 Consul입니다. Consul의 특징을 보시죠.

# HashiCorp Consul
* Written in Go
* Service discovery
* Health checking
* Key / Value Store
* Multi datacenter support
* Agent / Server Concept
* Gossip protocol for all the nodes (discovery)
* Consensus protocol (Raft-based) for servers
* 3 consistency modes
* Access Control List (ACL)

Consul에서 제공하는 기능을 HAProxy에 적용을 해봤고 결과는 만족스럽습니다.

HAProxy는 GSLB or DNS RR로 상위 Client에서 접속을 시도하게 되게되고 이 경우 HAProxy를 HA구성(Active-Standby)을 해야 합니다.

HAProxy가 설치된 VM에 consul agent를 등록하고 별도의 config 파일을 만들어 HAProxy에 대해서 Health checking을 수행 합니다. LB 매커니즘이 Round Robin일 경우 HTTP로 health check를 하게되면 LB가 문제가 될 소지가 많기에, 저의 경우에는 별도의 script를 만들어서 health checking을 수행했습니다.

haproxy.cfg 파일을 중앙에서 관리하기 위해 haproxy.cfg.ctmpl 파일을 만들어 consul-template daemon을 구동합니다. haproxy.cfg.ctmpl내에 별도의 key를 만들어 이 정보를 Consul의 K/V store에서 관리 및 변경시 모든 node에 즉시 동기화가 됩니다.

이렇게 함으로써 HAProxy를 도입할때의 고려사항 중 일부가 해소가 되었습니다.

앞으로 남은 부분은 consul-ui에서 확인할 수 있는 alarm 및 기타 정보를 push 형태로 IaaS Management 쪽에 전달하는 부분만 보완하면 될 것 같습니다. consul-ui는 어디까지나 확인 용도이고 IaaS management tool과 Integration이 되어야 하기 때문입니다.

그동안 Zookeeper를 쓰면서 client number limit, TCP connection, store data design 부분에 대해서 고민이 있었는데, consul을 활용하면서 이런 부분이 어느정도 해소가 될 것으로 보여집니다.

관련하여 좋은 Tip이 있으면 같이 공유했으면 합니다.
