---
layout: post
title:  "Java class를 Python에서 사용하기"
comments: true
tags: Python, Java
---

프로젝트내에서 만든 Java Util class를 Python에서 사용해야 하는 케이스가 발생했다. py4j, jnius, subprocess .., 등등의 방법이 있었다.

# py4j
GatewayConnection 방식으로 진행하기에 내부적으로 socket을 사용함. Fault 발생 여지가 있어서 부적절하다고 판단

# jnius
Java class를 수행전에 JVM을 start 해야하고 종료시 shutdown 해야 함. Fault 발생 여지가 있어서 부적절하다고 판단

# subprocess
os command를 수행하는 방법, 별도의 package를 설치하지 않아도 되고, 위의 package에 비해 fault 발생 여지가 작다고 판단 아래는 샘플 코드 jar파일은 executable jar여야 한다.

## python 2.7
<script src="https://gist.github.com/giljae/c608c14ce3c2db7810c568f4207f4c14.js"></script>

## python 2.7/2.6
<script src="https://gist.github.com/giljae/67adbd991b69b75754d1f6b6519d5244.js"></script>
