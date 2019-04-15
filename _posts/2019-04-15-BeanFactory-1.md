---
layout: post
title: "Bean Factory"
---

## Bean Factory

### 스프링의 빈(객체)을 관리하는 공장

> 주관적인 생각이니 언제든 보완해주시면 감사하겠습니다.

ApplicationContext 에서 중요한 부분을 차지하는 BeanFactory 는 스프링에서 사용되는 모든 빈들을 관리해요.

관리라는 말을 조금 더 분해하면 크게 생성/수정/삭제/조회(CRUD) 의 기능을 한다고 볼 수 있겠네요.

하지만 신기한 건 BeanFactory interface 에는 getBean 이라는 조회 기능의 메소드만 들어가 있죠. 왜 그럴까요

BeanFactory 의 기초적인 메소드를 구현하는 Spring 의 AbstractBeanFactory 를 통해서 이유를 찾을 수 있을 거 같아요

이 클래스는 다음과 같죠

```java
public interface ConfigurableBeanFactory extends HierarchicalBeanFactory, SingletonBeanRegistry {}

public abstract class AbstractBeanFactory extends FactoryBeanRegistrySupport implements ConfigurableBeanFactory {}
```

여기서 중요하게 봐야할 부분은 ConfigurableBeanFactory 를 구현한다는 점이에요
