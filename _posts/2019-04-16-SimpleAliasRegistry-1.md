---
layout: post
title: "Simple Alias Registry - 1"
---

## SimpleAliasRegistry

### AliasRegistry 의 구현체
### 별칭을 관리하는 저장소

![SimpleAliasRegistry Diagrams](https://raw.githubusercontent.com/YounHyunJun/YounHyunJun.github.io/master/img/Alias_Diagram.PNG)

AliasRegistry 는 말그대로 별칭을 관리하는 저장소에요 

기본 자료구조는 ConcurrentHashMap 을 사용하고 있지요.

```java
private final Map<String, String> aliasMap = new ConcurrentHashMap<>();
``` 

이 클래스에는 aliasMap 변수에 별칭을 등록/삭제/수정/조회 등의 관리하는 기능의 메소드들이 존재해요

이상한 점은 제가 못찾는 건지 스프링 소스에서 해당 클래스의 등록/조회에 참조되는 메소드를 못찾겠다는 점인데..

실제로 사용되고 있는지 궁금하네요

이 부분을 아시는 부분이 있으면 공유해주세요..

다음은 DefaultSingletonBeanRegistry 에 대해서 포스팅할 예정이에요