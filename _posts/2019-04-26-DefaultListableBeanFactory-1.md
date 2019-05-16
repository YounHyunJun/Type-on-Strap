---
layout: post
title: "Default Listable Bean Factory - 1"
---

## DefaultListableBeanFactory

### ListableBeanFactory 와 BeanDefinitionRegistry 에 관한 기능을 지원하는 클래스
### 단독으로 빈 팩토리로서 작동할 수 있는 클래스

![DefaultListableBeanFactory Diagrams](https://github.com/YounHyunJun/YounHyunJun.github.io/blob/master/img/AbstractAutowireCapableBeanFactory_Diagram.PNG)

#### 빈 팩토리로 동작할 수 있는 클래스로 GenericApplicationContext 에서 Default 로 사용되는 BeanFactory 이기도 하다.

이 클래스를 이해하기 위해서 ListableBeanFactory 와 BeanDefinitionRegistry 의 역할을 이해할 필요가 있다.
ListableBeanFactory 는 BeanFactory 의 기능을 확장한 인터페이스로 getBean 메소드를 위해 Bean Definition 을 가져올 수 있는 메소드들을 지원한다.

BeanDefinitionRegistry 는 BeanDefinition 을 관리의 관점에서 지원되는 기능의 인터페이스다

이 두 인터페이스는 어떤면에서 같은 메소드를 가지기도 하고 서로 다른 메소드를 가지기도 하는데 전자는 bean 을 가져오는 목적에 초점을 두는 반면 후자는 빈을 관리하는 부분에 관심을 가지고 있다.

이 두 인터페이스를 모두 구현하는 DefaultListableBeanFactory 는 BeanDefinition 의 관리 기능을 지원하며 bean 을 가져오는 역할을 수행한다고 볼 수 있겠다.

여기서 등장하는 BeanDefinition 은 bean 의 메타정보에 해당하는 데이터로 이 클래스에서 중요한 역할을 한다.

> BeanDefinition<br>
> Bean Factory 에서 Bean 을 만들 때 필요한 핵심 정보들을 담는다. 몇 가지 필수항목을 제외하면 컨테이너에 미리 설정된 디폴트 값이 적용된다. <br>
> 여러 개의 빈을 만드는 데 재사용 될 수 있고 그에 따라 빈의 이름이나 아이디를 나타내는 정보는 Bean Factory 에 등록될 때 부여한다.  

주로 스프링에서 ApplicationContext 를 만들게 되면 설정에 따라(XML 이나 애노테이션 등) 빈 메타 정보들을 읽어와 빈으로 등록시켜 주는데,
이때 빈으로 등록 되기 전 이 클래스에서 메타 정보들에 대한 Bean Definition 을 만들어 등록해 준다. 실제로 Bean 을 만드는 부분은 모든 BeanDefinition 이 등록된 이후이다.

우리가 빈을 검색할 때 타입이 존재하지 않거나 2개 이상인 경우 NoSuchBeanDefinitionException 혹은 NoUniqueBeanDefinitionException 을 마주하게 된다.
빈을 타입으로 검색할 때 이런 경우가 발생하는데 이 클래스에서 제공하는 getBean 메소드의 기능이다. 적절한 타입이 하나만 있는 경우 혹은 적절한 자료구조를 사용하면 
AbstractBeanFactory 의 getBean 메소드로 작업을 위임하지만 그 외의 경우 상황에 걸맞는 Exception 을 발생시킨다.
  

