## TIL - Transactional
### 1. Transaction
- 트랜잭션(Transaction)은 데이터베이스에서 데이터에 대한 하나의 논리적 실행단위이다.
- 데이터베이스 시스템은 각각의 트랜잭션에 대해 ACID(원자성, 일관성, 독립성, 영구성)을 보장한다.
- 트랜잭션을 사용하는 이유는 무결성 유지(Integrity Consistency)를 위해서이다.
- 트랜잭션으로 끼어듬 없이 독립적으로 수행되어야 하는 일련의 연산들을 한 동작으로 묶어 처리하는데, 성공한 경우 데이터베이스에 커밋하고 실패한 경우 롤백하여 원래 상태로 되돌린다.
### 2. Spring의 Transaction
- `@Transactional`: Spring에서 트랜잭션을 적용하기 위해 사용하는 애노테이션.
  메서드 또는 클래스 수준에서 붙여 트랜잭션 관리 범위를 지정한다. 메서드에 붙인 경우 해당 메서드를 호출할 때 트랜잭션을 시작하고 메서드가 완료되면 트랜잭션을 커밋 또는 롤백한다.
- `@EnableTransactionManagement`: Spring 애플리케이션에서 트랜잭션 관리를 활성화하는 애노테이션. (`@SpringBootApplication` 애노테이션에 포함되어 있다.)
### 3. 주로 Service 클래스 또는 메서드에 @Transactional을 붙이는 이유
- Transaction이 필요한 것은 주로 비즈니스 로직이 실행되는 메서드이다. 이 때문에 @Transactional을 서비스 클래스나 서비스 클래스의 메서드에 붙이는 것이 일반적이다.

```java
@Transactional(readOnly = true)
public class DefaultFooService implements FooService {

	public Foo getFoo(String fooName) {
		// ...
	}

	// 메서드의 설정이 우선순위가 높다
	@Transactional(readOnly = false, propagation = Propagation.REQUIRES_NEW)
	public void updateFoo(Foo foo) {
		// ...
	}
}
```

### 4. @Transactional의 롤백 시점
- 기본적으로 롤백은 UnCheckedException(RuntimeException)과 Error에 대해 발생한다. CheckedException은 롤백이 진행되지 않는다.
- 따라서 CheckedException에 대해서도, 즉 모든 예외에 대해 롤백하고 싶다면 `rollbackFor = {Exception.class}` 설정이 필요하다.

### 5. @Transactional의 추가 설정
- 전파 유형, 격리 수준, 읽기 전용(readOnly=true) 등을 설정할 수 있다.
![image](https://github.com/seohyun-lee/TIL/assets/32611398/5684788c-bb3d-4d57-a053-4fc318b5b186)


## 레퍼런스
https://docs.spring.io/spring-framework/reference/data-access/transaction/declarative/annotations.html#transaction-declarative-annotations-method-visibility
https://www.baeldung.com/transaction-configuration-with-jpa-and-spring
https://javatute.com/spring/transactional-rollbackfor-example-using-spring-boot/
