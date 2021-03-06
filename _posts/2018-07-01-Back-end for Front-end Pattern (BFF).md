---
title:  "Back-end for Front-end Pattern (BFF)"
tags: MSA, BFF
---

# 소프트웨어 구성의 진화
완전하게 분산된 아키텍처가 실행되기 전에는 일반적으로 하나 이상의 계층으로 어플리케이션을 작성합니다.

![image](https://user-images.githubusercontent.com/111643/116025120-b5291c00-a68a-11eb-8043-7926f8fc481d.png)

이러한 아키텍처는 매우 복잡할 순 있지만, 전반적으로 다른 어플리케이션간에 선을 그려서 시작지점과 끝지점을 명확하게 구분할 수 있었습니다. 각 어플리케이션마다 데이터 사본과 공통 비즈니스 프로세스가 중복되고 시간이 지남에따라 데이터를 공유하고 비즈니스 로직을 재사용 할 수 있는 어플리케이션이 필요했으며 단순했던 아키텍처는 약간 복잡해졌습니다.

![image](https://user-images.githubusercontent.com/111643/116025131-be19ed80-a68a-11eb-91f6-789c0e678482.png)

더 많은 재사용과 통합이 필요함에 따라 집단적 사고 방식은 “서비스”라는 매우 추상적인 개념에 정착했습니다.

![image](https://user-images.githubusercontent.com/111643/116025143-c540fb80-a68a-11eb-8a55-91864c2a7384.png)

단순한 사용자 인터페이스였던 LOB(Line-of-Business)시스템이 점점 증가하여 UI기반의 시스템으로 작성되고 있습니다.

![image](https://user-images.githubusercontent.com/111643/116025162-cd993680-a68a-11eb-8d2d-cc019effc8e5.png)

# 모노리틱 파괴
2011년 SoundCloud의 웹사이트는 아래와 같았습니다.

![image](https://user-images.githubusercontent.com/111643/116025172-d7229e80-a68a-11eb-89f0-1c66827c6e01.png)

모두 하나의 시스템에 존재했었습니다.

![image](https://user-images.githubusercontent.com/111643/116025186-de49ac80-a68a-11eb-8f03-8c4f21f5a935.png)

SoundCloud의 경우 Back-end 시스템을 매우 간단한 형태로 생각을 했었습니다.

![image](https://user-images.githubusercontent.com/111643/116025197-e6095100-a68a-11eb-8668-6a6ecd01f8ae.png)

위의 사고 방식은 큰 상자를 하나의 모놀리틱으로 구현하는 것을 의미합니다. 사실 하나의 큰 상자는 아래처럼 되어 있습니다.

![image](https://user-images.githubusercontent.com/111643/116025215-f15c7c80-a68a-11eb-916a-8dba21ef90c4.png)

사실 SoundCloud는 하나의 시스템을 가지고 있지 않았고, 여러 구성요소가 존재하는 플랫폼을 가지고 있었고, 이 아키텍처에 대한 [문제점](http://philcalcado.com/2015/09/08/how_we_ended_up_with_microservices.html)을 발견하고 마이크로서비스를 사용하기로 결정했습니다. 그래서 아래와 같이 아키텍처를 변경했습니다.

![image](https://user-images.githubusercontent.com/111643/116025262-0802d380-a68b-11eb-9faf-859f04477639.png)

![image](https://user-images.githubusercontent.com/111643/116025275-0cc78780-a68b-11eb-9891-43969d7bbd06.png)

위의 아키텍처를 수립할때 가장 중요한 점은 새로운 기능에 대한 출시일 단축이었습니다. 그리고 위의 아키텍처 수립당시에는 거의 모든 사용자는 웹에서 서비스를 사용했었습니다. 그러나 사용자층은 웹보다 모바일을 많이 사용하기 시작했습니다. SoundCloud는 안드로이드와 iOS를 위한 모바일 클라이언트를 제공하고 있었습니다. 기존 웹 어플리케이션과 마찬가지로 API를 통해 모바일 클라이언트를 지원했습니다.

![image](https://user-images.githubusercontent.com/111643/116025284-151fc280-a68b-11eb-8415-80fae6c381f3.png)

# SoundCloud의 과제
자체 API를 바탕으로 제품을 구축하는 것이 API가 높은 품질을 유지하고 항상 최신 상태를 확인할 수 있는 가장 좋은 방법으로 인식되었습니다. SoundCloud는 이 접근 방식에 대해 몇가지 문제점을 발견했습니다.
첫번째 문제는 제품 개발에 대한 근본적인 문제였습니다.

![image](https://user-images.githubusercontent.com/111643/116025302-2072ee00-a68b-11eb-920e-a61bffdfd75b.png)

단일 프로필 페이지를 생성하려면 아래와 같이 여러 API를 호출해야 합니다.
* GET /tracks/1234.json (저자)
* GET /tracks/1234/related.json (관련 트랙 추천)
* GET /users/7123.json (트랙 작성자 정보)
* GET /users/me.json (현재 사용자 정보)
* …

위의 API들을 병합하여 사용자 프로필 페이지를 만들게 됩니다. 위의 아키텍처에서는 네트워크 병목 현상이 존재 했었고 새로운 것을 추가할때마다 모든 클라이언트가 쉽게 사용할 수 있도록 많은 시간을 투자해야 했습니다.

# Front-end를 위한 Back-end
위의 아키텍처로 수립한지 약 1년후, SoundCloud는 새로운 iOS 어플리케이션을 개발하기 위한 준비를 시작했습니다.이것은 사용자 환경을 변화시키는 대규모 프로젝트였고, 아키텍처팀은 위의 문제들이 프로젝트의 방해물이 된다는 것을 알았습니다. 아키텍처를 다시 고민해야 하는 시점이었습니다.

Google의 첫번째 제안 솔루션은 모바일 및 웹용으로 서로 다른 API를 사용하는 것입니다. 이 아이디어는 클라이언트가 API를 소유하고 있는 팀을 가지고 있으면, 상호간의 조정이 필요하지 않으므로 훨씬 빨리 움직일 수 있다는 것입니다.

![image](https://user-images.githubusercontent.com/111643/116025342-35e81800-a68b-11eb-88fd-d1e1386b710a.png)

궁극적으로 코드를 단순화하고 성능을 향상 시켰습니다. BFF는 어플리케이션의 API가 아니라 클라이언트 프로그램의 일부라는 사실을 발견했습니다.

![image](https://user-images.githubusercontent.com/111643/116025359-3ed8e980-a68b-11eb-8c21-a5860710c312.png)

# 결론
SoundCloud는 생산성을 더욱 향상시키는 방법을 연구하기 시작했습니다. 모든 어플리케이션에 동일한 사용자 프로필 페이지가 존재했기 때문에 모든 BFF가 데이터를 가져와서 병합하는 과정중에 많은 중복코드가 있었습니다. 그래서 API를 묶어서 비즈니스 로직을 제공하는 서비스가 필요했습니다.

![image](https://user-images.githubusercontent.com/111643/116025371-47c9bb00-a68b-11eb-85a2-ee217b3ba75b.png)

시간이 지남에 따라 이런 상황이 점점 더 많이 발견되었고, 아키텍처는 이 부분을 포용하면서 변화되었습니다.

![image](https://user-images.githubusercontent.com/111643/116025386-5021f600-a68b-11eb-9c65-ac522d1708d2.png)

Sources
* http://philcalcado.com/2015/09/18/the_back_end_for_front_end_pattern_bff.html
