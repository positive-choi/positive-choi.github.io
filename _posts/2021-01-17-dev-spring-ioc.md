---
layout: post
title:  "[Spring] IoC"
subtitle:   "Spring doc"
categories: dev
tags: spring ioc
comments: true
---

# 스프링 IoC

## 제어권의 역전

---

* 기존 상식으론 자신`Controller`이 의존성을 만들어서 사용한다.

```java
private OwnerRepository repository = new OwnerRepository();
```

* Spring을 사용하다보면 자신이 의존성을 만들지 않는다.
* 아래 코드의 경우 생성자를 통해 의존성을 받아온다.

```java
	private final OwnerRepository owners;

	private VisitRepository visits;

	public OwnerController(OwnerRepository clinicService, VisitRepository visits) {
		this.owners = clinicService;
		this.visits = visits;
	}
```

* 의존성을 주입해주는것을 injection이라고 한다.
* OwnerRepository와 VisitRepository가 없으면 생성자는 실행될 수 없다
* Spring이 관리하는 객체를 bean이라고 하는데 Spring이 그 bean들을 생성자에 주입해준다.

---

## IoC 컨테이너

### IoC 컨테이너의 역할

* 빈(bean)을 만들고 엮어주며 제공해준다.
* IoC 컨테이너가 bean으로 등록되어있는것만 주입해줄 수 있다.
* BeanFactory가 IoC 컨테이너인데 BeanFactory를 보통 BeanFactory를 상속받은 ApplicationContext를 사용한다.

**ApplicationContext 역할 확인하기**

```java
	private final OwnerRepository owners;

	private VisitRepository visits;

	private ApplicationContext applicationContext;

	public OwnerController(OwnerRepository clinicService, VisitRepository visits, ApplicationContext applicationContext) {
		this.owners = clinicService;
		this.visits = visits;
		this.applicationContext = applicationContext;
	}

	@GetMapping("/bean")
	@ResponseBody
	public String bean() {
		return "bean : " + applicationContext.getBean(OwnerRepository.class) + "\n"
			+ "owners : " + this.owners;
	}
```

`localhost:8080/bean 결과`

```
 bean : org.springframework.data.jpa.repository.support.SimpleJpaRepository@4b835efe
owners : org.springframework.data.jpa.repository.support.SimpleJpaRepository@4b835efe
```

* 같은 객체인것을 확인할 수 있다.
* 이렇게 객체하나를 Application 전반에서 사용하는걸 **싱글톤스코프**라고 한다.

---

## 빈(Bean)

### IoC 컨테이너가 관리하는 객체를 Bean이라고 한다.

* 즉 ApplicationContext에서 가져올 수 있는 객체들이 Bean이 된다.

**Bean으로 생성방법**

 1. ComponentScan

    - @ComponentScan Annotation이 붙어있는 위치부터 하위클래스에

      @Component Annotation을 찾아 모두 Bean으로 등록한다.

      Component를 포함한 Annotation(Repository, Service, Controller, Configuration등)

    2. Bean으 로 직접 등록

    - @Configuration과 @Bean으로 등록한다.

    - @Configuration도 @Component이기 설정되있기에 @ComponentScan이 찾아 bean으로 등록한다

      ```java
      @Configuration
      public class SampleConfig {
      	@Bean
      	public SampleController sampleController (){
      		return new SampleController();
      	}
      }
      ```

---

## 의존성 주입 (Dependency Injection)

### @Autowired

 1. 사용할수있는 지점

    - 생성자(스프링 4.3 이상이면 생성자가 하나고 주입받을 객체가 bean으로 등록되있을 경우 생략가능하다.)

      ```java
      	@Autowired
      	public OwnerController(OwnerRepository clinicService, VisitRepository visits) {
      		this.owners = clinicService;
      		this.visits = visits;
      	}
      ```

    - 필드

      ```java
      	@Autowired
      	private final OwnerRepository owners;
      ```

    - Setter

      ```java
      	@Autowired
      	public void setVisits(VisitRepository visits) {
      		this.visits = visits;
      	}
      ```

    2. 추천하는 방법

    - 생성자에 선언 하는것을 추천한다.
      1. 필수적으로 필요한 Bean이 없을 경우 생성되지않는다.
    - 예외의 경우(순환참조일때)
      1. a가 b를 참조하고 b가 a를 참조할때는 setter를 사용한다.

---



















