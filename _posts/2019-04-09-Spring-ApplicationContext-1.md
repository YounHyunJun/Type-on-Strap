---
layout: post
title: "Spring ApplicationContext"
---

## Spring Framework

#### 스프링의 핵심이자 자원을 관리하는 ApplicationContext

> 주관적인 생각이니 언제든 보완해주시면 감사하겠습니다.

최근에 스프링 스터디를 하며 느끼는 것들에 대한 정리입니다.

매주 토비의 스프링과 스프링 코어에 대한 스터디를 하며 느낀 바가 많아 이를 글로 남기고 싶었는데 

혹시 최근 스프링에 관심을 가지고 내부를 들여다보려는 초심자분들이 있다면 도움이 될 것 같습니다

ApplicationContext 정의는 주석에서 찾아볼 수 있는데 다음과 같습니다.

Central interface to provide configuration for an application.
(우리가 개발하는 애플리케이션을 위한 설정을 제공해주는 중요한 인터페이스)

여기서 애플리케이션을 위한 설정을 제공해준다는 것이 무엇인지 이해하기 쉽지 않아 이를 위한 추가 설명이 보입니다.

ApplicationContext 가 제공해주는 설정 목록.

1. 애플리케이션 컴포넌트에 접근을 위한 BeanFactory Method
2. 일반적인 방식으로 파일 자원을 읽어오는 기능 
3. 등록된 listener 에게 이벤트를 전달하는 기능
4. 메시지를 처리하고 국제화를 제공하는 기능
5. 부모 컨텍스트로 부터의 상속

그리고 1~4번까지 ApplicationContext 가 상속하는 특정 인터페이스들이 명시되어 있습니다.

1. ListableBeanFactory
2. ResourceLoader
3. ApplicationEventPublisher
4. MessageSource

이를 정리해보면 
애플리케이션을 만들때 여러가지 설정이 필요할 것이고 그런 설정들 중에서 중요할법한 기능들을 ApplicationContext 에서 지원해줄거야
첫째로 애플리케이션의 컴포넌트들을 관리해 줄 것이고, 둘째로 필요한 자원들을 가져올 수 기능들을 제공할거야, 셋째로 이벤트가 발생했을 때 
전달이 필요하다면 이벤트를 전달해주는 기능도 있을거야, 마지막으로 국제화(여러 언어 지원)가 필요하다면 이에 대한 기능을 지원할거야.
혹시 Context 간 상속이 필요하다면 이것도 지원해주고 있어.

이정도로 정리가 될 거 같네요
ApplicationContext 에 대해서 알고 싶으면 이 4가지 개념들을 자세히 살펴보면 될 거 같다는 느낌이 들어요  

사실 여기서 가끔 면접문제나 질문에 등장하는 ApplicationContext 와 BeanFactory 의 차이가 보이는 거 같아요
일차적으로 애플리케이션은 빈 팩토리를 포함하는 개념이니까요.

> Factory 는 아시다시피 공장이니까 무언가를 만들어 내는 친구라고 볼 수 있을 것 같아요
> 공장은 무언가를 생산해서 만들어 주는 역할을 하는데 BeanFactory 는 빈을 만들어서 주는 역할이라는 생각이 드네요
> 이 클래스의 가장 중요한 메소드라고 생각되는 getBean 이 그런 역할을 한다고 보면 될 것 같아요

하지만 실제로 ApplicationContext 에서 사용하는 BeanFactory 는 훨씬 다양한 일들을 하고 있어요
그래서 이 BeanFactory 를 구현한 상세화된 클래스들이 있지요 

이 4가지 중 다음에는 BeanFactory 에 대해서 자세하게 알아보면 좋을 거 같아요

