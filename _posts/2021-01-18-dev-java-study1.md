---
layout: post
title:  "[JAVA] Study-1 JVM,JRE,JDK"
subtitle:   "JVM,JRE,JDK"
categories: dev
tags: java jvm jre jdk
comments: true
---
# 자바 스터디 1주차 JVM, JRE, JDK의 이해

---

## JVM이란 무엇인가?

### JVM(java Visual Machine)

* Java byte 코드를 어찌 실행할지 정하는 표준 스펙
  1. class파일을 OS에 특화된 코드로 변환하여 실행한다.(인터프리터와 JIT컴파일러)
  2. OS에 맞춰 실행해야하기에 플랫폼에 종속적이다.
* JVM만 홀로 제공되지 않는다.
  1. 최소한의 배포 단위가 JRE이다. 
* JVM의 역할
  1. 클래스 파일 읽어드림
  2. 메모리에 올림
  3. 인터프리터와 JIT컴파일러를 통해 실행

---

## 컴파일 하는 방법

1. 자바 파일 생성

   ```java
   public class HelloJava {
           public static void main(String[] args) {
                   System.out.println("Hello, Java");
           }
   }
   ```

2. `Javac` 명령어로 컴파일(바이트 코드 생성)

   `컴파일하기이전 java파일만 존재`

   ```bash
   mac  ~/Desktop/2021git/study  ls -al
   total 8
   drwxr-xr-x  3 mac  staff   96  1 17 20:47 .
   drwxr-xr-x  7 mac  staff  224  1 17 20:47 ..
   -rw-r--r--  1 mac  staff  109  1 17 20:40 HelloJava.java
   ```

   `javac로 컴파일 실행후 class파일 생성확인`

   ```bash
   mac  ~/Desktop/2021git/study  javac HelloJava.java
    mac  ~/Desktop/2021git/study  ls -al
   total 16
   drwxr-xr-x  4 mac  staff  128  1 17 20:48 .
   drwxr-xr-x  7 mac  staff  224  1 17 20:47 ..
   -rw-r--r--  1 mac  staff  423  1 17 20:48 HelloJava.class
   -rw-r--r--  1 mac  staff  109  1 17 20:40 HelloJava.java 
   ```

---

## 컴파일 후 실행하는 방법

1. `java` 명령어로 실행

   ```bash
    mac  ~/Desktop/2021git/study  java HelloJava
   Hello, Java
   ```

---

## 바이트 코드란?

* 바이트코드(Bytecode, portable code, p-code)는 특정 하드웨어가 아닌 가상 컴퓨터에서 돌아가는 실행 프로그램을 위한 이진 표현법
* .class파일로 생성된 파일을 말함.
* class파일로 각 os별로 jvm만 있으면 실행가능하다.

---

## JIT 컴파일러란 무엇이며 어떻게 동작하는지

### JIT 컴파일러 

* Just-in-time 프로그램을 실제 실행하는 시점에 기계어로 번역하는 컴파일 기법

* 인터프리터가 기계어로 해석을 해서 실행하다. 

  코드중에 반복되는 코드(자주사용)는 jit컴파일러가 기계어로 변환해 캐싱해놓는다.

  인터프리터가 기계어로 변환해 캐싱해놓은걸 재사용한다.

---

## JVM 구성요소

## JVM 구조

### 클래스 로더시스템

* 자바 바이트 코드를 읽어 메모리에 적절하게 배치

  | 로딩(loading)                                    | 링크(linking)            | 초기화(initialization)            |
  | ------------------------------------------------ | ------------------------ | --------------------------------- |
  | * .class파일에서 바이트코드를 읽어 메모리에 저장 | 레퍼런스를 연결하는 과정 | static 값들 초기화 및 변수에 할당 |

### 메모리

	#### 스택,PC,네이티브 스택은 스레드에서 사용

#### 힙, 메소드는 전체에서 사용

* 스택
  1. 스레드 마다 런타임 스택을 만들어 사용한다.
* PC
  1. 스레드 마다 스레드 내 현재 실행할 스택 프레임을 가리키는 포인터
* 네이티브 메소드 스택
  1. 네이티브 메소드 라이브러리를 쓸때 사용한다.
  2. 네이티브 메소드? java가 아닌 c나 c++로 구현되어있는 것.
* 힙
  1. 객체를 저장, 공유
* 메소드
  1. class 수준의 정보를 저장,공유 (클래스 이름, 부모 클래스 이름, 메소드, 변수)

### 실행엔진

* 인터프리터 

  1. 한줄씩 네이티브 바꿔가며 실행한다.

* JIT 컴파일러 

  1. 인터프리터가 반복되는 코드를 발견하면 바이트 코드를 네이티브 코드로 바꿔둔다.

  2. 인터프리터가 실행중 반복되는 코드 발견하면 JIT컴파일러로 네이티브 코드로 변경후 캐싱해놓은것을 사용한다.

* GC

  1. 쓰이지 않는 객체를 정리한다.

---

## JDK와 JRE의 차이

## JRE(Java Runtime Environment)

* 자바 애플리케이션을 실행할 수 있도록 구성된 배포판
* 자바를 개발하는데 필요한 툴은 제공되지 않는다.
* JVM이 포함되어 자바를 실행이 가능하다.

## JDK(Java Development Kit)

* JRE과 개발에 필요한 툴을 포함한다.

---
