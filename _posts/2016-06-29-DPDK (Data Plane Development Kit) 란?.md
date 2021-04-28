---
layout: post
title:  "DPDK (Data Plane Development Kit) 란?"
comments: true
tags: DPDK
---

오늘날 네트워크 기술은 널리 활용되고 있고, 전용 네트워크 어플라이언스 장비들을 가상화 네트워크에서 동작 시키기 위한 NFV(Network Function Virtualization) 기술이 각광을 받고 있습니다. NFV는 기존 장비들을 동일한 성능에서 지원한다라는 전제 조건이 있습니다. 일반적으로 우리가 Native환경에서 어떤 프로그램을 사용하는 것과 가상화 환경에서 사용할 경우에는 Performance의 Gap이 존재하게 됩니다.

DPDK는 Intel에서 개발한 고성능 패킷 처리 소프트웨어로 고속 패킷 처리를 위한 라이브러리와 드라이버를 제공하고 NFV의 네트워크 성능을 높이기 위한 핵심 기술입니다.

![image](https://user-images.githubusercontent.com/111643/115676664-c7edd900-a38a-11eb-8857-b1f448d26002.png)

DPDK의 특징:
* 리눅스 or 윈도우의 Kernel 대신에 네트워크 패킷을 처리하는 응용 소프트웨어를 제공하고 전용 CPU core를 할당하여 네트워크 카드의 패킷을 Kernel을 거치지 않고 직접 처리
* 오픈 소스로 제공

기존 Kernel에서 네트워크 패킷을 처리하는 방식과는 다르게 응용 어플리케이션이 User Space의 DPDK 라이브러리를 사용하여 직접 엑세스한다는 점이 특징입니다.

즉, 응용 어플리케이션을 특정 CPU core에 할당하여 가장 높은 우선 순위로 실행하고, Kernel을 거치지 않고 직접 네트워크 카드의 패킷을 처리하게 되면 네트워크 성능을 비약적으로 향상 시킬 수 있습니다.

![image](https://user-images.githubusercontent.com/111643/115676742-d89e4f00-a38a-11eb-8615-54b72ba85ea3.png)

이런 장점을 갖고 있는 DPDK도 단점은 존재합니다.
* DPDK를 지원하는 네트워크 카드가 한정되어 있다라는 점 (http://dpdk.org 참조)
* 응용 어플리케이션에서 해야 할 부분이 많아지는 점
* 네트워크 패킷을 직접 처리
* 네트워크 패킷 헤더를 보고 TCP/IP와 같은 프로토콜 판별부터 동작하는 기능까지 구현해야 함

DPDK를 이용하는 응용 어플리케이션을 적용할 경우에는 아래의 사항을 고려해야 합니다.
* 응용 어플리케이션이 직접 가상 NIC에 접근
* Guest OS의 Kernel을 skip하고 응용 어플리케이션에서 직접 가상 NIC를 acess하여 개발 (가상 NIC와 물리 NIC 사이의 패킷처리는 Hyper 바이저가 제공하는 가상 스위치를 이용하게 됨으로 Over-head는 기존과 다르지 않음)
* 응용 어플리케이션이 가상 OS의 DPDK를 이용하여 가상 NIC에 접근
* Guest OS의 DPDK 지원 프로그램에서 물리 NIC를 직접 조작하는 방법이며, 이 경우 Hyper 바이저가 특정 물리 NIC를 가상 머신까지 통과시킵니다. Hyper 바이저가 제공하는 가상 스위치의 기능은 이용할 수 없기에 멀티플 가상 시스템에서 물리 NIC를 공유하고자 한다면 SR-IOV를 지원하는 물리 NIC가 필요하게 됩니다. (SR-IOV는 하나의 물리 NIC를 가상으로 여러개의 NIC로 보여주는 기능을 의미)
* Host의 가상 NIC를 DPDK가 사용
* vSwitch에 DPDK를 사용하여 vSwitch와 물리 NIC사이의 패킷 처리 속도를 향상

