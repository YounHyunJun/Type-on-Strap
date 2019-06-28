### Spring Controller Validation

Spring 에서는 Request 의 파라미터를 검증하기 Validator 인터페이스를 제공합니다.<br>
이를 이용하면 파라미터를 쉽게 검증할 수 있습니다.

적용 방법은 다음과 같습니다.

1) @Validated 적용

2) Validation 구현 클래스
- supports() 검증의 적용 여부
- validate() 검증 실행, 실패 시 BindException 예외 호출

3) Validation 적용(Validator 는 Bean 으로 사용)

<hr>
하지만 '레퍼런스 객체가 아닌 기본 타입' 에서는 @Validated 를 이용한 검증이 동작하지 않습니다.
 
그 이유는 레퍼런스 타입 파라미터에 적용되는 ArgumentResolver 구현체인 ServletModelAttributeMethodProcessor 에는 검증로직이 존재하지만,
기본 타입에 적용되는 구현체 RequestParamMethodArgumentResolver 에는 검증 로직이 없기 떄문입니다.

그래서 스프링 만으로는 Controller 의 기본타입 parameter 에 Validator 기능을 사용할 수 없습니다.

- DispatcherServlet 프로세스 과정 참고

다른 방법으로 스프링에서 AOP 등을 이용해 해당 기능을 직접 만들어 사용하거나 검증 기능을 제공하는 오픈 소스를 사용할 수 있습니다. 
오픈소스로는 JSR-303 을 구현하고 Proxy 를 이용해 Parameter 를 검증하는 Hibernate Validator, Apache Bval 등이 있습니다.
이를 이용하면 JSR-303(등) 에서 제공하는 여러 Validation Annotation 과 도구들을 제공받을 수 있어 기능을 구현하기 편리합니다.

Controller Handler 의 Parameter 를 검증하기 위한 로직이 때때로 필요합니다.
여러 방법 중 상황에 맞는 적절한 것을 선택하면 도움이 될 것 같습니다.