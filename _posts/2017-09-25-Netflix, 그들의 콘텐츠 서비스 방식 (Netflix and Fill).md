---
layout: post
title:  "Netflix, 그들의 콘텐츠 서비스 방식 (Netflix and Fill)"
comments: true
tags: netflix, cdn, oca
---

본 글은 넷플릭스의 Tech블로그의 내용과 개인적인 의견을 포함하여 작성했습니다.
(참조: https://medium.com/netflix-techblog/netflix-and-fill-c43a32b490c0)

새로운 콘텐츠가 출시 되면 CP(Content Provider)로부터 넷플릭스내의 콘텐츠 운영팀으로 Digital Asset이 전달 됩니다.

이 과정에서 Netflix Platform에 통합하기 위해 필수적인 품질관리, 인코딩등 다양한 유형의 처리 및 개선이 이루어집니다. 이 단계가 끝나면 관련 Digital Asset (Bitrate, Caption)이 다시 패키징되어 Amazon S3에 배포 됩니다.

배포 준비가 된 S3의 콘텐츠는 콘텐츠 운영팀에 의해 metadata flag가 지정되며, 이 시점에서 Open Connect 시스템이 인계받아 OCA(Open Connect Appliance)에 콘텐츠를 배포하기 시작합니다.

# Proactive Caching
Netflix의 Open Connect와 다른 CDN과의 가장 큰 차이점은 Proactive Caching입니다. 사용자들이 시청할 시간과 시청 시간을 높은 정확성으로 예측할 수 있기 때문에 구성 가능한 시간대 동안 non-peak bandwidth를 사용하여 예측한 콘텐츠를 OCA에 다운로드 할 수 있습니다. 다른 CDN은 이것이 불가능하고 범용적인 Delivery Service를 제공해야 하므로, LRU기반의 Caching을 선호하지요. CDN 사업자가 콘텐츠를 예측할 필요는 없으니까요. 그들은 미디어 사업자가 아니니까요.

# OCA Clusters
Netflix의 Fill pattern이 어떻게 동작하는지 이해하려면 OCA가 IX에 위치하거나 ISP 네트워크에 포함되어 있는지 여부에 상관없이 OCA 클러스터를 구성하는 방법을 이해해야 합니다.

OCA는 manifest cluster로 그룹화 됩니다. 각 manifest 클러스터는 적절한 콘텐츠 영역(콘텐츠를 스트리밍 할 것으로 예상되는 국가 그룹), 인기도 피드(이전 데이터를 기준으로 간략하게 정렬된 콘텐츠 목록)로 구성되고 보유해야 하는 콘텐츠의 수를 표시합니다. Netflix는 국가, 지역 또는 기타 선정 기준에 따라 독립적으로 인기 순위를 계산합니다.

Fill cluster는 shared content영역과 인기 피드가 있는 manifest cluster의 그룹입니다. 각각의 fill cluster는 Open Connect 운영팀에 의해 fill escalation policies와 fill master의 수로 구성됩니다.

아래의 다이어그램은 동일한 Fill cluster내의 manifest cluster의 예를 설명합니다.

![image](https://user-images.githubusercontent.com/111643/115819346-a0097e80-a439-11eb-8454-4850aa2a5841.png)

# Fill Source Manifests
OCA들은 네트워크내의 다른 OCA에 대한 정보, 콘텐츠, 인기도등을 저장하지 않습니다. 모든 정보는 AWS Control Plain에 집계되고 저장 됩니다. OCA는 주기적으로 Control Plain과 통신하여 Cluster 멤버들에게 storing하고 serving해야 할 콘텐츠 목록이 포함된 manifest 파일을 요청합니다. AWS Control Plain은 여러 고수준 요소를 고려하여 ranked list를 다운로드 할 수 있는 location을 response합니다.
* Title(Content) availability — Does the fill source have the requested title(content) stored?
* Fill health — Can the fill source take on additional fill traffic?
* A calculated route cost — Described in the next section. (아래 섹션에서 설명)

# Calculating the Least Expensive Fill Source
S3(Origin)에서 모든 OCA에 직접 콘텐츠를 배포하는 것은 시간과 비용면에서 비효율적이므로 계층화된 접근법을 사용 합니다. Open Connect의 목표는 가장 효율적인 경로를 사용하여 콘텐츠를 전달하도록 하는 것입니다.

Least expensive fill source를 계산하기 위하여 각 OCA의 네트워크 상태 및 구성 매개변수를 고려합니다.
* BGP path attributes and physical location (latitude / longitude)
* Fill master (number per fill cluster)
* Fill escalation policies

Fill escalation policies는 다음과 같이 정의합니다.
1. OCA가 콘텐츠를 다운로드 하기 위해 갈 수 있는 hop 수와 대기시간
2. OCA가 정의된 hop 이상으로 전체 Open Connect 네트워크로 이동 할 수 있는지 여부와 대기시간
3. OCA가 S3(Origin)으로 갈 수 있는 여부와 대기시간

Control Plain은 Master로 지정된 OCA를 선택합니다. Master에 적용되는 fill escalation policies는 일반적으로 콘텐츠를 가져와서 non-master와 로컬로 공유하기 위한 지연시간을 줄여 최대한 멀리 도달 할 수 있게 합니다.

경로 계산에 대한 모든 입력이 주어지면, fill source 작업은 다음과 같이 작동합니다.
1. Peer fill — Available OCAs within the same manifest cluster or the same subnet
2. Tier fill — Available OCAs outside the manifest cluster configuration
3. Cache fill — Direct download from S3

# Example Scenario
Fill master OCA가 S3로 부터 콘텐츠 다운로드를 완료 한 후 Control Plain에 콘텐츠가 저장되었음을 보고 합니다.

![image](https://user-images.githubusercontent.com/111643/115819475-e1019300-a439-11eb-9dc3-0c9025059457.png)

그 다음 다른 OCA가 Control Plain과 통신하여 해당 콘텐츠의 fill source 요청을 보내면 Fill master에서 fill option이 제공됩니다.

![image](https://user-images.githubusercontent.com/111643/115819497-ea8afb00-a439-11eb-81b5-3f4eb922bc29.png)

두 번째 계층의 OCA가 다운로드를 완료하면 상태를 보고하고 다른 OCA는 해당 콘텐츠에 대한 fill source 작업을 수행 합니다. 이 작업은 fill window내에서 계속 반복됩니다.

더 이상 필요없는 콘텐츠는 delete manifest에 저장되고 일정 기간 후에 삭제됩니다.

Netflix 사용자가 스트리밍을 시작하면 이 시간대의 fill source 작업이 끝나고, fill window가 다른 시간대로 이동하면서 fill source pattern이 계속 진행 됩니다. (Netflix는 글로벌 서비스이기에 각 지역별로 네트워크 유휴 시간대가 다름)

# Challenges
Netflix는 항상 fill process를 개선하고 있습니다. Open Connect 운영팀에서는 내부 툴을 사용하여 Fill traffic을 지속적으로 모니터링합니다. member들에게 서비스를 제공해야하는 catalog의 임계값 비율을 포함하지 않는 OCA에 대한 alert이 설정되고 모니터링 됩니다. 이 경우에는 다음 Fill process 전에 해당 문제를 해결합니다. 신속하게 배포해야 하는 새로운 콘텐츠나 기타 수정 사항에 대해 주기적으로 fast-track fill을 수행 할 수 있습니다. 기본적으로 이러한 fill pattern을 사용하면서 배포 시간 및 프로세싱 시간을 줄입니다.

Netflix는 190개국에서 운영되고 있으며 전 세계 여러 ISP 네트워크에 수천 개의 장비가 내장되어 있기에 ISP에 대한 대역폭 비용을 최소화 하면서 OCA에 최신 콘텐츠를 빨리 얻을 수 있도록 하는데에 집중하고 있습니다.

# 끝으로
Netflix가 일반적인 Caching(LRU, NRU)방식이 아닌 Proactive caching 방식을 택한 이유는 그들이 가지고 있는 Network를 온전히 서비스를 위해서 사용하기 위함으로 보여진다. NRU, LRU는 Proactive caching에 비해 Miss가 발생할 확률이 존재하기에 이러한 대역폭도 서비스적인 측면에서는 아깝다는 그들의 집착이 보여진다. Netflix는 일반 CDN업체가 아닌 미디어 사업자이기에 가능한 얘기가 아닐까? 결국 그들의 생각은 OCP는 비교적 저렴한 x86기반의 하드웨어를 쓰고 네트워크를 최대한 활용하여 가성비 있는 Open Connect를 운영하고 그 남는 비용으로 콘텐츠를 제작하겠다 아닐까?

관련된 내용은 아래의 링크를 참조
* https://www.youtube.com/watch?v=KtSBbsSwq-g
* https://johannesbergsummit.com/wp-content/uploads/sites/6/2014/11/Monday_8_David-Fullagar.pdf
