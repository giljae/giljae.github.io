---
title:  "Kubernetes란 무엇인가?"
tags: kubernetes, k8s
---

![image](https://user-images.githubusercontent.com/111643/115670920-e9e45d00-a384-11eb-9aed-16bfdce0a782.png)

Kubernetes는 구글에서 공개한 리눅스 컨테이너 관리 시스템입니다. 일반적으로 Docker Orchestrator라고 표현합니다. 얼마전 우리 회사에서 Kubernetes를 활용한 프로젝트가 있어서, 궁금하기에 조사해봤습니다. Docker를 이용하는 Machine이 Single일 경우에는 Orchestration에 대해서 고민할 필요가 없습니다. Docker client의 command를 사용하여 Single host에 대해서만 관리를 하면 되기 때문이죠. 저도 개인적으로는 이렇게 사용하고 있습니다.

하지만, 두대 이상의 Host상에서 Docker가 존재할 경우에는 얘기가 달라집니다. 사용자가 요청한 Docker 컨테이너를 어느 Host에 설치해야할까? 라는 고민이 시작됩니다. 결국, Manual하게 Resource에 여유가 있는 Host를 선택해서 컨테이너를 생성하게 됩니다.

만약에 Host가 50대 혹은 1000대라면 어떻게 해야 할까요? Manual하게 관리하기가 쉽지 않겠지요? 어느 Host에 어떤 Docker container가 있는지, LB는 어떻게 해야 할지, 이미지 동기화 처리는 어떻게 해야 하는지.., 등등의 고민을 해야 하고 이런 작업들이 Manual하게 이뤄지게 됩니다. 결국, 관리하는 사람은 단순 노동에 지쳐서… 이직을 결심할지도 모릅니다.

Kubernetes는 이런 힘든 작업을 누군가가 대신해주면 어떨까? 라는 고민에서 시작되었습니다. 물론 Kubernetes말고도 이런 프로젝트는 여러개가 존재합니다. (Swarm, Mesos ..,)

Kubernetes는 Multiple Host에서의 Docker Container를 관리하는 Container Orchestration이고 특징은 아래와 같습니다.

# 특징
* Multiple Docker Pool
  * 사용자는 Docker Daemon의 대수를 알 필요가 없고, “N개의 Container가 필요하니 A이미지로 생성해줄래?”라는 명령만 내리면 Kubernetes가 Multiple Docker Daemon에 알아서 Container를 생성해줍니다.
* 다른 서버 Container와의 Interface
  * Docker Container는 Docker Daemon이 있는 Host내의 Private IP를 가지고 있는데, A 호스트의 Container와 B 호스트의 Container가 서로 통신을 하기가 어렵습니다.
  * 위의 문제점을 Kube-proxy, flanneId의 sub tool로 가능하게 해줍니다.
* Container fail 처리
  * Container가 fail 상태일 경우, 동일한 Container를 생성해서 지속적인 서비스가 가능하게 합니다.
* Load Balance
  * Round-robin 형태의 로드밸런싱을 제공하여 Cluster로 묶인 Container에 접근이 가능하게 해줍니다.

# 주요 구성 요소
## Cluster
* Application Container들이 배치되고 실행되는 컴퓨팅 자원

## Pod
* Application 실행에 필요한 Container 집합
* Lifecycle이 같은 Container 그룹
* Deploy 및 실행의 최소단위

## Replication Controller
* pod들의 Lifecycle 관리자
* pod들의 health status를 체크

## Service
* pod 집합을 대표하는 이름 (e.g. IP address)
* 로드밸런서

## Label
* pode들의 Grouping/조직화를 위해 관리하는 key-value 메타 데이터


보다, 자세한 사항은 아래를 참조

http://kubernetes.io

