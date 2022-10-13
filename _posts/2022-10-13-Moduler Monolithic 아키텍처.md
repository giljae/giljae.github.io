---
title:  "Moduler Monolithic 아키텍처"
tags: Software Architect, Moduler Monolithic
---
프로젝트를 준비하면서 아키텍처에 대한 고민이 많다. 마이크로서비스 아키텍처로 프로젝트를 하였지만, 현 상황에서는 마이크로서비스 아키텍처가 어울리지 않을 수 있다는 판단을 했고 이유는 아래와 같다.

1. B2C라고 볼 순 있지만  사용자 수가 정해져 있다.
2. 트래픽이 증가하는 시간이 정해져 있다.
3. 리소스 비용을 절약해야 함 (비용 감축을 위해서 트래픽이 몰리는 시간대외에는 인스턴스를 축소할 계획 있음)
4. 데이터베이스가 통합되어 있음 (미니서비스로 고려가 가능)
5. 각 서비스마다 다른 기술을 사용하진 않을 것 같음
6. 레거시가 존재하기에 리팩토링이 우선되어야 함 (이 부분이 가장 중요!)

또한, 아래 사항들을 고려해야 한다.

1. 글로벌 진출 시 로컬라이제이션을 고려해야 함
2. 확장 가능하고 재사용 가능하며 유지보수가 쉽도록 만들어야 함
3. 새로운 사람이 참여하게 되면 비즈니스 기능 및 시스템 기능을 쉽게 이해할 수 있어야 함
4. Actor별 UI가 각각 존재해야 함
5. 외부로 연계되는 부분이 많음
6. 마이크로서비스 아키텍처 전환에 대한 기틀 마련

모놀리식은 단일 단위의 애플리케이션이다. 즉, 모든 비즈니스 로직이 한 곳에 있고 애플리케이션에 변경 사항이 적용되면 전체 애플리케이션에 영향을 미치기에 전체가 다시 배포되어야 한다.

또한, 모놀리식은 종종 큰 진흙 덩어리처럼 보이기도 한다. 시간이 지남에 따라 애플리케이션이 발전하기에 비대해지기 때문이다.

![image](https://user-images.githubusercontent.com/111643/195584625-8faa4f32-6e0f-4029-99af-465a4cba655c.png)

<출처: [https://lewiseason.co.uk/2019/11/05/large-application-architectures](https://lewiseason.co.uk/2019/11/05/large-application-architectures)>

위 그림은 큰 진흙 덩어리인지 분산된 진흙 덩어리인지에 따라  모듈러 모놀리식과 마이크로서비스 아키텍처를 선택하는 것에 대한 아이디어를 준다. 초반에는 모듈식 모놀리식으로 시작하고 제품이 충분히 성공적이면 마이크로서비스로의 이동을 고려해 볼 수 있다.

# 모놀리식 아키텍처

기존의 모놀리식 애플리케이션은 빌드할 때 코드 개발 및 유지 관리를 쉽게 하기 위해 로직을 여러 계층으로 나눈다. 

![image](https://user-images.githubusercontent.com/111643/195584659-76b5ca23-c98d-48ab-a1f8-7539db718dad.png)
  
<출처: [https://www.n-ix.com/microservices-vs-monolith-which-architecture-best-choice-your-business/](https://www.n-ix.com/microservices-vs-monolith-which-architecture-best-choice-your-business/)>

위의 구조는 효율적으로 애플리케이션을 구축하기 위한 방법이지만, 코드를 변경하면 전체에 영향을 미치기 때문에 유연하지 않다. 이런 상황으로 인해 순수한 모놀리식은 사용할 수 없다.

# 모듈식 모놀리식 아키텍처

모듈식 모놀리식 아키텍처는 먼저 로직을 모듈로 나누는 방식으로 구성되며, 각 모듈은 독립적이고 격리된다. 각 모듈에는 고유한 비즈니스 로직이 있어야 한다. 이렇게 다른 모듈에는 최대한 영향을 주지 않고 레이어를 만들고 수정할 수 있어야 한다.

![image](https://user-images.githubusercontent.com/111643/195584702-06f5f22f-7c66-4de0-b61a-e764e1bbcb31.png)

<출처: [https://www.fullstacklabs.co/blog/modular-monolithic-vs-microservices](https://www.fullstacklabs.co/blog/modular-monolithic-vs-microservices)>

마이크로서비스 아키텍처가 모든 것을 해결하는 솔루션이 아니다. 더 적합한 다른 접근 방식이 존재할 수 있다. 결국 진화에 의해 향후에는 마이크로서비스 아키텍처로 끝나더라도 거기까지 도달하기 위한 다른 중간 단계가 존재하는 것이다.

모듈식 모놀리식은 아래의 특징을 지니고 있다.

- 모듈은 완전히 독립적이지 않다. 다른 모듈과의 종속성이 있지만 최소화해야 한다.
- 모듈은 상호 교환이 가능하다.
- 코드를 재사용할 수 있다.
- 기존의 모놀리식에 비해 종속성을 더 잘 구성할 수 있다.
- 기존의 모놀리식보다 유지 관리하고 개발하기가 더 쉽다.
- 배포를 위해 다른 서버가 필요 없이 전체 프로젝트를 단일 단위로 유지할 수 있다.
- 기존의 모놀리식보다 확장성이 뛰어나다.
- 마이크로서비스 아키텍처보다 덜 복잡하다.

일반적인 Java 패키징 체계는 아래 왼쪽 그림처럼 웹 항목, 비즈니스 로직, 데이터베이스 엑세스을 별도의 레이어 패키지로 그룹화 한다.

![image](https://user-images.githubusercontent.com/111643/195584755-30151b35-8b4a-41e8-bbff-728a912a934e.png)
  
<출처: [https://medium.com/design-and-tech-co/modular-monoliths-a-gateway-to-microservices-946f2cbdf382](https://medium.com/design-and-tech-co/modular-monoliths-a-gateway-to-microservices-946f2cbdf382)>

문제는 시간이 지나면 위 오른쪽 그림처럼 스파게티 코드가 만들어진다는 것이다. 이런 상황이 발생하는 것은 일반적으로 Java에서 대부분 클래스를 Public으로 선언 하기에 캡슐화하는 것이 아니기에 어디서든 접근 할 수 있게 된다.

결국, Jigsaw(자바 모듈 관리)를 이용해서 패키지를 관리해야 한다. (참고: [https://openjdk.org/projects/jigsaw/](https://openjdk.org/projects/jigsaw/))

![image](https://user-images.githubusercontent.com/111643/195584784-2b44cf8c-2f30-41ba-96bb-6c7cfebe4a43.png)
  
<출처: [https://medium.com/design-and-tech-co/modular-monoliths-a-gateway-to-microservices-946f2cbdf382](https://medium.com/design-and-tech-co/modular-monoliths-a-gateway-to-microservices-946f2cbdf382)>

위 그림의 패키징 체계에서는 비즈니스 로직 및 데이터 엑세스를 구현한 클래스는 보호되고, 해당 패키지의 접근은 유일하게 정의된 인터페이스를 통해서만 이루어지기에 된다. 

이렇게 하면, 느슨하게 결합된 모듈을 만들 수 있고 인터페이스를 일관되게 유지 할 수 있고 향후 마이크로서비스 아키텍처로 전환 할 경우에도 유리하게 작용될 수 있다고 생각한다.

좋은 아키텍처는 끊임없이 진화하는 작업이며 올바른 솔루션은 운영 규모에 따라 달라진다. 

이번 선택이 올바른 선택이길 바란다.
