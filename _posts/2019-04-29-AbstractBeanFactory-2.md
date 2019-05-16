---
layout: post
title: "Abstract Bean Factory - 2"
---

## AbstractBeanFacotry 두번째

### ConfigurableBeanFactory 의 구현체
### Bean Factory 구현을 위한 추상 기본 클래스

AbstractBeanFactory 클래스의 중요한 기능 중 하나인 Bean 을 조회하는 getBean 에 대해서 알아보자

getBean 메소드는 파라미터에 따라 여러가지 방법으로 호출되지만 대부분은 내부적으로 doGetBean 이라는 메소드를 호출한다.

여기서는 doGetBean 메소드에 대해서 설명하고자 한다.

doGetBean's Process

1. 싱글톤 빈이 존재하는 경우(자신 혹은 부모)
    1. Singleton Bean 을 조회한다. (DefaultSingletonBeanRegistry 의 변수인 singletonObjects Map 에서 조회한다.)
        - Singleton Bean 이 존재하면 이를 반환한다하고 종료. (빈 팩토리인 경우 빈 팩토리를 통해 반환)
    2. 부모 빈 팩토리에서 Singleton Bean 을 조회한다.
        - Singleton Bean 이 존재하면 이를 반환하고 종료. (빈 팩토리인 경우 빈 팩토리를 통해 반환)

2. 싱글톤 빈이 존재하지 않는 경우    
    1. Singleton Bean 이 존재하지 않기 때문에 빈을 생성해야 한다. 빈을 생성하겠다는 마킹을 한다. (typeCheckOnly 파라미터 에 따라 구분)
    2. beanName 으로 등록된 합성된(Merged) RootBeanDefinition 을 가져온 후 체크한다.
        1. MergedBeanDefinition 을 조회한다.
            - AbstractBeanFactory 의 변수 mergedBeanDefinitions 에서 beanName 으로 조회한다.
            - mergedBeanDefinitions Map 에 없고 BeanDefinition 에 parentName 도 없으면 BeanDefinition 이 Root 인 경우 복사하고 아닌 경우 RootBeanDefinition 으로 생성한다.
            - mergedBeanDefinitions Map 에 없고 BeanDefinition 에 parentName 이 있으면 기존 RootBeanDefinition 에 생성하려는 BeanDefinition 을 Override 한다.
        2. MergedBeanDefinition 을 체크한다.
            - Abstract 클래스 인 경우 예외를 던진다.
            - Arguments 가 있는데 Prototype 스코프가 아닌 경우 예외를 던진다.
    3. dependsOn Bean 이 있는 경우 이들을 먼저 처리한다. 
        - getBean 메소드를 통해 해당 Bean 들을 전달하여 호출한다.(생성시키는 목적으로 보인다.)
        - 해당 빈 이름들을 종속성 관리 저장소에 저장한다. (dependentBeanMap)
    4. 빈을 생성한다.
        1. Scope Singleton 인 경우
            - 싱글톤 빈을 생성
        2. Scope Prototype 인 경우
        3. 그 외의 Scope 경우
            - 프로토타입에 대한 전 처리
            - 프로토타입 빈 생성
            - 프로토타입에 대한 후처리
    5. 생성된 빈을 반환한다.
    

