---
layout: post
title:  "[JAVA] Study-2 primitiveType,referenceType,literal,scope"
subtitle:   "primitiveType,referenceType,literal,scope"
categories: dev
tags: java primitiveType referenceType literal scope
comments: true
---
# 자바 스터디 3주차

---

## 산술연산자

* 사칙연산에 사용된다.
* int형으로 연산하면 소수점은 제거된다.
* 소수점 연산은 double로 해야한다.

| 연산자 | 설명   |
| ------ | ------ |
| +      | 더하기 |
| -      | 빼기   |
| *      | 곱하기 |
| /      | 나누기 |
| %      | 나머지 |

```java
		System.out.println(3 + 2);
		System.out.println(3 - 2);
		System.out.println(3 * 2);
		System.out.println(3 / 2);
		System.out.println(3 % 2);
```

`결과 출력`

```bash
5
1
6
1
1
```



## 비트연산자

### 비트이동연산자

* 데이터를 비트 단위로 연산한다.

* 비트는 0과 1로 표현된다.

* `<<,>>,>>>`등이있다.

* 화살표가 가르키는 방향으로 옮겨간다고 생각하면된다.

  ​	ex) << 오른쪽에서 왼쪽으로 이동, >> 왼쪽에서 오른쪽으로 이동

  ​	즉, <<는 숫자가 커지고 >>는 작아진다.

* `>>>`는 빈값이 채워지는 값을 반대로 변경한다.

  ```java
  		//00000000 00000000 00000000 00000010
  		int a = 2;
  		//오른쪽에서 왼쪽으로 세칸 이동
  		//00000000 00000000 00000000 00010000
  		a = a << 3;
  		System.out.println(a);
  ```

  `결과 출력`

  ```bash
  16
  ```

### 비트논리연산자

* boolean 타입일때는 논리연산자로 사용되지만 대상이 정수형이면 비트 논리 연산자로 활용된다.

  | 연산자 | 논리 | 설명                              |
  | ------ | ---- | --------------------------------- |
  | &      | AND  | 두 비트가 1일때만 1               |
  | \|     | OR   | 두 비트가 다르거나 모두 1일경우 1 |
  | ^      | XOR  | 두 비트가 다르면 모두 1           |
  | ~      | NOT  | 비트의 반대                       |

  ```java
  		int a = 2;
  		a = a & a;
  		System.out.println(a);
  		a = 2;
  		a = a | a;
  		System.out.println(a);
  		a = 2;
  		a = a ^ a;
  		System.out.println(a);
  		a = 2;
  		a = ~a;
  		System.out.println(a);
  ```

  `결과 출력`

  ```java
  2
  2
  0
  -3
  ```

## 관계연산자

* 양쪽의 값을 비교할떄 사용한다.

* true,false로 반환된다.

* 주로 조건문에서 사용된다.

  | 연산자 | 기능                                                         | 연산예제       |
  | ------ | ------------------------------------------------------------ | -------------- |
  | >      | 왼쪽이 크면 true 반대면 false를 반환한다.                    | 2 > 3 : false  |
  | <      | 왼쪽이 작으면 true 반대면 false를 반환한다.                  | 2 < 3 : true   |
  | >=     | 왼쪽이 크거나 오른쪽과 같으면 true 오른쪽이 크면 false를 반환한다. | 2 >= 3 : false |
  | <=     | 왼쪽이 작거나 오른쪽과 같으면 true 오른쪽이 작으면 false를 반환한다. | 2 <= 3 : true  |
  | ==     | 같다면 true 다르면 false를 반환한다.                         | 2 == 3 : false |
  | !=     | 다르면 true 같으면 false를 반환한다.                         | 2 != 3 : true  |

## 논리연산자

* 논리연산자는 &&,||가 있다.

* &&가 ||보다 우선순위가 높다()를 잘써야한다.

* 논리연산자도 반복문에서 자주 사용된다.

  | 연산자 | 기능                     |
  | ------ | ------------------------ |
  | &&     | 두 가지 모두 true면 true |
  | \|\|   | 둘중하나라도 true면 true |

## instanceof

* 객체타입을 확인하는데 사용한다.

* 연산자이다.

* true면 형변환이 가능하다.

  `A가 부모 B가 자식`

  ```java
  		class A {}
  		class B extends A{};
  		A a = new A();
  		B b = new B();
  		System.out.println(a instanceof A);
  		System.out.println(b instanceof A);
  		System.out.println(a instanceof B);//부모가 자식이 될 수 없다.
  		System.out.println(b instanceof B);
  ```

  `결과 출력`

  ```java
  true
  true
  false
  true 
  ```

## assignment(=) operator

### 대입연산자

* 제일 자주 사용하는 연산자이다.

* 값을 할당할떄 사용한다.

