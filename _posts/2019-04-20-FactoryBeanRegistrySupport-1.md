---
layout: post
title: "FactroyBean Registry Support - 1"
---

## AbstractBeanFacotry

### FactoryBean 지원을 위한 SingletonBeanRegistry 

![AbstractBeanFactory Diagrams](https://github.com/YounHyunJun/YounHyunJun.github.io/blob/master/img/FactroyBeanRegistry_Diagram.PNG)

- 이 클래스는 Singleton Bean 저장소로 사용되는 Bean Factory 의 FactoryBean 클래스를 지원합니다.
- Factory Bean 에 대한 Cache Map 을 가지고 있습니다.

> FactoryBean 란 구체적으로 일반적이지 않은 방법으로 생성되는 (생성자가 private) static 메소드로 생성되는 등의 클래스일 경우
> FactoryBean 을 이용해 getObject() 메소드와 getObjectType() 메소드를 구현해 줌으로 Spring 의 Bean 으로 등록할 수 있습니다. <br/>

이 클래스의 메소드를 살펴봄으로 팩토리 빈을 통해 어떤 기능을 지원하는지 알 수 있습니다. 

- 팩토리 빈을 통한 클래스 타입 / 빈을 찾는 경우
    - getTypeForFactoryBean(): 팩토리 빈에서 설정된 객체의 타입을 조회
    - getCachedObjectForFactoryBean(): 캐시가 존재한다면 캐시에서 빈을 조회
    - getObjectFromFactoryBean(): 팩토리 빈에서 Bean 으로 사용할 객체를 조회 
    
- 확장 포인트를 제공 (필요에 따라 상속을 통해 사용)
    - postProcessObjectFromFactoryBean(): 팩토리 빈을 통한 BeanPostProcessor 를 이용해 확장 포인트를 제공한다. 
    - removeSingleton: 팩토리 빈 제거와 관련된 기능 제공