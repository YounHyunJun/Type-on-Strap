---
layout: post
title: "Abstract Autowire Capable Bean Factory - 2"
subtitle: "Create Bean"
---

## AbstractAutowireCapableBeanFactory

### 스프링에서 빈을 생성하는 구체적인 전략

스프링이 생성하는 빈의 자세한 코드의 흐름을 보려면 클래스 내 createBeanInstance 를 보면 된다.<br>
이 메소드에서는 세 부분으로 구분하여 빈을 생성한다.

1. FactoryBean 을 통해 빈을 생성하는 경우
    - FactoryBean 의 getObject 메소드를 호출하여 빈을 생성한다.
2. Arguments 가 존재하는 생성자로 빈을 생성하는 경우
    - 여러개의 생성자 중 우선순위가 높은 생성자를 선택해 BeanFactory 에서 Arguments 를 주입받아 빈을 생성한다.
    - 생성자 선정 방법과 Arguments 조회 방법은 ConstructorResolver 클래스에서 확인할 수 있다.
3. 기본 생성자로 빈을 생성하는 경우
    - 클래스가 가진 기본 생성자로 빈을 생성한다.

빈을 생성하는 기능은 셋 모두 InstantiationStrategy (인스턴스 생성 전략) 인터페이스를 상속하는 클래스를 이용한다.
이 클래스에는 instantiate 메소드가 있는데 기본 생성자 방식, Arguments 를 가지는 생성자 이용 방식, FactoryBean 이용 방식으로 사용할 수 있도록 오버로딩 되어있다.

이 클래스의 전략을 구현하는 방법에 따라 일반적인 방법으로 빈이 생성되거나 Proxy 의 형태로 생성 될 수 있다.
스프링의 기본 빈 생성전략 클래스는 CglibSubclassingInstantiationStrategy 를 사용하고 있기 때문에 설정에 따라 Cglib 을 이용해 Proxy 빈을 생성하기도 한다.

 
 