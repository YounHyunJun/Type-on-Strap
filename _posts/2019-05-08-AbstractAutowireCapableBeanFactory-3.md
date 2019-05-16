---
layout: post
title: "Abstract Autowire Capable Bean Factory - 3"
subtitle: "Inject Bean's Property"
---

## AbstractAutowireCapableBeanFactory

### 스프링 빈의 프로퍼티를 채우는(주입) 방법

스프링 빈의 설정 방법에 따라 프로퍼티를 채우기 위한 데이터를 가져오는 프로세스가 다르다.

1. 프로퍼티 조회
    1. 빈 내부의 프로퍼티에 직접 주입하는 경우(XML 에서 직접 Property 를 등록할 때 등)
    2. BeanDefinition 의 설정이 byType 이거나 byName 으로 적용된 경우 (XML 에서 빈을 등록할 때 Type 이나 Name 에 의한 등록인 경우 등)
    3. Annotation 을 이용해 등록되는 경우(@Autowired, @Value 등)
2. 주입

프로퍼티 조회를 먼저 살펴보자
첫번째의 경우 PropertyValues 클래스를 이용한 빈의 프로퍼티를 지정하여 설정하기 때문에 주입될 데이터를 찾기가 어렵지 않다.
두번째의 경우 타입이 일치하거나 이름이 일치하는 Bean 을 BeanFactory 에서 조회한다. 생성된 Bean 이 있다면 가져오겠지만 BeanDefinition 으로 존재할 경우
Bean 을 생성하여 가져 올 것이다.
세번째의 경우는 BeanPostProcessor 를 이용한 방법인데 등록된 BeanPostProcessor 중InstantiationAwareBeanPostProcessor 를 구현한 bpp 가 있다면
postProcessPropertyValues 메소드를 이용해 빈을 채운다. 이 방법은 각각의 전략에 따라 빈의 주입 방법을 다양하게 가져갈 수 있다.

주입의 경우 Property 를 Bean 에 주입하기 위한 Bean 내부 데이터에 접근하기 위한 방법을 사용한다.
Bean 내부 변수 혹은 메소드에 쉽게 접근하는 기능을 가진 BeanWrapper(구현체: BeanWrapperImpl) 를 통해 Bean 을 감싸고 이를 주입에 사용하게 된다.
 


