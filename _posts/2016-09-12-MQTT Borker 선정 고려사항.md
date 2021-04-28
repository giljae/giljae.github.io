---
title:  "MQTT Borker 선정 고려사항"
tags: mqtt, broker
---

MQTT Broker를 선정시 고려할 사항을 정리합니다. MQTT는 서비스 품질(QoS)에 대해서 3가지 레벨의 신뢰성을 제공합니다. 일반적으로는 QoS 0를 선택하고, Application Level에서 처리하고 있습니다.

![image](https://user-images.githubusercontent.com/111643/115679316-68dd9380-a38d-11eb-80cb-5d5ff48a015b.png)

1. QoS Level 0 (최대 한번): 기본 전송 모드, 가장 빠른 모드
2. QoS Level 1 (최소 한번): 중복 메시지가 전달 될 수 있음
3. QoS Level 2 (정확히 한번): 가장 느린 모드

MQTT 보안에는 크게 Authentication, Authorization 부분과 Network 그리고 Payload 검증 부분으로 나눠집니다.

![image](https://user-images.githubusercontent.com/111643/115679360-75fa8280-a38d-11eb-99d2-325a305d7acd.png)

위의 3가지 요소중에 Clustering 방식을 가장 선호하며 MQTT Cluster를 구성하는 목적은 아래와 같습니다.

MQTT Broker를 사용하는 Subscriber의 Backend server가 같은 기능을 갖는 여러대로 구성되는 Case가 존재합니다. 이럴 경우 Subscriber Server 앞단에 HAProxy를 이용해서 한대의 서버만 message를 받도록 할 것인지, 아니면 Broker에서 제공하는 Exclusive 기능을 이용할 것인지 고려해야 합니다.

![image](https://user-images.githubusercontent.com/111643/115679387-80b51780-a38d-11eb-9134-135127bfe54a.png)

그 외에, Integration 부분도 고려해야 합니다.
1. Authorization Service
2. Processing Applications
3. Persistent Storage

