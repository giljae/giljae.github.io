---
layout: post
title:  "Kubernetes vs. Mesos: 어느 Container Orchestrator를 사용해야 할까? (사업 관점)"
comments: true
tags: Kubernetes, Mesos
---

다른 용도로 사용되는 10개의 컨테이너가 있다고 가정한다면, 많은 인스턴스를 사용하고 이러한 컨테이너를 실행하는 것은 매우 쉽습니다.

어플리케이션이 커지면서 배포한 컨테이너 수가 점점 증가하면 서로 다른 버전, 네트워크 구성을 가진 수천 개의 컨테이너를 관리할 때는 상황이 애매해집니다.

컨테이너에 의존하는 개발 기술을 사용하는 회사의 경우, 이러한 유형의 아키텍처를 확장하는 문제가 발생합니다. 오케스트레이션 인프라 스트럭처의 요점은 컨테이너를 “스케줄링”하는 간단한 방법을 제공하고 기본 인프라가 나머지를 수행하도록 하는 것입니다. 이것이 오케트스레이션의 핵심입니다.

Kubernetes, Mesos, ECS, Swarm 그리고 Nomad와 같이 5개의 툴이 존재합니다. 이 도구중 어떤 도구를 사용할지에 대한 결정은 회사 및 개인적 취향에 따라 달라집니다. Logz.io 의 경우 두개의 플랫폼으로 추렸다고 합니다.
* Swarm: 테스트에는 적합하지만, 상용에서 사용하기에는 부적합하다고 판단
* Amazon ECS: 클라우드에 무관심한 도구를 원했기에 ECS는 부적합하다고 판단
* Nomad: 아직 사용하기에는 성숙하지 않다고 판단, 시간이 지나면 괜찮을지도…

위 이유로 Kubernetes와 Mesos를 선정했다고 합니다. Mesos는 Apache 프로젝트로 컨테이너/비컨테이너 방식을 관리할 수 있습니다.

처음에는 Berkeley 연구 프로젝트로 사용되었고 향후 Twitter에서 사용하고 있습니다. Marathon 플러그인을 제공하여 컨테이너를 쉽게 관리할 수 있는 방법을 제공합니다.

2016년에 Mesosphere가 지원하는 오픈 소스 프로젝트인 DC/OS가 도입되었고, Mesos를 더욱 단순화 하였습니다. Kubernetes는 2014년에 Google에서 출시한 플랫폼이며 Cloud Native Computing Foundation에 기여했습니다.

API에 대한 Marathon의 접근 방식은 Kubernetes와 비교하면 간단합니다. Marathon은 Kubernetes에 비해 적은 API 리소스를 제공합니다. 두 플랫폼이 누리는 인기도에도 명백한 차이가 존재합니다.

#Kubernetes
![image](https://user-images.githubusercontent.com/111643/115820474-dd6f0b80-a43b-11eb-86c3-ceff53e7acd3.png)

#Mesos
![image](https://user-images.githubusercontent.com/111643/115820499-e6f87380-a43b-11eb-8234-05f44da1b3aa.png)

위의 그림은 Kubernetes와 Mesos의 commit, contribute를 보여주고 있습니다. 압도적으로 Kubernetes의 커뮤니티 규모가 큽니다.

Logz.io에서는 처음에는 DC/OS(Mesos)를 선택했었고, Kubernetes의 강점 중 일부를 포기할 준비를 했다고 합니다. 하지만 배포 프로세스를 자동화하는데 필요한 간단한 기능이 엔터프라이즈 버전에만 포함되어 있다는 사실을 알았다고 합니다.

Mesosphere에 문의 했었고, 이런 상황이 일시적인 것이 아닐 수 있다는 것을 깨달았다고 합니다.

DC/OS는 이 특수한 상황을 극복해도 결국 상업 회사에 통제되기에 Kubernetes로 작업을 시작했다고 합니다.

그후 Kubernetes기반으로 작업을 완료 했고, 이 변경으로 인해 새로운 코드를 하루에 여러번 배포하는 개발자들에게 모든 책임을 위임할 수 있었고, 보다 효율적이고 지속적인 배포가 가능했다고 합니다. 다음 포스팅에는 Kubernetes와 Mesos의 기술적 비교를 다룰 예정입니다. 

References:
* https://dzone.com/articles/kubernetes-vs-mesos-choosing-a-container-orchestra
* https://platform9.com/blog/kubernetes-vs-mesos-marathon/
