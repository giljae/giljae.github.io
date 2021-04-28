---
title:  "GraphQL과 RESTful"
tags: GraphQL, RESTful
---
![image](https://user-images.githubusercontent.com/111643/116030509-a9435700-a696-11eb-883a-573b22aa402e.png)

GraphQL(Graph Query Language)은 Server API를 만들기 위해 Facebook에서 만든 Query Language입니다.

Query Language는 질의문(Query)과 컴퓨터언어(Language)의 조합입니다. 기존에 RESTful이라는 개념이 존재하였고, 그동안 잘 사용하고 있었는데 Facebook은 왜 이런 개념을 만든 것일까요?

https://graphql.org/blog/graphql-a-query-language/에 의하면 아래와 같은 문제점이 존재했다고 합니다.
* 다양한 기기에서 필요한 정보들을 REST로 일일히 구현하기 힘듬
* 각 기기마다 UI/UX가 다르기에 Server에서 일일히 대응하기가 어려움

그래서 Facebook은 서버가 데이터를 노출하는 방법을 정의한 기술 표준이 필요했고 내부적으로 만들어서 사용하다가 2015년도에 오픈소스로 공개하게 되었습니다.

RESTful과 GraphQL의 차이점은 무엇일까요?

# RESTful과 GraphQL의 차이점
## API Endpoint
REST의 핵심 아이디어는 Resource입니다.각 Resource는 URL로 식별되며 해당 URL 에 요청을 보내 정보를 얻습니다. REST는 Resource의 유형 및 모양과 리소스를 가져오는 방법이 결합된 방식을 제공한다는 점입니다.
<pre>
<code>
GET / books / : id 
GET / authors / : id 
GET / books / : id / comments 
POST / books / : id / comments
</code>
</pre>


GraphQL의 경우는 위에서 언급한 두 개념이 완전히 분리되어 있습니다. 전체 API를 위해서 단 하나의 Endpoint만 사용합니다. github의 경우 [v3 API](https://developer.github.com/v3/)와 [v4의 API](https://developer.github.com/v4/)의 기술 표준이 다릅니다. v3의 경우는 v3 루트 element하위에 각 Resource들의 endpoint가 존재하지만, v4의 경우 루트 element만 존재합니다. 이 의미는 사용측에서 Query 스키마 유형을 만들어야 합니다.
<pre>
<code>
type Query {
  book(id: ID!): Book
  author(id: ID!): Author
}
type Mutation {
  addComment(input: AddCommentInput): Comment
}
type Book { ... }
type Author { ... }
type Comment { ... }
input AddCommentInput { ... }
</code>
</pre>

## API Response
RESTful의 경우 각 API마다 spec이 존재하며, 정의해놓은 format대로 각 Resource의 정보를 Response합니다.

GraphQL의 경우 사용하는 측에서 Response 구조를 원하는 방식으로 변경할 수 있습니다.

이 둘간의 차이점을 보여주는 그림입니다.

![image](https://user-images.githubusercontent.com/111643/116030666-0dfeb180-a697-11eb-8e18-9ef177cd013d.png)

GraphQL을 사용하게되면 아래의 장점을 지니게 됩니다.
* HTTP 요청 횟수를 줄일 수 있기 때문에, Network Latency가 감소됩니다.
* HTTP 응답 Size가 줄게 됩니다.

# 결론
그동안 RESTful은 N+1 쿼리의 제약에서 자유롭지 못했습니다. 특정 사용자가 작성한 글의 모든 내용을 보고 싶다고 하면 모든 글의 목록을 가져오고 내용은 목록 리스트를 루프를 돌면서 fetch를 해야 했었습니다. 그리고 필요 정보만 가져오고 싶어도 API에서 제공해주는 모든 정보를 받은 후에 필터 처리를 해야 했었습니다. 이런 점은 성능에도 영향을 주게 되었고요.

물론, RESTful의 단점을 개선한 GraphQL도 장점만 존재하는 것은 아닙니다. 높은 학습 곡선이 존재하고 적절한 상황에서 사용해야 합니다.
