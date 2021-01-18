---
layout: post
title:  "[Spring] AOP"
subtitle:   "Spring aop"
categories: dev
tags: spring aop 
comments: true
---
# 스프링 AOP

---

## Aspect Oriented Programing

* `AOP란?`쉽게말해 같은일을 여러 메소드에서 실행하게되면 그 코드를 한곳으로 뽑아내 적용시킨다.

* 반대로 생각하면 기존의 코드를 변경하지않고 적용가능하다.

  ex) 메소드 처리 시간 측정을 여러 메소드에 하게되면 시간측정 로직을 뽑아내 적용한다.

  `ex) @Transactional(readOnly = true) 스프링에서 제공하는 AOP`

```java
	/**
	 * Retrieve all {@link PetType}s from the data store.
	 * @return a Collection of {@link PetType}s.
	 */
	@Query("SELECT ptype FROM PetType ptype ORDER BY ptype.name")
	@Transactional(readOnly = true)
	List<PetType> findPetTypes();
```

### 다양한 AOP 구현 방법

1. 컴파일
   - AspectJ 제공
   - 컴파일할때 코드를 추가한다.
2. 바이트 코드 조작
   * AspectJ 제공
   * class파일을 로딩하는 시점에 적용한다.
3. 프록시 패턴
   * Spring AOP가 제공

### 예제 ) 기존코드 건드리지 않고 로직추가 

`지불방식 인터페이스 생성`

```java
public interface Payment {
	void pay(int amount);
}
```

`지불방식 구현`

```java
public class Cash implements Payment {
	@Override
	public void pay(int amount) {
		System.out.println(amount + " 현금 결제");
	}
}
```

`지불방식 사용하는 상점 구현`

```java
public class Store {

	Payment payment;

	public Store(Payment payment) {
		this.payment = payment;
	}

	public void buySomething(int amount) {
		payment.pay(amount);
	}
}
```

`상점에서 지불방식 사용`

```java
class StoreTest {

	@Test
	public void testPay() {
		Payment cash = new Cash();
		Store store = new Store(cashPerf);
		store.buySomething(100);
	}
}
```

* 위 코드에서 지불방식 구현한곳에 거래시간을 측정하고 싶을때 새로운 클래스를 생성해 인터페이스를 상속받고 구현한다.

`인터페이스 상속받는 새로운 클래스 생성`

```java
public class CashPerf implements Payment {

	Payment cash = new Cash();

	@Override
	public void pay(int amount) {
		StopWatch stopWatch = new StopWatch();
		stopWatch.start();
		cash.pay(amount);
		stopWatch.stop();
		System.out.println(stopWatch.prettyPrint());
	}
}
```

`상점에서 지불방식 사용시 새로운 클래스 사용`

```java
class StoreTest {
	@Test
	public void testPay() {
		Payment cashPerf = new CashPerf();
		Store store = new Store(cashPerf);
		store.buySomething(100);
	}
}
```

`거래시간 추가 로그`

```bash
100 현금 결제
StopWatch '': running time = 63298 ns
---------------------------------------------
ns         %     Task name
---------------------------------------------
000063298  100%  
```

---

## Annotation기반 프록시패턴 AOP 구현

`기존로직(위 코드중 cash 역할)`

* 두 메소드에 @LogExecutionTime Annotation을 통해 공통로직 추가

```java
	@GetMapping("/owners/new")
	@LogExecutionTime
	public String initCreationForm(Map<String, Object> model) {
		Owner owner = new Owner();
		model.put("owner", owner);
		return VIEWS_OWNER_CREATE_OR_UPDATE_FORM;
	}

	@PostMapping("/owners/new")
	@LogExecutionTime
	public String processCreationForm(@Valid Owner owner, BindingResult result) {
		if (result.hasErrors()) {
			return VIEWS_OWNER_CREATE_OR_UPDATE_FORM;
		}
		else {
			this.owners.save(owner);
			return "redirect:/owners/" + owner.getId();
		}
	}

```

`annotation생성`

* annotation만 생성해서는 의미가 없다. 구현해주어야한다.
* 어디다 쓸것인가?  `@Target(ElementType.METHOD)` 
* 언제적용할것인가? `@Retention(RetentionPolicy.RUNTIME)`

```java
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
public @interface LogExecutionTime {
}
```

`AOP구현 (위 코드중 CashPerf 역할)`

* ProceedingJoinPoint로 실행될 메소드를 가져온다.

```java
@Component
@Aspect
public  class LogAspect {

	Logger logger = LoggerFactory.getLogger(LogAspect.class);

	@Around("@annotation(LogExecutionTime)")
	public Object logExecutionTime(ProceedingJoinPoint joinPoint) throws Throwable {
		//joinPoint가 메소드
		StopWatch stopWatch = new StopWatch();
		stopWatch.start();

		Object proceed = joinPoint.proceed();

		stopWatch.stop();
		System.out.println(stopWatch.prettyPrint());
		return proceed;
	}
}
```

`log출력 확인`

```bash
StopWatch '': running time = 83815 ns
---------------------------------------------
ns         %     Task name
---------------------------------------------
000083815  100%  
```



---

## 여러곳에서 사용할 로직을 한곳에서 구현에서 여러곳에 적용하는것이라고 생각하자!
