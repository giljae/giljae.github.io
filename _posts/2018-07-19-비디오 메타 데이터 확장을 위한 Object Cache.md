---
title:  "비디오 메타 데이터 확장을 위한 Object Cache"
tags: Video, Meta data, Object Cache
---
본 글은 Netflix의 Tech 블로그의 [글](https://medium.com/netflix-techblog/object-cache-for-scaling-video-metadata-management-c3c17830983e)을 기반으로 작성되었습니다.

Netflix의 도전 과제중 하나는 40개국 이상의 3천 6백만명의 고객의 요구사항을 맞춰 서비스를 확장하는 것입니다.

Netflix의 영화, TV Shows는 복잡한 메타데이터를 지니고 있습니다. 메타데이터에는 제목, 장르, 시놉시스, 출연진, ratings등의 정보가 포함되어 있고 이미지, 예고편, 인코딩 된 비디오 파일, 자막 및 에피소드 그리고 시즌에 대한 링크 정보도 포함되어 있습니다. 그리고 맞춤 장르를 위한 tag가 존재합니다. Netflix의 경우 Global 서비스를 하기에 메타데이터들이 다국어로 번역되어야 합니다.

메타데이터는 서비스에서 활용되며, 각 서비스마다 다른 형태로 사용됩니다. 사용자에게 콘텐츠 정보를 제공하는 Front-end 서비스는 이미지에 대한 링크가 필요하지만, 검색 및 추천 서비스는 Tag를 활용하여 사용자에게 추천할 영화를 검색합니다. Netflix의 경우 이런 메타 데이터 자원들을 효율적으로 제공하기 위해서 VMS(Video Metadata Services) 플랫폼을 구성했습니다.

VMS에서는 메타데이터의 상관 관계를 제공합니다. (콘텐츠 추천, 배우등 사용자가 콘텐츠를 선택하는데 도움이 되는 메타데이터의 상관 관계)

![image](https://user-images.githubusercontent.com/111643/116031458-b19c9180-a698-11eb-8ae4-c512b6964c17.png)

VMS를 구축하면서 해결해야 하는 몇 가지 요구사항이 존재했습니다.
* 하루 1000억 건이 넘는 요청을 처리 해야 한다.
* 20–30GB 사이즈의 대용량 데이터 처리, 모든 국가 및 디바이스 대상
* 높은 복잡성을 지니고 있는 메타 데이터 처리 작업
* Auto Scaling을 효율적으로 수행

초기에는 클라우드 배포를 아래의 방식으로 접근했습니다.
* 기존 RDBMS와 상호 작용하는 서버를 구현, 주기적으로 데이터 스냅샷 생성 및 S3에 업로드
* 특정 서버는 국가별 데이터를 처리하고 데이터 상관 관계를 평가하고 비즈니스 및 필터를 적용한 후 데이터 스냅샷을 생성하도록 구성
* 클라이언트측의 Object Cache는 관련 스냅샷을 로드하고 메모리 객체에서 데이터를 사용할 수 있도록 지원

Cache는 VMS에서 생성되는 스냅샷을 기반으로 주기적으로 새로 고쳐집니다.

![image](https://user-images.githubusercontent.com/111643/116031521-d0028d00-a698-11eb-934c-089017d677c8.png)

위의 아키텍처는 Netflix가 Global하게 확대되기전까지 매우 효과적이었습니다. 위 아키텍처에서는 서버 인스턴스를 구성하고 국가 또는 글로벌 여부에 관계없이 각 국가별 메타 데이터를 처리해야 했습니다. 예고편 데이터, 자막, 더빙, 언어 번역 및 현지화 그리고 계약 정보와 같은 메타 데이터는 국가에 따라 다르지만 장르, 배우, 감독등의 메타데이터는 같습니다.
처음에는 미국 기준으로 Catalog를 제공하고, 캐나다에 서비스를 하면서 두번째 서버를 추가했지만, 남미를 추가할 때 모든 지역마다 다른 콘텐츠를 제공해야 했습니다. 50개 서버의 운영 오버 헤드외에도 국가별로 데이터 중복이 있기 때문에 클라이언트의 Object Cache Footprint가 증가했습니다. 이런 점 때문에 클라이언트의 로드 시작이 길어지게 되었습니다. (중복 제거 작업을 해야 했기에…) 이러한 측면에서 Netflix는 확장할 수 있는 솔루션을 고민하게 되었습니다.
* 국가 단위가 아닌 섬(유사한 특성을 가진 국가 컬렉션)을 중심으로 체계화 된 VMS 서버 간소화
* NetflixGraph를 기반으로 적용된 메모리 최적화 기술로 메타 데이터 처리 및 중복 제거
* 모든 데이터가 아닌 Application이 원하는 메타 데이터 기반으로 운영

위의 기능을 반영하였고, 아래 노란색으로 표시된 부분이 변경되었습니다.

![image](https://user-images.githubusercontent.com/111643/116031553-e01a6c80-a698-11eb-9b98-24b3f7dcbb9e.png)

VMS는 Karyon, Netflix Graph, Governator, Curator, Archaius와 같은 Netflix에서 개발한 오픈 소스 솔루션을 활용하여 구성되었습니다. 그리고 전체 Metadata Platform Infra는 Chaos Monkey, Simian Army를 사용하여 테스트 합니다.

[Netflix Metadata Template — 한국어](https://drive.google.com/file/d/0B-s1wSZpxnXkZDgzalpldXppd0k/view?usp=sharing)
