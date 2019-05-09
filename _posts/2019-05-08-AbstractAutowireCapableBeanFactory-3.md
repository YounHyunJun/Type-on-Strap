---
layout: post
title: "Abstract Autowire Capable Bean Factory"
subtitle: "Inject Bean's Property"
---

## AbstractAutowireCapableBeanFactory

### 스프링 빈의 프로퍼티를 채우는(주입) 방법

스프링 빈의 설정 방법에 따라 프로퍼티를 채우기 위한 데이터를 가져오는 프로세스가 다르다.

1. 빈 내부의 프로퍼티에 직접 주입하는 경우(XML 에서 직접 Property 를 등록할 때 등)
2. BeanDefinition 의 설정이 byType 이거나 byName 으로 적용된 경우 (XML 에서 빈을 등록할 때 Type 이나 Name 에 의한 등록인 경우 등)
3. Annotation 을 이용해 등록되는 경우(@Autowired, @Value 등)

첫번째의 경우 빈의 프로퍼티를 직접 지정하여 설정하기 때문에 주입될 데이터를 찾기가 어렵지 않다.  

