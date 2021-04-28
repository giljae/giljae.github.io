---
layout: post
title:  "Industrial Internet of Things"
comments: true
tags: IIoT
---
IoT(Internet of Things)가 B2C영역의 일반 소비자 중심이라면 IIoT(Industrial Internet of Things)는 B2B영역에 속하는 산업용 IoT라고 할 수 있다.

B2C영역의 IoT는 웨어러블 피트니스, 스마트홈, 무인 자동차와 같이 최종 소비자에게 편의성을 제공하는 기기들이 떠오르며 해당 유즈케이스가 아직까지는 우리 생활에 크게 와닿지 않는 부분들이 존재한다.

반면, B2B영역의 IIoT는 스마트 도시, 스마트 농업, 스마트 공장, 스마트 그리드등 산업계내에서 유용한 혁신들이 일어나고 있다. IIoT의 핵심은 다수의 산업 시스템들이 서로 연결되어 여기서 발생하는 활동 데이터를 분석함으로써 성능과 효율을 개선하는데에 목적을 가지고 있다.

IIoT라는 개념을 가장 먼저 도입한 기업은 GE이다. GE는 가전제품, 의료기기 및 항공기 엔진등을 생산하는 세계적인 제조업체이며 이 플랫폼을 통해 자사의 관리자산인 센서에서 유입되는 대용량 데이터를 수집, 저장, 분석, 모니터링을 하고 있고 다른 비즈니스 파트너들에게 공개하였다.

![image](https://user-images.githubusercontent.com/111643/115669130-e94ac700-a382-11eb-8928-76ec162adc01.png)

GE의 Predix는 IIoT를 세 계층으로 구분하고 있다.
* Connectivity : 자동화 공정에 존재하는 수 많은 센서, 액츄에이터 및 디바이스와 같은 물리적 기기들을 담당하는 계층. 데이터 발생의 근원지
* Cloud Services : 디바이스에서 생성되는 데이터의 통합, 분석, 처리 역할을 담당하는 계층. Predix는 Cloud Foundry와 같은 PaaS 기반의 Cloud Service를 제공
* Applications : 데이터에 대한 시각화를 담당하는 계층

앞으로 다가오는 시대는 우리가 누리고 있는 전기나 수도처럼 사물이 연결될 것으로 보인다. 다음에는 GE의 Predix에 대해서 자세히 기술하고자 한다.
