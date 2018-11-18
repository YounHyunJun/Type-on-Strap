---
layout: post
title: "토비의 스프링 AOP"
---

# 토비의 스프링 AOP 를 읽고..

짧막하게나마 토비의 스프링 6장 AOP를 읽고 느낀점을 주관적인 글로 써보려 한다. 굉장히 개인적인 글이니 틀린 부분이 있다면 꼭 알려주길 바란다. 그래야 서로 실력이 늘테니.. 

부족한 글 재주로 생각을 나누려니 창피하기도 하고 누가 이 글을 읽을까 싶지만 혹시라도 이 부족한 글을 이해하려면 앞 장에 나왔던 내용들을 읽어보면 좋을 것 같다. 

- 1장 오브젝트와 의존관계, 2장 테스트, 3장 템플릿, 4장 예외, 5장 서비스 추상화

---

이 글의 핵심은 **왜 AOP를 써야하는가**, 곧 AOP 가 탄생한 이유이다.

AOP는 관심사의 분리 때문에 등장하게 되었다. 

비즈니스 로직과 엉켜있는 다양한 부가기능들은 우리를 혼란스럽게 만들고 중복된 코드를 만든다. 
스프링에서는 부가기능을 하나의 애스팩트로 보고 비즈니스 로직에서 분리해 각각의 관심사를 분리시킬 수 있게되었다.

예를 들어 보자, 이 코드는 5장의 코드이다.
```java
public void upgradeLevels() throws SQLException {
        TransactionStatus status = transactionManager.getTransaction(new DefaultTransactionDefinition()); // 트랜잭션 시작
        try {
            // 사용자 레벨 업그레이드
            transactionManager.commit(status); // 트랜잭션 종료
        } catch (Exception e) {
            transactionManager.rollback(status); // 트랜잭션 종료
            throw e;
        }
    }
```  

> 레벨을 업그레이드 하는 기능을 트랜잭션으로 처리하기 위한 경계를 설정하는 부분이 비즈니스 로직과 엉켜있다.
우리가 집중하고 싶은 부분은 단지 사용자 레벨을 업그레이드하고 싶은데 트랜잭션 처리와 섞이니 정확한 기능을 파악하기 어렵다.

트랜잭션이라는 관심사와 사용자 레벨 업그레이드라는 관심사를 분리하는게 AOP의 시작이다.

#### 첫 번째로 위임이라는 방법으로 관심사의 분리를 시도한다. 
서비스 클래스를 인터페이스,
트랜잭션을 구현한 클래스, 비즈니스 로직을 구현한 클래스로 분리한다.

```java
public interface UserService {
    upgradeLevels();
}

public class TransactionService implements UserService {
    upgradeLevels() {
        // 트랜잭션 시작
        // 사용자 레벨 업그레이드 호출
        // 트랜잭션 종료
    }
}

public class BusinessService implements UserService {
    upgradeLevels() {
        // 사용자 레벨 업그레이드
    }
}
```  
> 클라이언트는 트랜잭션 서비스를 사용하고 트랜잭션 서비스는 비즈니스 서비스를 사용하게 되면,
 클라이언트는 트랜잭션이 설정된 비즈니스 로직을 사용할 수 있고 두가지 관심사가 각각의 클래스로 나뉘게 된다.

이때의 장점은 비즈니스와 트랜잭션 로직을 수정할 때 다른 기술적인 내용에 대해서 신경쓰지 않아도 되고,
비즈니스 로직에 대한 테스트를 손쉽게 만들어 낼 수 있다.

이와 같이 부가기능(트랜잭션)을 처리하고 비즈니스 로직으로 기능을 위임하는 기능을 프록시라 한다.

* 하지만 첫번째 방법은 강력하지만 사람들이 잘 사용하지 않는 이유가 있다.
    * 타깃의 인터페이스를 구현하는 메소드에 대한 위임 코드를 작성하기가 번거롭다.
    * 부가기능 코드가 중복될 가능성이 높다. 업그레이드 레벨 뿐만 아니라 다른 메소드에도 트랜잭션이 필요하다면
    트랜잭션 기능을 메소드마다 추가해주어야 한다.
    
### 둘째 다이나믹 프록시를 적용한다.

다이나믹 프록시는 런타임 시 다이나믹하게 만들어지는 오브젝트이다. 이 프록시는 자동으로 모든 구현체를 만들어 기능을 위임시켜 주지만
부가기능 코드는 직접 작성해야 한다. 또한 파라미터로 인터페이스를 전달받아 리플렉션으로 메소드를 모으고 InvocationHandler 를 이용해 
각 메소드를 호출시켜주는 일을 한다.

```java
Hello proxiedHello = (Hello) Proxy.newProxyInstance(
        getClass().getClassLoader(),
        new Class[] { Hello.class }
        new UppercaseHandler(new HelloTarget()));

public class UppercaseHandler implements InvocationHandler {
    private Hello Target;
    public Object invoke(Object proxy, Mehtod method, Object[] args) throws Throwable {
        // 비즈니스 로직
        method.invoke(target, args);
        // 부가기능 제공
    }
}
```
다이나믹 프록시가 받는 모든 요청은 InvocationHandler 의 invoke 메소드의 파라미터로 전달된다. invoke 에서는 전달받은 메소드를 가지고 
타겟 객체를 주입해 실행 시킨다.

다이나믹 프록시가 많은 장점을 제공해주지만 스프링의 빈으로 사용하기엔 무리가 있다. 생성시에 생성자를 통해 생성하지 못하고
정적 메소드를 통해 생성하기 때문에 자체로는 빈으로 등록할 수 없다.
스프링에서는 스프링을 대신해 오브젝트의 생성 로직을 담당하는 특별한 팩토리 빈을 제공한다.

```java
public interface FactoryBean<T> {
    T getObject() throws Exception; // 빈 오브젝트를 생성해 돌려준다.
    Class<? extends T> getObjectType; // 생성되는 오브젝트의 타입을 알려준다.
    boolean isSingleton(); // 싱글톤 오브젝트의 여부를 판별한다.
}
```

시간이 부족하므로 나중에..

 
  

