---
title:  "Linux Atomic desktops"
tags: Linux, Atomic desktop, Fedora
---

지난 주말 오래된 노트북에 Fedora Atomic Desktop을 설치했다. 이제까지 사용했던 리눅스와는 생소한 개념을 갖고 있기에 여기에 글을 정리한다.


# Linux Atomic Desktops이란?

Linux Atomic Desktops은 Fedora의 Atomic Host를 통한 Atomic update 개념을 활용한다. 이 아이디어는 데스크톱을 구성하는 최소한의 것들을 변경 불가능한 것으로 취급하여 애플리케이션 업데이트 및 변경 사항을 필요한 경우 쉽게 롤백할 수 있는 개념을 적용한 것이다. 이 접근 방식은 시스템 중단을 최소화하고 시스템 수정에 대한 안정성을 강하게 만든다.

이 개념은 약 10년전 [Atomic Host](https://projectatomic.io/blog/2014/04/announcing-project-atomic/) 개발과 함께 시작되었다.


# Linux Atomic Desktop은 어떻게 동작되는가?


## Atomic update



* Transaction update: 업데이트는 단일 트랜잭션으로 적용된다. 업데이트 프로세스에 문제가 발생하면 이전 상태로 롤백하여 시스템이 안정적이고 사용 가능한 상태를 유지할 수 있도록 만든다.
* Ostree: Atomic update의 핵심은 부팅 가능한 파일 시스템을 관리하는 Ostree이다. 각 업데이트는 새로운 트리로 구성되기에 롤백과 병렬 설치가 가능하다.


## 컨테이너화



* Flatpak: 애플리케이션은 Flatpak을 사용하여 격리된 환경에서 실행된다. 이는 애플리케이션이 시스템에 엑세스 하는 것을 제한하여 보안을 강화하고 다양한 애플리케이션들이 서로 충돌없이 다양한 버전의 라이브러리를 사용할 수 있도록 만든다.
* Podman: 복잡한 애플리케이션의 경우에는 Podman이 Docker와 유사한 방식으로 컨테이너를 실행하기에 Atomic 개념에 부합된다.


## Immutable Infrastructure



* Read Only System: 코어 시스템의 파일은 읽기 전용으로 처리되고 특정 영역에서만 변경이 허용된다. 이렇게 하면 시스템 손상 및 수정의 위험이 줄어든다. 읽기 전용으로 처리되는 주요 루트 폴더는 다음과 같다.
    * /usr: 바이너리와 라이브러리가 위치하는 곳
    * /boot: 부트 로더 파일이 위치하는 곳
* 통제 가능 영역
    * /etc: ostree에서 변경사항이 추적되고 관리된다.
* 계층형 패키지: 사용자는 기본 이미지 위에 레이어 형태로 RPM 패키지를 설치할 수 있다. 이는 추적되고 Atomic하게 관리된다.


# 장점



* 신뢰성: 업데이트와 잠재적 충돌의 영향을 최소화하기에 데스크톱 환경의 안정성이 향상된다.
* 일관성: 각 인스턴스를다른 인스턴스와 일관성을 유지하여 소프트웨어 버전 및 동작의 불일치를 줄인다.
* 보안: 애플리케이션의 격리와 시스템 구성 요소의 변경 불가로 인해 보안을 강화한다.
* 롤백 및 재현성: 쉬운 롤백 기능과 환경을 재현하는 기능으로 관리와 문제 해결이 편리해진다.


# 단점

일반적으로 “컨테이너 = 복잡하다” 라고 주장하는 사람들에게는 어려울 수 있다. 근데 실제 사용해보면 사용자는 이 차이를 잘 느끼지 못한다. Flatpak을 사용해본 경험이 있다면, 전혀 다를게 없다고 느낄 수 있다.


# 다른 배포판은 없는가?

물론 “Immutable distro (불변 배포판)” 라는 이름으로 이런 개념이 적용된 배포판이 존재한다. 최근에는 Atomic이라는 용어를 사용하고 있다. 아래는 “Immutable distro” 목록이다.

**1. Carbon OS**

[carbonOS](https://carbon.sh/)는 Flatpak 및 컨테이너 우선 접근 방식을 취한다. carbonOS는 시스템 업데이트시 검증된 부팅을 제공하는 것을 목표로 한다.

**2. Fedora Silverblue**

[Siverblue](https://silverblue.fedoraproject.org/)는 불변성을 갖춘 Fedora Workstation의 변형이다. (내가 사용하고 있다.)

사용자 인터페이스 및 경험은 Fedora Workstation과 변함이 없다. 

Siverblue는 테스트 및 컨테이너 기반 소프트웨어 개발에 유용한 안정적인 경험을 제공하는 것을 목표로 한다. 업데이트 후 문제가 발생하면 이전 버전으로 롤백할 수 있다.

**3. Flatcar 컨테이너 리눅스**

[Flatcar](https://www.flatcar.org/)는 컨테이너 워크로드에 맞춰 커뮤니티에서 만든 리눅스 배포판이다.

컨테이너를 실행하는데 필요한 도구만 포함된 최소한의 OS 이미지가 제공된다.

**4. NixOS**

[NixOS](https://nixos.org/)는 사용 가능한 가장 진보된 리눅스 배포판 중 하나이다. 패키지 관리자를 여러 운영체제에서 사용할 수 있다.

**5. GUIX**

[GUIX](https://guix.gnu.org/)는 NixOS와 비슷하지만, 안정적인 시스템 사용 및 업그레이드에 대한 제어를 위한 고급 사용자를 위해 제작되었다.

**6. 바닐라 OS**

[Vanilla OS](https://vanillaos.org/)는 불변 OS 분야에서 최근에 진입한 기업이다. 신뢰성과 변경 불가능한 기능을 갖춘 쉬운 데스크탑 환경을 제공하는 것을 목표로 한다.

**7. BlendOS**

[BlendOS](https://blendos.co/)는 다른 배포판의 모든 장점을 제공하는 것을 목표로 한다.

불변성과 업데이트에 대한 안정성을 확보하면서 RPM, DEB등 다양한 패키지를 이용해 애플리케이션을 설치할 수 있다. (개인적으로 궁금한 배포판이다.)


# 결론

Linux Atomic Desktops는 데스크탑 환경이 관리되고 유지되는 방식에 많은 변화를 가져다준다. 그리고 사용자에게 일관된 데스크탑 경험을 제공한다. 이 기술이 성숙해지게 되면 Linux 생태계에서 도태되고 있던 데스크탑 환경의 안정성 및 새로운 표준으로 자리매김 할 것으로 예상된다.