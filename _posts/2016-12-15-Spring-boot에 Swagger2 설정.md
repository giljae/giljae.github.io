---
title:  "Spring-boot에 Swagger2 설정"
tags: Spring boot, Swagger
---

RESTful API를 만들 경우에는 문서화가 중요하고, API문서와 코드와의 변경 사항을 반영하는 것은 지루한 일입니다.

일반적으로 Swagger를 사용할 경우에는 @Api, @ApiOperation과 같은 Annotation을 사용하여Swagger에 보여질 내용을 설정하게 됩니다.

Swagger2의 구현체인 Springfox를 사용할 경우에는 이런 부분들이 자동화되게 됩니다.

# 타겟 프로젝트
REST 서비스는 아래의 URL을 참조하여 Swagger2를 적용할 수 있도록 생성해야 합니다. (본 문서의 범주가 아니므로 링크로 대체합니다.)
* Build a REST API with Spring 4 and Java Config article
* Building a RESTful Web Service.

# 메이븐 의존성 추가
생성한 프로젝트의 pom.xml 내에 아래의 의존성을 추가합니다.

<script src="https://gist.github.com/giljae/4f9aaf57e191a59bde0948ad5c4a29df.js"></script>

# 프로젝트에 설정하기
Swagger 설정은 Docket Bean으로 합니다.

<script src="https://gist.github.com/giljae/1750b0f6d42ed69173eddceef63f79c3.js"></script>

위의 설정을 통해 스프링 부트에 Swagger2를 통합할 수 있습니다.

# 확인하기
설정이 완료되었으면, 스프링 부트 어플리케이션을 실행하여 아래의 URL로 확인할 수 있습니다.

http://localhost:8080/{your-app-root}/v2/api-docs

JSON type의 response를 보게 되면 올바르게 설정이 된 겁니다. Swagger에서는 보다 편하게 확인하기 위해 Swagger-UI를 제공해줍니다.

# Swagger-UI로 확인하기
이미 Swagger-UI 의존성을 pom.xml에 추가하였기에, 브라우저에서 아래의 URL로 확인합니다.

http://localhost:8080/{your-app-root}/swagger-ui.html

![image](https://user-images.githubusercontent.com/111643/115681528-93c8e700-a38f-11eb-9da0-bb289f12e5c3.png)

기타 고급 설정에 대한 부분은 아래의 참조 URL을 확인하세요. 테스트에 사용된 Full-source는
https://github.com/giljae/sandbox/tree/master/swagger-springboot에서 확인할 수 있습니다.

# 참조
* http://www.baeldung.com/swagger-2-documentation-for-spring-rest-api
