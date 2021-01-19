---
layout: post
title:  "[JAVA] Study-2 primitiveType,referenceType,literal,scope"
subtitle:   "primitiveType,referenceType,literal,scope"
categories: dev
tags: java primitiveType referenceType literal scope
comments: true
---
# 자바 스터디 2주차 primitiveType,referenceType,literal,scope

---

# 프리미티브 타입 종류와 값의 범위 그리고 기본값

* 8가지가 있다.
* 값의 범위는 1byte는 8bit이다. 

| 타입   | 종류         | 메모리크기       | 기본값   | 값의범위                                               |
| ------ | ------------ | ---------------- | -------- | ------------------------------------------------------ |
| 논리형 | boolean      | 1 byte           | flase    | True, false                                            |
| 정수형 | byte         | 1 byte           | 0        | -128 ~ 127                                             |
|        | short        | 2 byte           | 0        | -32,768 ~ 32,767                                       |
|        | Int(기본)    | 4 byte           | 0        | -2,147,483,648 ~ 2,147,483,647                         |
|        | long         | 8 byte           | 0        | -9,223,372,036,854,775,808 ~ 9,223,372,036,854,775,807 |
| 실수형 | float        | 4 byte           | 0.0F     | (3.4 X 10-38) ~ (3.4 X 1038) 의 근사값                 |
|        | double(기본) | 8 byte           | 0.0      | (1.7 X 10-308) ~ (1.7 X 10308) 의 근사값               |
| 문자형 | 문자형       | 2 byte(유니코드) | '\u0000' | 0 ~ 65,535                                             |



## 프리미티브 타입과 레퍼런스 타입

* primitiveType
  1. primitiveType : 실제 값을 저장한다.`stack에 저장`
  2. stack에 저장되니 해당 스레드에서 사용된다.
  3. 문법상으로 에러를 확인할 수 있다.(컴파일 에러)
* referenceType
  1. referenceType : 값이 저장되는 주소갑을 저장한다.`heap에 저장`
  2. heap에 저장되니 모든 스레드에서 사용가능
  3. 런타임시점에 할당된다.
  4. 문법상으로 에러를 확인 못한다.(런타임 에러)
  5. null을 선언하면 GC가 청소한다.



## 리터럴

* literal

  1. 변수에 할당되는 변하지 않는 값을 리터럴이라한다.

  `3이 리터럴이다.`

  ```java
  int a = 3; 
  ```

  2. 즉, primitiveType에 할당되는 값들은 리터럴이다.
  3. String의 값은 문자열리터럴이다. 

  ```java
  String a = "hello";
  ```

  * 다만 생성자를 사용하면 Heap영역에 생성된다.

  ```java
  String a = "a";
  String b = "a";
  String c = new String("a");
  System.out.println(a==b);
  //true
  System.out.println(a==c);
  //false
  c.intern();
  System.out.println(a==c);
  //true
  ```

  * 리터럴 방식으로 문자열을 생성하면 메소드영역의 상수풀에 할당된다.

    생성자를 사용하면 그냥 Heap영역에 할당된다.

    그때 `intern()`을 사용하게되면 상수풀에 값을 사용하게된다.

  4. long 형태는 L, float 형태는 f를 뒤에 붙여준다.

  ```java
  long a = 100L;
  float b = 0.1f;
  ```

## 변수 선언 및 초기화하는 방법

### 변수 선언

 - 변수 선언 방법 : 타입을 적고 변수명을 적어준다.

   ```java
   int a;
   ```

- 참고사항 static 변수는 클래스가 메모리에 로딩될때 메모리에 올라간다.

  

### 초기화하는 방법

* 변수의 초기화(값을 할당해준다.)

  ```java
  int a = 0;
  ```

* 클래스의 초기화

  ```java
  A a = new A();
  ```

* 초기화블록

  1. 클래스 초기화블럭
     - static 으로 할당한다. static이란걸보면 클래스가 메모리에 로딩될때 한번만 수행되는걸 유추할 수 있다.
  2. 인스턴스 초기화블럭
     * 인스턴스가 생성될때 수행된다. stack에 할당될때

  ```java
  class A {
    //클래스가 메모리에 올라갈때 실행
    static {
  		System.out.println("static 초기화");
  	}
    //인스턴스가 생성될때 실행
    {
  		System.out.println("인스턴스 초기화");
  	}
  }
  ```

### 변수의 스코프와 라이프타임

* 변수의 스코프
  1. 변수에 접근할 수 있는 영역을 나타낸다.
  2. 일반적으로 선언된 블록내에서만 사용가능하다.
* 변수의 라이프타임
  1. 변수가 메모리에 있는 시간
* 일반적인 변수의 3가지 스코프 영역(큰 범위순으로)
  1. Class variables
     - 클래스 내부 static으로 선언된 변수(클래스 내부의 메소드,블럭은 제외한다.)
     - static이기에 클래스가 로딩될때 몰라간다. 
     - 전체에서 접근가능
  2. Class variables
     - 클래스 내부에서 선언된 변수(클래스 내부의 메소드,블럭은 제외한다.)
     - 클래스 내에서만 사용가능하다.
  3. Local variables
     * 메소드,블럭에서 선언된 변수
     * 선언된 곳에서만 사용가능하다.

### 타입변환, 캐스팅 그리고 타입프로모션

* 타입 변환, 캐스팅 : 크기가 더 큰 자료형을으로 변경하는 것을 말한다.(자동)

  ```java
  int a = 10;
  long b = a;
  ```

* 타입 프로모션 : 크기가 더 큰 자료형으로 변경하는 것을 말한다.(선언해야함)

* 주의점 : 감당할 수 있는 크기가 아닐경우 값이 이상하게 들어간다.

  ``` java
  int c  = 40000;
  short d = (short) c;
  ```

  `결과`

  ```bash
  -25536
  ```

### 1차 및 2차 배열 선언하기

- 배열을 생성하면 배열은 첫번째 주소값을 가르킨다.

  `1차 배열 선언 및 주소값 확인`

  ```java
  int[] a = new int[3];
  a[0] = 1;
  a[1] = 2;
  a[2] = 3;
  System.out.println(a);
  ```

  `주소값 System.out.println(a);의 결과`

  ```bash
  [I@1c16babf
  ```

- 2차 배열은 각각 배열의 첫번째 주소값을 가지고있다.

  `2차 배열 선언 방법`

  ```java
  int[][] a = new int[2][2];
  a[0][0] = 1;
  a[0][1] = 2;
  a[1][0] = 3;
  a[1][1] = 4;
  System.out.println(a[0]);
  System.out.println(a[1]);
  ```

  ```bash
  [I@40e7a87
  [I@5404d4fc
  ```

### 타입추론,var

* java10부터 지원한다.
* 타입추론 : 타입을 명시하지 안혹 컴파일러가 추측해서 컴파일한다.
* 초기화값에 null을 지정할 수 없다.
* 지역변수에만 사용가능하다.

```java
{//지역변수
  var a = 1;
  var b = "String";
}
```