* 위에서 나온 연산자들을 응용해서 사용한다.

  | 대입연산자 | 설명                                                         |
  | ---------- | ------------------------------------------------------------ |
  | =          | 왼쪽의 피연산자에 오른쪽의 피연산자를 대입함.                |
  | +=         | 왼쪽의 피연산자에 오른쪽의 피연산자를 더한 후, 그 결괏값을 왼쪽의 피연산자에 대입함. |
  | -=         | 왼쪽의 피연산자에서 오른쪽의 피연산자를 뺀 후, 그 결괏값을 왼쪽의 피연산자에 대입함. |
  | *=         | 왼쪽의 피연산자에 오른쪽의 피연산자를 곱한 후, 그 결괏값을 왼쪽의 피연산자에 대입함. |
  | /=         | 왼쪽의 피연산자를 오른쪽의 피연산자로 나눈 후, 그 결괏값을 왼쪽의 피연산자에 대입함. |
  | %=         | 왼쪽의 피연산자를 오른쪽의 피연산자로 나눈 후, 그 나머지를 왼쪽의 피연산자에 대입함. |
  | &=         | 왼쪽의 피연산자를 오른쪽의 피연산자와 비트 AND 연산한 후, 그 결괏값을 왼쪽의 피연산자에 대입함. |
  | !=         | 왼쪽의 피연산자를 오른쪽의 피연산자와 비트 OR 연산한 후, 그 결괏값을 왼쪽의 피연산자에 대입함. |
  | ^=         | 왼쪽의 피연산자를 오른쪽의 피연산자와 비트 XOR 연산한 후, 그 결괏값을 왼쪽의 피연산자에 대입함. |
  | <<=        | 왼쪽의 피연산자를 오른쪽의 피연산자만큼 왼쪽 시프트한 후, 그 결괏값을 왼쪽의 피연산자에 대입함. |
  | >>=        | 왼쪽의 피연산자를 오른쪽의 피연산자만큼 부호를 유지하며 오른쪽 시프트한 후, 그 결괏값을 왼쪽의 피연산자에 대입함. |
  | >>>=       | 왼쪽의 피연산자를 오른쪽의 피연산자만큼 부호에 상관없이 오른쪽 시프트한 후, 그 결괏값을 왼쪽의 피연산자에 대입함. |

## 화살표(->)연산자

* 람다식 식별자 없이 실행가능한 함수

* 즉, 함수의 이름이 없다.(익명함수)

* 함수형 인터페이스를 이용해서 만들 수 있다.

* -> 문법을 사용하면 컴파일러 익명클래스로 변환한다.

* @FunctionalInterface를 사용하면 함수를 하나만 만들 수 있다.

  ```java
  	@FunctionalInterface
  	interface A {
  		public void get();
  	}
  
  
  	@Test
  	public void testPay() {
  		A a = () ->{
  			System.out.println("123");
  		};
  	}
  ```

## 3항연산자

* 한줄로 표현하는 조건문이라고 생각하면된다.

* (조건문) : ? 참 : 거짓 으로 이해하면된다.

* 3항연산자를 중복해서쓰면 가독성이 떨어진다.

  ```java
  		int a = 2;
  		int b = 3;
  		String c = a < b ? "b가 크다.":"a와 b가 같거나 a가 크다";
  		System.out.println(c);
  		c = (a < b ? "b가 크다.":"a와 b가 같거나 a가 크다").equals("b가 크다.")? "중복으로사용하면": "복잡하다";
  		System.out.println(c);
  ```

  `결과 출력`

  ```bash
  b가 크다.
  중복으로사용하면
  ```

## 연산자 우선 순위

| 우선순위 | 이름                     | 연산자                                      |
| -------- | ------------------------ | ------------------------------------------- |
| 1        | 괄호                     | (),[]                                       |
| 2        | 부정 / 증감연산자        | !,~,++,--                                   |
| 3        | 곱셈 / 나눗셈            | *,/,%                                       |
| 4        | 덧셈 / 뺄셈              | +,-                                         |
| 5        | 비트단위의 쉬프트 연산자 | <<,>>,>>>                                   |
| 6        | 관계연산자               | <, <=, >, >=                                |
| 7        |                          | ==,!=                                       |
| 8        | 비트단위의 논리연산자    | &                                           |
| 9        |                          | ^                                           |
| 10       |                          | \|                                          |
| 11       | 논리곱연산자             | &&                                          |
| 12       | 논리합연산자             | \|\|                                        |
| 13       | 조건연산자               | ?:                                          |
| 14       | 대입 / 할당 연산자       | =, +=, -=, *=, /=, %=, <<=, >>=, &=, ^=, ~= |

## 스위치연산자

* 스위치 문은 break를 안쓰게되면 다음 분기로 넘어간다.

* 자바 12에선 ->로 표현하면 break문이 없어도된다.

* 자바 13에선 yield(예약어)로 사용한다.

  ```java
          //Java 12 이전
          int num = 1;
          int returnNum = 0;
          switch(num){
              case 1:
                  returnNum = 1;
                  System.out.println("1들어옴");
                  break;
              case 2:
                  returnNum = 2;
                  System.out.println("2들어옴");
                  break;
              case 3:
                  returnNum = 3;
                  System.out.println("3들어옴");
                  break;
          }
          System.out.println("returnNum : [ " + returnNum + " ]");
  
          //Java 12
          returnNum = switch(num){
              case 1 -> 1;
              case 2 -> 2;
              default -> throw new IllegalStateException("Unexpected value: " + num);
          };
          System.out.println("returnNum : [ " + returnNum + " ]");
  
  
          //Java13
          returnNum = switch(num){
              case 1 : yield 3;
              default : throw new IllegalStateException("unexpected value : " + num);
          };
  
          System.out.println("returnNum : [ " + returnNum + " ]");
  
      
  ```
