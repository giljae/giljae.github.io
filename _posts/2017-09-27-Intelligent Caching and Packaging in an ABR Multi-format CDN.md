---
layout: post
title:  "Intelligent Caching and Packaging in an ABR Multi-format CDN"
comments: true
tags: Netflix, CDN
---

2012년에 나왔던 자료인거 같은데…
이전에 작성한 “Netflix, 그들의 콘텐츠 서비스 방식(Netflix and Fill)” 과 연관이 있어 보인다.

![image](https://user-images.githubusercontent.com/111643/115819648-4190d000-a43a-11eb-9152-e477411320e7.png)

VOD를 처음 시작할때는 Caching이 필요하지 않았다. VOD가 초기 단계에 있었을 때 하나의 중앙 서버가 전체 노드를 지원했었고 콘텐츠는 SD 포맷으로만 제공 되었다. 한곳에서 콘텐츠를 관리했기에 Caching이 필요하지 않았다.

![image](https://user-images.githubusercontent.com/111643/115819664-4bb2ce80-a43a-11eb-86c6-1b1e5fc99433.png)

VOD가 많아짐에 따라 Caching이 필요하게 되었다. LRU는 지능형 Caching으로 진화했다.

![image](https://user-images.githubusercontent.com/111643/115819677-540b0980-a43a-11eb-88fd-64699c43a633.png)

Caching은 미래 행위를 예측하기 위해 과거 행위를 이용하려는 예측 활동이다. Caching은 삭제해야 할 콘텐츠를 결정하는 것이 중요하다. LRU는 사용 패턴에 관계없이 가장 오래동안 사용하지 않은 콘텐츠를 제거한다.
Intelligent Caching은 전체 시청 시간을 기반으로 추가 조회 가능성에 대한 통계적 추론 기법을 가미한다.

![image](https://user-images.githubusercontent.com/111643/115819687-5e2d0800-a43a-11eb-8b66-f72e8b59ad93.png)

이 장표를 소개할 시점의 CDN Caching 알고리즘의 주류는 LRU

![image](https://user-images.githubusercontent.com/111643/115819699-66854300-a43a-11eb-871a-2156e9adbc4f.png)

예측 가능성을 향상 시키기 위해서는 동일한 콘텐츠에 대한 모든 스트림 요청이 동일한 edge streamer로 전달되어야 하며, Cluster Manager를 사용하여 콘텐츠 요청 경로를 지정해야 한다. Netflix의 경우 Control Plain으로 보면 될듯

![image](https://user-images.githubusercontent.com/111643/115819720-6f761480-a43a-11eb-9537-ab96399ec6c3.png)

동일한 Cache에서 같은 콘텐츠를 스트리밍하면 count가 증가하고 네트워크 사용률이 감소한다. 이렇게 되려면 Cluster Manager가 Edge를 Referring 해야 한다.

![image](https://user-images.githubusercontent.com/111643/115819745-79981300-a43a-11eb-817b-c1738feb7aba.png)

LRU caching은 콘텐츠 및 콘텐츠 위치, usage에 대한 정보가 없다. 즉, 가장 최근에 사용된 콘텐츠를 결정하고 나머지는 제거한다.

Intelligent caching은 Edge에 콘텐츠를 잘 갖다 놓고 여러 매개 변수를 사용하여 Edge에 저장된 Chunk의 실제 가치를 평가하기에 보다 효율적으로 동작한다.

![image](https://user-images.githubusercontent.com/111643/115819777-87e62f00-a43a-11eb-93d9-2bfa0c441841.png)

Caching은 패키징 옵션의 영향을 받는다는 의미인데, 어떤 방식으로 패키징하고 Origin에 어떤 모양으로 담겨 있는지에 따라 Caching에 영향을 준다는 의미인듯…

Source: https://www.slideshare.net/briantarbox/2012-ncta-motorolaintelligent-caching-and-packaging-in-an-abr-multiformat-cdn-tarboxfinal
