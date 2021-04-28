---
title:  "CMAF (Common Media Application Format)"
tags: CMAF
---

CMAF는 비디오 스트림을 여러 장치에서 저렴하고 쉽게 전달할 수 있도록 Microsoft 및 Apple에서 MPEG 형식으로 정의한 표준화 된 새로운 미디어 스트리밍 형식입니다.

오늘날 거의 모든 스트리밍 비디오가 표준화 된 인코딩 기술을 사용하여 압축되지만 “비디오의 경우 H.264 또는 H.265(HEVC)이고 오디오의 경우 AAC로 압축하고 있습니다.” Container는 이러한 파일에 Timing 정보를 추가하고 기타 meta data를 추가할 수 있습니다. 그리고 이러한 Container는 표준화 되지 않았습니다.

Apple의 HTTP Live Streaming(HLS) 프로토콜은 데이터를 MPEG-2 TS로 래핑하거나 캡슐화합니다. MPEG-DASH는 MPEG-4 Container(ISOBMFF)를 사용합니다.

결국, 재생할 실제 미디어는 동일한 포맷으로 되어 있지만 이런 캡슐화 포맷을 사용하면 다른 포맷을 만들어야 합니다.

OTT 서비스 제공 업체들은 비디오를 스트리밍 하기전에 이 작업을 수행해야 합니다. 그리고 이런 작업을 통해 생성된 파일을 저장해야 합니다. 즉, 사용자에게 HLS, DASH 서비스를 제공하기 위해 추가 비용이 발생하며, 이런 시스템을 구축하고 운영하는데 있어서 복잡성이 증가합니다.

![image](https://user-images.githubusercontent.com/111643/115820297-76515700-a43b-11eb-939e-929206016464.png)

CMAF는 표준화된 Container 또는 캡슐화 형식을 만들어 이 문제를 해결합니다.

![image](https://user-images.githubusercontent.com/111643/115820314-7f422880-a43b-11eb-925b-f66952afde96.png)

기존에는 위의 그림 처럼 Encryption/Container가 나눠져 있었습니다. HLS, MPEG-DASH를 제공하기 위해서는 물리 파일이 2벌이 필요했습니다. CMAF를 통해 아래의 그림처럼 협의 되었습니다.

![image](https://user-images.githubusercontent.com/111643/115820332-89fcbd80-a43b-11eb-9939-ce9c72e5a8f9.png)

Container는 CMAF로 통일 되었지만, 아직 Encryption 부분이 남아 있습니다. HLS의 경우 Encryption 알고리즘으로 AES-CBC를 채택하고 있고, MPEG-DASH의 경우 AES-CTR을 채택하고 있습니다. 이 부분이 2016년까지는 풀리지 않고 있었습니다. IBC 2017(http://www,unified-streaming.com/blog/looking-back-hot-topics-ibc-2017)에서 이 부분에 대해서 논의가 있었고, 내용은 아래와 같습니다.
* CMAF가 발표되었을 때, 통합 미디어 형식을 제공하겠다는 약속이 두 가지의 양립 불가능한 암호화 모드(AES-CTR, AES-CBC) 때문에 회의적이었음
* Apple은 CBC모드를 사용하고 Google, Microsoft는 CTR모드를 사용하기에 암호화 콘텐츠를 주요 DRM 시스템에서 다루기 위해서는 두 번 암호화해야 하는 필요가 있었음
* Google이 Widevine에 CBC 지원을 발표했고, Microsoft도 PlayReady 4.0으로 자사 제품에 CBC 지원을 통합함으로써 CMAF에서는 암호화된 미디어도 통일된 형식의 콘텐츠 제공이 가능

위의 논의 이후에 Microsoft가 2018년까지 CBC를 적용하겠다는 공지를 합니다. (https://www.microsoft.com/playready/newsroom/), 해당 링크의 PlayReady 4.0 supports CMAF interoperable format with AES-CBC and AES-CTR encryption 확인 하지만, 현실에 반영되기까지는 약간의 시간이 필요해보이네요. 그래도 미디어 사업자들의 오랜 염원이었던 부분이 스펙상으로는 해소된 느낌입니다.
