---
layout: post
title:  "Istio & Envoy에서 OpenTracing 사용 하기"
comments: true
tags: Istio, Envoy, Service Mesh, OpenTracing
---
Sidecar Proxy는 코드 삽입없이 모니터링 데이터를 얻는 매우 간단한 방법을 제공한다. Tracing은 대규모 분산 시스템에서 가장 어려운 부분이기 때문에 Sidecar Pattern은 큰 이점으로 작용한다.

Sidecar Proxy에서 Tracing을 하기 위해서는 Inbound에서 Outbound 요청으로 일부 Header를 전달해야 한다. Application단에서는 매우 간단하지만 전달받은 Header를 넘기는 로직을 처리하면 불편 할 수 있다. 비즈니스 레이어에서 Header를 전달하는 것을 조정한다고 상상해보면 Sidecar Pattern을 사용하는 이점이 없을 수 도 있다.

# Tracing은 Envoy만 사용
Header 전달이 기존 Application 코드에서는 아래처럼 작성된다.

<script src="https://gist.github.com/giljae/7ea7c9ff3b12b65a26cf24ac3f3600f9.js"></script>

위의 코드상에서는 특별한 것은 없다. 하지만 코드상에 header 정보를 set하기에 변경이 생기면 리팩토링이 필요할 수 도 있다. 그리고 코드에 header 정보를 set하기에 잊어버릴 가능성도 존재한다.

# Envoy and OpenTracing
OpenTracing의 일부 기능을 어플리케이션에 추가 한다. Spring Boot는 classpath에 jar를 추가하는 것만으로도 구현이 가능하다. Auto Configuration은 개발자가 개입하지 않아도 필요한 Tracing Code를 어플리케이션에 추가합니다.

OpenTracing은 Vendor 중립적이기에 Tracing 구현을 제공해야 한다. 이 경우 일반적으로 jaeger-java-client를 사용하게 된다. 끝으로 Tracing Bean을 Instance화하고 구성해야 한다. Envoy는 기본적으로 Jaeger에서 활성화되지 않고 명시적으로 등록되어야 한다.

<script src="https://gist.github.com/giljae/aa89f9c7259510932194ef02ed364ea6.js"></script>

Istio, Jaeger 및 어플리케이션을 kubernetes에 배포한다. 배포가 완료되면 /chaining endpoint에 요청을 할 수 있다.

![image](https://user-images.githubusercontent.com/111643/116033280-1c030100-a69c-11eb-81fb-f3d1e1148ee2.png)

Proxy 범위에 적용되는 Timeout 규칙은 두 가지이다.

첫번째는 Proxy Client의 지속 기간이 원래 지속 기간보다 항상 짧아야 한다.

두번째는 첫번째와 반대이며 Proxy Server 기간이 항상 길어야 한다.

![image](https://user-images.githubusercontent.com/111643/116033304-258c6900-a69c-11eb-9b38-3f0bb8655bb4.png)

span과 관련된 로그 내에서 어떤 컨트롤러 메소드가 호출되었는지 확인할 수 있다.

Envoy만 Tracing하면 설치가 매우 간단해진다. OpenTracing을 사용하면 이 작업을 자동으로 할 수 있으며 모니터링되는 프로세스에 대한 가시성을 높일 수 있다.

