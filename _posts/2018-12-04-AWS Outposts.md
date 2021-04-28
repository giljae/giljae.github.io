---
title:  "AWS Outposts"
tags: AWS, Outpost
---
AWS는 지난 수년간 Amazon VPC(Virtual Private Computing), AWS Direct Connect와 Amazon Storage Gateway와 같은 서비스를 제공하여 AWS와 함께 사내 구축형 데이터 센터를 손쉽게 실행할 수 있도록 지원해왔다. 2017년에는 VMware와 공동 작업을 통해 AWS에 VMware Cloud를 도입하여 VMware 가상화 기업의 대다수가 AWS에 쉽게 인프라를 관리 할 수 있도록 VMware 도구도 지원하고 있다.

이런 지원에도 불구하고 고객들은 일부분은 사내 데이터센터에 머물기를 희망하고 있었다. 그리고 AWS는 현재 많은 회사들이 여전히 사내에 머물고 싶어한다는 점을 인정했다.

AWS Outposts는 AWS Computing 기반의 Rack에 Amazon EC2(Amazon Elastic Compute Cloud), Amazon EBS(Amazon Elastic Block Store)와 같은 서비스를 실행 할 수 있도록 제공함으로써 이러한 문제를 해결한다. Amazon Outposts는 두가지 Option을 제공한다.
* VMware Control Plane과 API를 사용하여 인프라를 구성하고자 하는 고객은 AWS Outposts에서 VMware Cloud를 로컬로 실행 할 수 있다. AWS Outposts의 VMware Cloud Option은 On-Premise에서 실행되고 AWS의 VMware Cloud와 동일한 console에서 서비스를 관리할 수 있다.
* AWS Cloud에 익숙한 고객은 On-Premise에서 AWS Outpost의 AWS Option을 사용할 수 있다. 즉, On-Premise와 Cloud에서 같은 방식으로 관리할 수 있게 된다.

두 경우 모두 AWS는 고객에게 Rack을 제공하고, 고객이 원하는 경우 설치하고, Rack의 유지 보수 및 교체를 모두 처리하게 된다. AWS Outposts는 고객의 Amazon VPC의 연장이며 고객은 AWS Outposts에서 AWS 또는 기타 AWS 서비스의 나머지 어플리케이션과도 원할하게 연결할 수 있게 된다.

AWS의 경우 고객이 AWS가 사용하고 있는 것과 동일한 하드웨어, 동일한 인터페이스, 동일한 API를 사용하여 On-Premise AWS 환경에서 AWS또는 VMware Cloud로 확장하길 원한다고 언급했고, 최신 AWS 기능을 즉시 사용할 수 있어야 하며 하드웨어나 소프트웨어를 고려하고 싶지 않다고 했다. 그래서 AWS는 Hybrid mode에서 실행될 때 고객이 실제로 원했던 것을 재해석하기 위해 AWS Outposts를 개발했다고 말했다.

Google 그리고 Amazon도 On-Premise 시장에 진출을 시작했다. 이미 시장 저변에 깔려있는 VMware라는 우군과 함께…

이 상황에서 국내 Cloud 사업자들은 어떤 전략을 펼칠 것인지 궁금하다. 가장 무서운 점은 익숙함이라는 무기이다. AWS Cloud를 사용했던 익숙함을 경험한 고객의 선택은 무엇일까?
