---
layout: post
title: "Default Singleton Bean Registry"
---

## DefaultSingletonBeanRegistry

### SingletonBeanRegistry 의 구현체
### 공유된 빈 객체들을 위한 저장소

> 앞으로 보완하며 나아갈 예정입니다.

스프링을 사용하다 보면 기본적으로 빈들이 싱글톤이라는 사용된다는 것을 들어 본 적 있으실 거에요
이 클래스가 바로 그 싱글톤 빈들을 관리하는 중요한 역할을 맡고 있어요

클래스 내부의 singletonObjects 라는 Map 변수가 바로 그 주인공 이지요

1. 빈 등록

빈을 등록할 경우 빈 이름이 필요해요. Map 변수에 담기 때문에 이름과 빈 객체를 맵핑하기 떄문이거든요
공유된 객체의 저장소를 Map 으로 사용하기 때문에 싱글톤 공유 객체가 될 수 있겠네요

- 이 클래스에서 사용하는 저장소 Map 변수들이 많아요
    - singletonObjects, singletonFactories, earlySingletonObjects, registeredSingletons 4가지가 있어요

실제로 빈을 등록하면 이 중 singletonObjects 와 registeredSingletons 두군데에 저장되는 거 같아요 

2. 빈 조회

빈을 조회할 때는 singletonObjects Map 을 확인하는데, 이곳에 있으면 반환시켜주지만 없는 경우에는..
earlySingletonObjects 를 확인해서 반환하거나 singletonFacotires 를 이용해서 객체를 얻어오는 거 같아요

