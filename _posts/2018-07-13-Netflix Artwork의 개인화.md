---
title:  "Netflix Artwork의 개인화"
tags: Netflix, Artwork, Personalization
---
본글은 [“Artwork Personalization at Netflix”](https://medium.com/netflix-techblog/artwork-personalization-c589f074ad76)의 글의 내용중 일부를 발췌해서 작성했습니다.

Netflix 추천 시스템의 주요 목표는 사용자에게 맞는 콘텐츠를 추천하는 것이었습니다. 사용자가 콘텐츠에 흥미를 가지고 해당 콘텐츠가 가치가 있는지를 확인하는 방법이 무엇일지 많은 고민을 했습니다. 콘텐츠의 포스터가 관문 역할을 하며 사용자에게 콘텐츠에 대한 매력을 어필할 수 있다고 판단했습니다.

![image](https://user-images.githubusercontent.com/111643/116030977-bf9de280-a697-11eb-9fe4-999a56f6b416.png)

구구절절한 설명보다 단 한장의 이미지로 함축적인 내용을 전달 할 수 있습니다. Netflix는 이점에 착안하여 Artwork의 개인화를 시스템에 반영했습니다.

“Good Will Hunting”이라는 영화를 묘사할 때 사용하는 이미지를 개인화하려고 시도해봅시다. 로맨틱한 영화를 즐겨본 사용자는 Matt Damon과 Minnie Driver가 포함된 포스터를 보여주면 “Good Will Hunting”에 관심이 있는 반면, Robin Williams가 포함된 포스터를 사용하면 코미디 영화를 좋아하는 사용자가 영화에 관심이 있을 수 있습니다.

![image](https://user-images.githubusercontent.com/111643/116031019-d3e1df80-a697-11eb-8fa2-6b14aa93a88c.png)

또 다른 시나리오를 생각해봅시다. Pulp Fiction의 포스터의 경우, 우마서먼이 출연한 영화를 많이 본 사용자(바로 저)는 우마서먼이 나오는 포스터에 호감을 가지게 될 것입니다. 한편 존 트라볼타의 팬은 존 트라볼타가 나오는 포스터에 더 관심이 있을 수 있습니다.

![image](https://user-images.githubusercontent.com/111643/116031056-e2c89200-a697-11eb-909a-0d268c8f7338.png)

Netflix는 포스터를 개인화함으로써 모든 사용자가 최상의 선택을 할 수 있도록 사용자 경험을 향상 시킵니다.

포스터를 개인화하는 방법을 알기 위해서는 한 콘텐츠에 대해 어떤 포스터가 더 효과가 있다라는 신호를 찾기 위해 많은 데이터를 수집해야 합니다.

효과적인 개인화를 달성하려면 각 콘텐츠에 대해 훌륭한 포스터 풀이 필요합니다. clickbait(독자가 흥미롭지 않거나, 가치가 떨어지는 콘텐츠의 링크를 클릭하도록 유도하는 행위)를 피하기 위해 매력적이면서 유익한 포스터 자산이 필요하다는 것을 의미합니다. Netflix의 아티스트 및 디자이너팀은 다양한 차원에서 다양한 이미지를 만들기 위해 노력을 기울입니다.

포스터를 개인별로 제공하기 위해서는 엔지니어링 관점에서 Challenge가 필요합니다. 사용자의 시각적 경험을 위해 많은 이미지를 포함한다는 점입니다. 각 콘텐츠에 대해 개인별 맞춤형 포스터를 제공하면 초당 2천만건의 요청을 처리할 수 있는 견고한 시스템이 필요합니다. UI에서 포스터를 제대로 렌더링 하지 못하면 사용자의 경험이 크게 저하됩니다. 사용자의 취향이 변함에 따라 포스터의 효과가 시간이 지나면서 바뀌게되므로 알고리즘이 지속적으로 대응해야 합니다.

Netflix의 추천엔진 대부분은 기계학습에 의해 동작됩니다. 사용자의 서비스 사용에 대한 데이터를 수집하고 그 데이터를 기반으로 알고리즘을 실행합니다.

그리고 A/B 테스트를 통해 새로운 알고리즘을 테스트 합니다. A/B 테스트는 새로운 알고리즘이 무작위로 구성된 사용자를 대상으로 진행하며, 현재 프로덕션 시스템보다 우수한지를 확인합니다.

그룹B의 사용자가 새로운 알고리즘 환경에 접속하는 동안 그룹A의 사용자는 현재의 프로덕션 환경으로 접속합니다. 그룹B의 사용자가 참여도가 높으면 새로운 알고리즘을 전체 사용자에게 배포합니다. 그러나 불행하게도 이러한 접근법은 실패했습니다. 오랜동안 많은 사용자가 더 나은 경험을 얻지 못했습니다.

![image](https://user-images.githubusercontent.com/111643/116031079-f07e1780-a697-11eb-85bf-447cf3ad6f1b.png)

Netflix는 이 실패를 해결하기 위해 Batch성 기계학습에서 벗어나 실시간 기계학습을 고려했습니다. Netflix가 사용하는 학습 알고리즘은 Contextual bandits입니다. (참조: https://medium.com/netflix-techblog/artwork-personalization-c589f074ad76)

이러한 접근 방식을 통해 Netflix는 사용자가 새로운 콘텐츠를 선택하는 방법에 대해 의미있는 개선을 했습니다. 대단합니다.

콘텐츠의 첫 관문인 포스터는 정말 중요합니다.

![image](https://user-images.githubusercontent.com/111643/116031130-01c72400-a698-11eb-9253-f8dc96b0e36d.png)

성인은 첫번째, 어린아이들은 두번째 포스터를 선택하지 않을까요? 개인화 Artwork를 통해 사용자 경험을 증대시키는 Netflix에게 박수를 보냅니다.
