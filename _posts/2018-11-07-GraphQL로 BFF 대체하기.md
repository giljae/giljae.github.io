---
title:  "GraphQL로 BFF 대체하기"
tags: GraphQL, BFF
---
![image](https://user-images.githubusercontent.com/111643/116032404-95015900-a69a-11eb-9299-6d29885c6ef8.png)

위의 그림에서 BFF의 목적은 Orchestration, Business Logic을 공유하고 Backend 서비스가 제공하는 것보다 UI에 친화적인 모델을 제공하는 것입니다. 그래서 각 클라이언트별로 BFF가 존재하게 됩니다. Netflix는 Client Adapter라는 이름으로 SoundCloud는 BFF라는 이름으로 UI에 친화적인 Backend 서비스를 제공하고 있는데, 이런 BFF에도 문제점이 존재합니다.
* 업무 조직간 교차 관리가 어렵습니다.
* Traffic에 대한 용량 사이징을 예측하기 어렵습니다.
* 단일 실패 지점이 될 가능성이 존재합니다.
* 추가적인 아키텍처 복잡성이 발생합니다.

![image](https://user-images.githubusercontent.com/111643/116032420-9f235780-a69a-11eb-8526-28de7cbfbdc3.png)

다른 접근 방법은 클라이언트가 서비스를 제공하거나 Backend와 직접 상호 작용을 하는 것입니다.

![image](https://user-images.githubusercontent.com/111643/116032433-a6e2fc00-a69a-11eb-85f3-a1140a578983.png)

위의 그림처럼 BFF를 걷어내면 심플해보이는 장점이 있지만, 여전히 BFF와 공통된 문제점이 존재합니다. Application Server는 PC, Mobile의 데이터 요구를 구별해야 하며 각 클라이언트별로 API가 달라질 가능성이 존재합니다.

# GraphQL을 써보자
이러한 상호 작용을 깔끔하게 하고 최종 사용자에게 더 나은 서비스를 제공하기 위해서 GraphQL을 사용해 볼 계획을 가지고 있습니다. GraphQL은 API Graph에서 구성 요소를 관리하는 복잡성에 대처할 수 있도록 Facebook에서 개발했습니다.

GraphQL을 사용하면 UI가 데이터와 선언적으로 상호 작용할 수 있으며 서버에서 UI를 분리 할 수 있습니다. UI에서 필요한 데이터를 지정하기에 UI에 친화적인 API를 구축하는 것에 대해 고민할 필요가 없습니다. 또한 GraphQL은 일반적인 AJAX 요청보다 더 쉽게 최적화되기에 Request 개수가 줄어들게 됩니다.

![image](https://user-images.githubusercontent.com/111643/116032455-b3675480-a69a-11eb-9b06-2fae69a15e04.png)

앞으로는 GraphQL이 주류가 될 것입니다. Amazon의 [AppSync](https://aws.amazon.com/appsync/) 혹은 [Graphcool](https://www.graph.cool/) 같은 서비스가 주류가 되는데에 일조 할 것으로 보여집니다.
