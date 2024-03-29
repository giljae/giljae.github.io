---
title:  "개발자 경험(Developer Experience)과 사용자 경험(User Experience)"
tags: DX, UX
---
서비스를 기획할 때는 UX(User Experience)관점에서 진행되는 경우가 많다. 오늘은 DX(Developer Experience)에 대해서 언급하고자 한다.

내가 하는 일이 개발자를 위한 제품을 만드는 것은 아니지만, 내부적으로 개발을 하게되면 어느 순간 다른 개발자가 조인하여 구현된 코드를 인계받아 업무를 진행하는 사례도 많기 때문에 DX 관점에서 개발을 진행할 필요성이 있어 보였다.

둘은 비슷해 보일 수 있다. DX에 대해서 정의하자면 “개발자를 위한 사용자 경험"이 아니다. DX는 기술 언어 및 도구를 이용해 사용자에 초점을 맞춘 UX의 확장이다. DX도 UX의 핵심 원칙을 따르지만 개발자가 기술 세부 사항과 프로세스를 효율적으로 이해하고 활용할 수 있게 한다는 점을 확장 시킨 개념이다.

UX는 비기술적이며 비즈니스 요구를 최종 사용자에게 적용하는데 중점을 둔다. 반면, DX는 개발자가 소프트웨어를 사용하여 솔루션을 구축할 수 있도록 하는데 중점을 둔다. 최종 사용자와 개발자의 요구 사항이 다르기 때문에 둘 간 차이가 발생한다.

# DX는 UX의 원칙에서 시작된다.
훌륭한 UX는 제품을 쉽게 사용할 수 있게 만들고 계속 즐겁게 사용할 수 있도록 해야 한다. DX는 기술 관점에서 UX와 동일한 핵심 아이디어로 출발한다.

가능한 쉽게 제품을 사용하도록 해야 한다. 제품은 직관적이어야 하고 최종 사용자나 개발자는 최소한의 지침으로 제품 사용을 시작하고 목표를 달성 할 수 있어야 한다. 

UX와 DX의 주요 차이점은 사용자 여정이다. 좋은 UX는 제품에 대한 사용자 여정을 가능한 간단하게 유지한다. 너무 많은 선택은 사용자에게 부담이 될 수 있고, 이로 인해 제품에서 가치를 얻는 것이 더 어려워질 수 있기 때문이다. 

좋은 DX는 개발자의 목표를 가능하게 하기 위해 많은 유연성이 필요하다. 사용자 관점에서는 제품을 잘 사용하기 위해 기술에 대한 세부 정보가 필요하지 않지만, 개발자는 요구 사항을 평가할 수 있도록 기술 세부 정보에 접근 할 수 있어야 한다. 좋은 DX를 사용하면 개발자가 제품이나 서비스가 어떻게 작동하는지 쉽게 이해할 수 있으므로 성공적으로 구축할 수 있다.

좋은 UX는 사용자가 제품에 동등하게 접근할 수 있도록 하는 반면, DX는 다양한 개발자의 기술 능력을 고려하고 다양한 수준의 지원을 제공해야 한다. 경험이 많은 개발자는 더 강력한 도구에 접근하길 원하지만 경험이 적은 개발자는 구성하기 쉬울 수 있는 간단한 도구로 시작해야 할 수 있기 때문이다. 개발자 경험이 제품에 잘 맞도록 하려면 기본 사용자의 기술적 능력 및 역량을 이해하는 것이 중요하다.

# UX vs DX 사례
이 글을 읽는 대부분 사람들은 서비스 앱의 사용자였을 것이다. 쇼핑앱을 이용한다고 가정해보자. 검색창에 “가방"이라는 검색어를 입력하거나 카테고리 탐색을 통해 원하는 상품에 접근할 수 있다. 그리고 원하는 상품을 장바구니에 추가하고 결제 정보 및 배송 정보를 추가하여 결제를 진행한다. 그 후 배송이 되기를 기다리면 된다.

위 관점에서 개발자가 동일한 기능을 구축한다고 생각해보자. 검색 기능에 사용되는 API를 제외하고 인증 및 결제를 처리하는 API가 필요할 것이다. 카테고리, 상품, 장바구니, 결제, 배송에 대한 API 명세를 만들고 관련된 코드를 작성해야 한다. 이때 API의 엔드포인트, 메소드, 응답, API 인증등 여러가지를 이해하고 고려해야 한다. 좋은 DX가 없으면 개발자는 문제를 신속하게 해결 할 수 없게된다. 

사용자에게 “가방" 쇼핑에 대한 선택권이 많은 것처럼 개발자에게도 도구 선택권이 많다. 적합한 선택은 좋은 DX에서 시작된다. 개발자 라이브러리, 솔루션등에 대해서 잘 이용할 수 있다는 확신이 들면 선택하고 계속 사용할 것이다.

훌륭한 개발자 경험은 사용자 관점에서 개발의 요구 사항을 아는 것에서 시작된다. 

# 우수한 개발자 경험으로 API 제품 가치 실현
개발자 경험은 개발자가 빌드할 서비스 또는 제품과 상호 작용할 때 발생하는 모든일을 포함한다. 개발자는 창조하고 혁신할 수 있는 권한을 느끼고 싶어하며 좋은 DX는 아이디어를 달성하기 위한 가능성과 단계를 보는데 도움이 된다.

개발자는 다양한 프로세스에 맞게 사용하는 서비스를 구성할 수 있어야 한다. 훌륭한 DX는 유연함을 지원하는 도구와 정보를 제공한다. 이를 수행하는 방법은 다음과 같다.
범위가 잘 지정된 다양한 API 엔드포인트 제공
여러 언어로 SDK를 개발 (혹은 API 명세서를 제공)
코드를 재사용 가능하도록 작게 분할
누구나 쉽게 이해할 수 있는 코드 만들기
작업한 것들을 광범위하게 문서화 (다음 사람을 위해)

개발자들에게 제품의 기술적 세부 사항을 이해하기에 충분한 정보를 제공하는 것은 매우 중요하다.
문서는 제품에 대한 개발자의 진입점이며 구현에 대한 것들을 알려준다. 개발자가 제품 작동 방식을 이해할 수 있다면 구현할 수 있다. 개발자가 제품 사용 방법을 배우고, 문제를 해결하고, 기존 프로세스에 통합할 수 있도록 명확한 현행화된 문서가 필요하다.

궁극적으로 개발자는 자신의 고충에 대해 제품, 도구 및 문서에서 이를 해결해야 한다고 느낄 필요가 있다. 좋은 DX를 사용하면 모든 개발자가 최소한의 지침으로 제품의 기본 구현을 할 수 있어야 한다. 예제 코드를 표시하고, 문서에 자세한 설명을 제공하고, API 명세서를 통해 개발자가 API를 사용하는 방법을 이해하고, 여러 사용 사례에 대한 명확한 예를 보여줌으로써 이를 수행할 수 있다.

# 개발자 경험
좋은 UX도 시간이 지남에 따라 개선하듯이 좋은 DX도 장기적으로 투자해야 하는 것이다. DX는 UX의 많은 측면을 공유하지만, 개발자는 UX외에 구현을 위한 특정 요구 사항이 존재한다는 것을 잊지 말아야 한다. 개발자를 위한 제품을 만들땐 DX를 이해해야 개발자를 고객으로 만들 수 있거나 혹은 잘 인계할 수 있기 때문이다.
