---
layout: post
title: "Abstract Autowire Capable Bean Factory - 1"
subtitle: "빈을 생성하는 프로세스"
---

## AbstractAutowireCapableBeanFactory

### RootBeanDefinition 을 이용해 빈 생성을 구현하는 추상 클래스 
### 제공하는 기능: 빈 생성, 속성 주입, 자동 주입, 초기화

![AbstractBeanFactory Diagrams](https://github.com/YounHyunJun/YounHyunJun.github.io/blob/master/img/AbstractAutowireCapableBeanFactory_Diagram.PNG)
#### AbstractBeanFactory 의 추상 메소드인 createBean 을 구현하는 부분이 이 클래스의 핵심이다.

> 실제로 빈을 생성하는 작업을 포함하여 빈에 필요한 여러 정보들을 주입해주는 작업과 초기화 작업을 한다. <br>
> 빈에 대한 추가 정보를 제공하는 일은 스프링의 확장 포인트인 BPP 를 통해 실행된다.

클래스의 핵심 메소드는 createBean 이다. 이 클래스에서는 
1. 빈 클래스 결정
2. 메소드 오버라이드 준비
3. 프록시를 사용할 경우 BPP 를 이용해 프록시를 생성하여 반환
4. 빈 생성

실제로 빈을 만드는 메소드는 doCreateBean() 인데 여기서 빈을 생성하기 전 전처리와 생성 후처리 및 초기화와 관련된 모든 작업을 수행한다.
큰 흐름으로 설명하면 다음과 같다.

1. 빈을 생성한다. (createBeanInstance)
2. 빈에 필요한 데이터를 주입한다. (populateBean)
3. 빈을 초기화 한다. (initializeBean)

빈의 생성은 1) 팩토리 메소드를 이용할 것인지, 2) 파라미터가 존재하는 생성자를 이용할 것인지, 3) 기본 생성자를 이용할 것인지에 따라 프로세스가 달라진다.
이와 같은 이유는 필요에 따라 bean 을 생성하는 방법이 달라 질 필요가 있기 때문이다. 예를들어 파라미터가 있는 생성자를 사용할 경우 여러개의 생성자 중 어떤 생성자를
사용할 지 결정할 수 있어야 하기 떄문이다.
어떤 방법을 사용하더라도 최종적으로는 빈 생성 전략을 이용해 빈을 생성하게 된다. 

> 빈 생성 전략<br>
> 빈 생성을 위한 전략 인터페이스로 InstantiationStrategy 를 사용하는데 Default 는 CglibSubclassingInstantiationStrategy 클래스를 사용한다.
> 이 클래스는 SimpleInstantiationStrategy 를 상속받는데 이 클래스를 보면 실제로 빈을 어떻게 생성하는지 instantiate 메소드를 통해 알 수 있다.

빈이 생성된 이후 빈이 가진 Property 들에게 필요한 데이터를 주입하는 일을 하게 된다.
이 메소드는 BeanPostProcessor 혹은 다른 방법으로 주입되는데 테스트 코드를 작성하며 확인한 방법은 AutowiredAnnotationBeanPostProcessor 를 이용하여
@Autowired 애노테이션이 있는 Property 에 빈을 주입하는 방법이었다. 조금만 설명을 하자면 이 BPP 는 Bean 의 Class 를 리플렉션으로 분석하여 
특정 애노테이션이 작성되어 있으면 관련된 BeanDefinition 을 찾아 생성하고 주입하는 작업을 한다.
여기 까지 진행되면 빈이 생성되고 빈의 Property 에 필요한 값들이 모두 채워지게 된다.

마지막으로 빈을 초기화하는 작업은 크게 4가지의 일을 하는데 다음과 같다.
> 1) Aware 류의 인터페이스를 구현했다면 각각의 Bean 을 채워준다. 예를들어 BeanFactoryAware 를 구현했다면 BeanFactory 를 주입해준다.
> 2) 생성된 빈에 설정되어 있는 모든 BPP 의 전처리를 적용한다.
> 3) Bean 이 InitializingBean 을 구현했다면 afterPropertiesSet 메소드를 실행시켜 준다.
> 4) 모든 BPP 의 후처리기를 빈에 적용해 준다. 

이렇게 빈을 생성 후 주입 및 초기화 작업까지 끝마쳤다.
큼직한 부분으로 잘라 설명했기 때문에 그 안에서 벌어지는 세세한 일들을 살펴보지 못했다. 시간이 된다면 구체적으로 어떤 일들을 하는지 작성할 예정이다.