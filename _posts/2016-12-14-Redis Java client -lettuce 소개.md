---
layout: post
title:  "Redis Java client -lettuce 소개"
comments: true
tags: Redis, Java client, lettuce
---

Lettuce는 synchronous, asynchronous, reactive usage를 지원하는 scalable thread-safe한 Redis Java client 입니다.

>lettuce — IntroductionLettuce is a scalable thread-safe Redis client for synchronous, asynchronous and reactive usage. Multiple threads may…
redis.paluch.biz

Micro Service Architecture(MSA)를 적용하면서, 앞단의 Gateway의 Discovery영역에 redis로부터 active instance를 read하는 부분에서 Jedis를 사용하였지만 Jedis는 Multiple threads상에서 connection pool 성능이 생각보다 만족스럽지 않았고 차선책으로 Lettuce를 사용하였고 결과는 만족스럽습니다.
