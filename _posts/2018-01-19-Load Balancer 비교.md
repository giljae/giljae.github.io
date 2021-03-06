---
title:  "Load Balancer 비교"
tags: Load balancer
---

어플리케이션의 고 가용성을 설정하고 성능을 향상시키는 쉽고 가장 빠른 방법 중 하나는 LB(Load Balancer)를 이용하는 것입니다.

로드밸런서는 세 가지 유형이 있습니다.

하드웨어 로드 밸런서는 부하 분산을 제공하는 전용 장치이고 하드웨어 벤더 중 일부는 다음과 같습니다.

하드웨어 로드 밸런서는 가격이 비싸지만, 좋은 성능을 제공합니다.

클라우드 로드 밸런서는 클라우드의 인기와 더불어 많이 사용되고 있습니다. 클라우드 로드 밸런서를 사용하는 것은 하드웨어 어플라이언스에 투자하지 않고도 관련된 모든 기능을 즐길 수 있는 저렴한 방법 중 하나입니다.

다음은 클라우드 로드 밸런서 중 일부입니다.
* AWS
* Google Cloud
* Cloudflare
* Incapsula
* DigitalOcean
* Azure

![image](https://user-images.githubusercontent.com/111643/115833577-2aa8a880-a44f-11eb-9dd9-434c9b2dcb38.png)

월 기준으로 20달러의 가격부터 시작할 수 있습니다. 마지막으로 직접 설치, 관리 및 구성하는 소프트웨어 로드 밸런서가 있습니다.

오픈 소스 로드 밸런서의 목록은 다음과 같습니다.
* Seesaw
* LoadMaster by KEMP
* HAProxy
* ZEVENET
* Neutrino
* Balance
* Pen
* Nginx
* Tradefik
* Gobetween

# Seesaw
구글에서 제작한 Seesaw는 Go 언어로 개발되었으며, 우분투/데비안과 같은 리눅스 배포판에서 잘 동작됩니다.

Anycast, DSR(Direct Server Return)을 지원하며 최소 두 개의 Seesaw 노드가 필요합니다. Baremetal환경과 가상 환경에서 동작합니다.

기본적으로 Seesaw는 Layer 4 network에서 작동하며, Layer 7에서의 균형 조정이 필요한 경우 Option을 이용해서 사용할 수 있습니다.

# KEMP
KEMP는 AWS또는 Azure와 같은 클라우드 데이터센터에 배포하여 사용 할 수 있습니다.

![image](https://user-images.githubusercontent.com/111643/115833702-5166df00-a44f-11eb-90dd-83b83ccfa107.png)

무료이지만 상용 수준의 기능을 제공합니다.
* Round Robin 또는 Least connection 알고리즘을 사용하는 TCP/UDP에 대한 Layer 4 계층의 로드 밸런싱
* Layer 7 계층의 로드 밸런싱
* WAF(Web Application Firewall)가 내장되어 있음
* IPS(Intrusion Prevention Engine)가 내장되어 있음
* Global 로드 밸런싱, 다중 사이트 지원
* Caching, 콘텐츠 압축, 콘텐츠 스위칭 지원
* Web cookie persistence
* IPSec tunneling

KEMP 로드 밸런서는 Apple, Sony, JP Morgan, Audi, Hyundai 등의 대형 회사에서 사용되고 있습니다. 무료 버전으로도 충분한 기능을 제공하지만, 더 많은 기능이 필요할 경우 상용 라이센스를 구입해야 합니다.

# HAProxy
High-availability, Proxy, TCP/HTTP 로드 밸런싱을 지원하는 제품입니다. HAProxy는 아래의 회사에서 사용하고 있습니다.
* Airbnb
* Github
* Imgur
* MaxCDN
* Reddit

기능은 다음과 같습니다.
* Support IPv6 and Unix socket
* Deflate & Gzip compression
* Health-check
* Source-based session stickiness
* 통계 레포팅 기능이 내장되어 있음

# Zevenet
Zevenet은 L3, L4, L7을 지원합니다.

![image](https://user-images.githubusercontent.com/111643/115833889-85420480-a44f-11eb-962a-ee3cc2cc724c.png)

Advanced health-check 모니터링을 지원 하기에 끊김없는 사용자 경험을 제공합니다. Zen으로 알려진 Zevenet은 FTP, SIP, SSL, HTTP등과 같은 TCP기반 프로토콜을 지원합니다.

# Neutrino
Neutrino는 Ebay에서 사용하고 있으며, Scala & Netty를 사용하여 개발되었습니다. Least-connection, Round-robin 알고리즘을 지원합니다.
* Using canonical names
* Context-based
* L4 using TCP port numbers

![image](https://user-images.githubusercontent.com/111643/115833948-9a1e9800-a44f-11eb-8c58-fb7166081194.png)

Neutrino는 2 Core VM에서 초당 300개의 요청 처리가 가능합니다. HAProxy와 비교시 Neutrino를 사용할 때의 주요 이점은 L7 스위칭입니다. 항상 그렇듯이 두 가지 방법을 모두 사용하고 환경에 가장 적합한 것을 고려해야 합니다.

# Balance
기본적인 로드 밸런싱 기능을 지니고 있습니다.

# Pen
Pen은 로드 밸런싱의 기본 기능과 함께 아래 기능을 제공합니다.
* GeoIP 필터
* SSL 종료
* IPv4 및 IPv6 호환성

# Nginx
오픈 소스 Nginx는 기본 수준의 콘텐츠 스위칭 및 여러 서버에 대한 라우팅을 지원합니다. Nginx Plus의 경우는 그 이상의 기능을 제공합니다. 로드 밸런싱, 콘텐츠 캐싱, 웹 서버, WAF, 모니터링등을 포함한 All-in-one web application delivery solution을 제공합니다. 초당 수백만 건의 요청을 처리 할 수 있는 확장형 어플리케이션을 위한 고성능 로드 밸런서 솔루션도 제공 합니다.

# Traefik
HTTP reserve proxy와 로드 밸런서를 제공합니다. 또한 여러 백엔드 서비스를 지원합니다. (Amazon ECS, Docker, Kubernetes, Rancher.,)

![image](https://user-images.githubusercontent.com/111643/115834031-b589a300-a44f-11eb-85c5-370d43e15bfd.png)

Web socket, HTTP/2, 자동 SSL 인증서 갱신, 암호 관리, 리소스 관리 및 모니터링을 위한 Interface를 제공합니다.

# Gobetween
Lightweight하지만 강력한 고성능 L4 TCP, TLS 및 UDP 기반의 로드 밸런서 입니다.

![image](https://user-images.githubusercontent.com/111643/115834080-c508ec00-a44f-11eb-9748-b8201698e23f.png)

Windows, Linux, Docker, Darwin과 같은 멀티 플랫폼에서 동작합니다. 로드 밸런싱은 구성에서 설정해야 합니다. 아래의 알고리즘 기반으로 수행됩니다.
* IP 해시
* Round-robin
* The Least bandwidth
* Least connection
* Weight

아래의 벤치마크 정보를 기준으로 Gobetween은 HAProxy보다 빠르지만 Nginx보다는 느립니다.

![image](https://user-images.githubusercontent.com/111643/115834141-d81bbc00-a44f-11eb-9612-55365d2dc633.png)

동적 환경을 위한 자동 검색 기능을 탑재한 최신 L4 로드 밸런서를 찾고 있다면 Gobetween이 유망할 것으로 보여집니다.

위의 정보를 기준으로 각 상황에 맞는 로드 밸런서를 선택하시길 바랍니다.
