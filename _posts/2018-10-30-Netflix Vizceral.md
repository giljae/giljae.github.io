---
title:  "Netflix Vizceral"
tags: Netflix, Vizceral
---
Vizceral은 Netflix Control Plain으로 유입되는 트래픽 상태에 대한 정보를 이해하는 방식을 변화 시켰다고 합니다. Netflix의 경우 전체 시스템의 상태에 기반한 의사결정을 내리기를 원했고 이를 위해서 전체 시스템의 상태에 대해 직관적으로 이해할 수 있는 도구가 필요했습니다. Netflix의 경우 데이터 구문 분석에 의존하는 대신 직관적인 방법을 적용하기로 했습니다. 장애로 인해 수백만명이 영향을 받는 시간을 최소화 하는 방안을 고려했고 이를 Intuition Engineering이라고 부르며 Vizceral이 그 대표적인 예입니다.

아래의 영상은 지역 간 트래픽 이동시 전체적인 모습을 시뮬레이션한 모습입니다.

<iframe width="665" height="510" src="https://www.youtube.com/embed/KVbTjlZ0sfE" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Netflix의 트래픽 팀에서는 Intuition Engineering의 중요성을 입증 한 후 다양한 의견을 통해서 사회에 공헌 차원에서 오픈 소스 프로젝트로 유지 관리해야 한다는 결정을 했습니다. 그들이 공개한 소스는 아래와 같습니다.
* vizceral: 그래프 데이터를 보고 상호 작용할 수 있는 기본 UI 구성 요소
* vizceral-react: 시각화를 쉽게 할수 있는 Wrapper
* vizceral-component: 웹 구성 요소를 사용하여 시각화를 돕는 Web Component Wrapper
* vizceral-example: 예제 프로젝트

이외에 Netflix는 내부적으로 Atlas 및 Salp로 부터 데이터를 수집하는 서비스를 제공하고 있고 이 서비스는 vizceral 구성 요소에 필요한 형식으로 데이터를 변환하고 웹 소켓을 이용해 UI를 업데이트 합니다.

아래는 특정 지역에 대한 세부 트래픽을 보여주는 영상입니다.

<iframe width="665" height="510" src="https://www.youtube.com/embed/MYHf_BXWuOc" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

특정 지역을 클릭하면 해당 지역에서 운영되는 마이크로 서비스의 확대 보기가 나타납니다. 보기 좋게 하기 위해 노드간 연결을 단일 차선으로 최소화 하였고 황색과 빨간색 점이 서비스간에 오류 응답을 표시합니다.

서비스를 더 자세히 보고 싶으면 노드위에 마우스를 오버하여 입력 및 출력을 표시할 수 있습니다.

![image](https://user-images.githubusercontent.com/111643/116032207-35a34900-a69a-11eb-9571-a30a8e87a760.png)

노드를 클릭하면 컨텍스트 패널이 보여지며 관련 정보를 입력 할 수 있습니다.

![image](https://user-images.githubusercontent.com/111643/116032224-3d62ed80-a69a-11eb-9c84-3985fdd81839.png)

vizceral을 이용하는 방법은 vizceral-example 프로젝트의 지침을 따르는 것이 가장 빠릅니다.

Source: https://medium.com/netflix-techblog/vizceral-open-source-acc0c32113fe
