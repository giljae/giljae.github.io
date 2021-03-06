---
title:  "OCA(Open Connect Appliance)에서 100Gbps 서비스"
tags: Netflix, OCA, CDN
---

본 글은 Netflix Tech 블로그의 내용을 번역한 내용입니다. (관심 있는 부분만…)
원문: https://medium.com/netflix-techblog/serving-100-gbps-from-an-open-connect-appliance-cdb51dda3b99

2015년 여름, Netflix Open Connect CDN팀은 NVM Express(NVMe) 스토리지 기반의 단일 FreeBSD OCA에서 100Gbps 속도로 서비스 할 수 있도록 100GbE 네트워크 인터페이스 기술을 활용하는 프로젝트를 수행하기로 결정 했습니다.

# Fake NUMA(Non-uniform Memory)
OCA의 경우 대부분의 콘텐츠는 디스크에서 제공되며 인기있는 타이틀 10–20%만 메모리에서 제공됩니다. 초기의 NVMe 프로토타입은 디스크 대역폭 문제로 인해 제한적이었습니다. 그래서 인기 있는 콘텐츠만 제공하는 형태로 테스트 서버에서 실험을 시작했고 모든 콘텐츠가 메모리에 저장되어 디스크 병목 현상이 발생하지 않았습니다. 의외로 성능은 40Gbps로 제한된 CPU에서 22Gbps로 떨어졌습니다.

pmcstat과 Frame graph를 사용하여 매우 기본적인 프로파일링을 수행했습니다. 수행 결과 lock contention에 문제가 있다는 의심이 들었습니다. 그래서 DTrace기반의 lockstat으로 프로파일링을 수행했습니다. Lockstat의 결과 Inactive page queue에 대한 lock에 CPU waiting time이 많이 소요되는 것을 확인했습니다. 왜 메모리상에서 Serving을 하는데 성능이 더 나빠졌을까요?

Netflix OCA는 비동기 sendfile() 시스템 호출을 통해 Nginx를 사용하여 Large 미디어 파일을 제공합니다.

![image](https://user-images.githubusercontent.com/111643/115835610-6b092600-a451-11eb-80fc-bc1957e31976.png)

sendfile() call flow 여기서 문제는 Inactive queue가 NUMA별 단일 목록으로 구성되고 single mutex lock으로 보호된다는 점입니다.

![image](https://user-images.githubusercontent.com/111643/115835637-72c8ca80-a451-11eb-8c5e-387683d5164c.png)

NUMA Netflix가 생각해낸 해결책은 “Fake NUMA”입니다. 시스템에 거짓으로 2개의 CPU마다 하나의 Fake NUMA가 있다고 합니다. 이 작업후에 lock contention이 거의 사라졌으며, 52Gbps로 서비스 할 수 있었습니다. (PCIe Gen3 x8 slot)

# Pbufs
FreeBSD는 “buf”구조를 사용하여 디스크 I/O를 관리 합니다. Bufs는 시스템 부팅시 정적으로 할당되며 single mutex로 보호됩니다. Netflix의 문제는 sendfile() 시스템 호출이 VM paging system을 사용하여 메모리에 없을 때 디스크에서 파일을 읽는 것입니다. 결국 모든 디스크 I/O는 pbuf mutex에 제약을 받았습니다.

Global pbuf mutex에 대한 lock contention 문제가 있었고, 이를 해결하기 위해 스왑 파티션이 아닌 파일기반으로 페이징을 처리하는 vnod pager를 수정하여 일반 커널 영역의 확장자를 사용하도록 변경했습니다. 이 변경으로 인해 잠금 경합이 제거되고 성능이 70Gbps로 향상 되었습니다.

# Mbuf Page Arrays
FreeBSD의 mbuf는 네트워크 스택의 핵심입니다. 모든 패킷은 하나 이상의 mbuf로 구성됩니다. 대량의 트래픽을 처리하기 위해서 사용하는 sendfile() system call은 mbuf내에 있는 4k page를 wrapping합니다.

![image](https://user-images.githubusercontent.com/111643/115835707-8aa04e80-a451-11eb-9c24-b9462da53b57.png)

여기에서의 단점은 많은 mbuf가 함께 연결된다는 것입니다. sendfile을 통과하는 1MB의 요청은 256개의 VM page를 참조할 수 있으며, 각 VM page는 mbuf로 wrapping되어 연결 됩니다.

![image](https://user-images.githubusercontent.com/111643/115835738-93912000-a451-11eb-808e-84f3770b9de3.png)

전송되는 mbuf의 수를 줄이기 위해 동일한 mbuf에서 동일한 유형의 여러 페이지를 전달 할 수 있도록 mbuf를 확장하기로 결정했습니다. sendfile을 위해 최대 24페이지를 전송 할 수 있는 mbuf를 설계했습니다. 그 결과 7Gbps의 성능이 향상 되었습니다. 위의 작업으로 인해 FreeBSD TCP 스택을 사용하여 90Gbps에서 100% TLS 트래픽을 제공 할 수 있게 되었습니다. 그러나 RACK, BBR과 같은 고급 TCP 알고리즘을 사용할 경우 목표에 미치지 못한다는 사실을 발견했고 TCP 코드 최적화에 대한 작업을 계속 진행중입니다.

Netflix는 참 대단한 회사입니다. 어디까지 성능을 끌어올릴지가 궁금하네요.
