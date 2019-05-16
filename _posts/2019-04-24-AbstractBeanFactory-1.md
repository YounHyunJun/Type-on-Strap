---
layout: post
title: "Abstract Bean Factory - 1"
---

## AbstractBeanFacotry 첫번째

### ConfigurableBeanFactory 의 구현체
### Bean Factory 구현을 위한 추상 기본 클래스

스프링 코드는 클래스를 구현할 때 자신의 SuperClass 에 대해서 주석으로 층을 나누어 구분했습니다.
각 구분에 대해 중요한 부분을 언급하는 것이 이 클래스를 설명하는데 도움이 될 것 같습니다.

![AbstractBeanFactory Diagrams](https://github.com/YounHyunJun/YounHyunJun.github.io/blob/master/img/AbstractBeanFactory_Digagram.PNG)

여기서는 위의 그림에서 빨간 부분인 인터페이스를 구현하는 부분, 자신의 메소드를 구현하는 부분, 확장을 위한 추상메소드를 구현하는 부분으로 나뉘어 설명합니다. 

1. **인터페이스** 빈 팩토리 구현 메소드 (BeanFactory)

    - getBean()
        - 이 클래스에서 모든 Bean 을 얻어오는 메소드는 doGetBean() 메소드로 전달 됩니다.
        - 이 메소드의 핵심은 존재하는 빈이 있다면 반환하고 없다면 만들어주는 역할입니다.
        - 빈을 만들어 주는 경우 5번 항목의 createBean 추상 메소드를 실행해 하위 클래스에게 역할을 위임합니다.

2. **인터페이스** 계층 빈 팩토리 구현 메소드

    - getParentBeanFactory()
        - 빈 팩토리는 계층구조가 가능합니다. 가지고 있는 참조된 부모의 빈 팩토리를 전달합니다. 
        - 이를 사용해 자신에게 없는 빈의 경우 부모에게 요청할 수 있습니다.

3. **인터페이스** 설정가능한 빈 팩토리 구현 메소드

    - 클래스 로더 설정
    - Property Editor 설정 (TypeConverter 설정)
    - BeanPostProcessor 설정
    - Scope 설정
    등..

4. **자신** 의 메소드

5. **추상** Template methods (하위 클래스에서 확장하여 구현해야할 메소드)
    - containsBeanDefinition
    - getBeanDefinition
    - createBean