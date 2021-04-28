---
layout: post
title:  "Websocket Proxy"
comments: true
tags: Websocket, Proxy
---

Websocket proxy가 필요한 케이스가 생겼다. 일반적으로는 독립된 proxy server를 통해 지원하면 되지만, Servlet단에서 지원을 해야 하는 상황이다. 아래의 그림처럼 제공되는 것이 중간의 Gateway를 통해 Communication이 되어야 하는 상황이다.

![image](https://user-images.githubusercontent.com/111643/115679825-eef9da00-a38d-11eb-83dd-0ca598f6b8ac.png)

아래의 그림처럼 중간에 다른 서버를 거쳐야 한다.

![image](https://user-images.githubusercontent.com/111643/115679855-f6b97e80-a38d-11eb-9f42-c259591cefd3.png)

가장 편하게 할 수 있는 방법은 jetty-proxy library를 이용하는 방법이다.

이미 구현체가 존재하기 때문에, 필요한 부분만 customizing하여 Servlet으로 등록하면 된다.
