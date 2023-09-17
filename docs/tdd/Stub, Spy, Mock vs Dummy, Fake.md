---
layout: default
title: Stub, Spy, Mock vs Dummy, Fake
nav_order: 1
has_children: false
parent: TDD
# grand_parent: 
permalink: /docs/tdd/stub-spy-mock-dummy-fake
---


# Stub, Spy, Mock vs Dummy, Fake
{: .no_toc }
<br>


## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

<br>

### 참고자료
{: .fs-6 .fw-700 }

- [Test Doubles (테스트 더블) - Dummy, Fakes, Mocks, Spy and Stubs](https://codinghack.tistory.com/92)
- [Test Doubles - Fakes, Mocks and Stubs](https://blog.pragmatists.com/test-doubles-fakes-mocks-and-stubs-1a7491dfa3da)

<br>


### Stub, Spy, Mock vs Dummy, Fake
{: .fs-6 .fw-700 }
테스트를 진행하거나, 여기 저기 공통적으로 산재된 코드를 리팩토링할 때 가짜 객체의 종류들을 미리 공부해둔 다면, 용어적으로도 명확히 범위가 구분되고 종류를 구분할 수 있기 때문에 Dummy, Stub, Fake, Spy, Mock 을 파악해둔다면 좋다.<br>
<br>

### 테스트 더블이란?
{: .fs-6 .fw-700 }
테스트 코드 작성시 실제 객체가 아닌 단순한 객체를 이용해 어떤 동작을 하게끔 해서 결과값이나 동작을 테스트 하는 경우가 많다.<br>
<br>
이런 경우 Fake 객체를 사용하거나 대역을 수행하는 객체를 도입한다.
- Fake 객체
  - 어떤 하나의 동작을 수행하는 하나의 클래스를 테스트용도로 정의해서 사용하는 것을 의미한다.
- 대역
  - `이런 이런 조건에서는 이렇게 대답한다` 라는 대사를 기억하는 **대역**을 수행하는 도입하는데, 이것을 실제 하나의 타입으로 정의해서 테스트를 만드는 경우도 있고 Mockito 와 같은 프레임워크를 통해 하나의 표현식으로 만들어서 테스트를 만드는 경우도 있다.
  - 대역이 하는 **대사** 를 stub 이라고 이야기한다.

<br>
<br>
**Test Double?**
  - 가짜 객체 또는 대역 객체를 이용해 테스트하는 것을 `Test Double` 이라고 부른다.
  - 제라드 메스자로스(Gerard Meszaros)가 만든 용어다.
<br>

![](./img/STUB-SPY-MOCK-VS-DUMMY-FAKE/UML.png)

<br>



### Stub, Mock, Dummy, Fake
{: .fs-6 .fw-700 }
Stub, Mock, Dummy, Fake 모두 테스트를 위한 객체를 만드는 것을 의미한다.

<br>



**Mock, Stub**<br>

Stub 은 하나의 실제 행위를 테스트 하기 위해 실제 객체를 대체하면서 **어떻게 연기할지** 를 정의하는 것을 의미한다. Mock 은 Stub을 하기 위해 사용하는 가짜 객체를 의미하는데, Mock 객체를 기반으로 Stub을 하는 것은 꽤 흔하게 자주 사용되는 편이다. Stub이 Mock 에 비해 더 포괄적인 개념이다.<br>
Stub 을 하는 타입(클래스)을 정의하기도 하기에 Stub 자체로는 대역을 의미하기도 하고 가짜 객체를 의미하기도 한다.<br>

- Stub 
  - Dummy 데이터 기반으로 동작하게 만든 객체
  - Stub 을 테스트 코드에서 사용하는 방식은 **클래스 기반의 Stub**, **가짜 객체 기반의 Stub 표현식**이 있다.
  - **클래스 기반의 Stub, 가짜 객체 기반의 Stub 표현식**은 글의 아래에 별도로 정리해두었다.
- Mock
  - 가짜 객체를 만드는 것을 의미한다.
  - 가짜 객체를 만들어서 표현식 기반으로 Stub 하는 것 역시 가능하다. 
  - 예를 들면 Mockito 프레임워크를 사용해서 Stub을 하면, Stub을 표현식 기반으로 하는 것이 된다.
  - 테스트 케이스가 비즈니스 적으로 확립된 요건이라면 여기 저기 산발되는 테스트 코드의 재사용성을 위해 `Stub-` 클래스를 정의해두기도 한다.

<br>



**Dummy**<br>

Dummy 는 테스트를 위한 데이터를 의미하기도 하고, 테스트를 위한 가짜 동작을 하는 Dummy 클래스를 만드는 것 역시 Dummy를 의미한다. Stub, Mock 모두 대역이여서 실제 객체를 연기하는 역할을 하는데, Dummy 는 Stub, Mock 이 하는 대역의 역할과, Spy 처럼 실제 객체의 인자 호출 타입/값, 호출횟수 등을 감시하는 것들을 포함하는 광범위한 개념이다.

<br>



**Fake**<br>

Fake 는 완전하게 개별적인 클래스를 의미한다. 대역을 통해 어떤 역할을 가정한다기 보다는, 그 클래스/객체를 대체하는 경우를 의미한다. 예를 들면 RedisRepository 를 상속(확장)하는 FakeRedisRepository 를 테스트 코드에서 사용하는 것을 예로 들 수 있다.<br>

만약, 테스트 코드에서 테스트 시 마다 네트워크를 통해 레디스에 접속하고, 레디스의 키/밸류가 여러 개발자에 의해 공유된다면, 테스트의 멱등성을 확보하기 쉽지 않다. 레디스 커넥션 테스트는 따로 하면 된다. <br>

레디스에서 특정 키에 대해 밸류의 값이 여러가지 종류일 경우 이 때마다 레디스의 키/밸류를 수정하기보다는, 순수 자바 코드 기반에서 특정 키에 대한 Value 의 종류에 따른 순수 자바 로직에 대한 테스트는 FakeRedisRepository 를 구현해서 테스트를 한다면, 테스트의 멱등성을 확보할 수 있게 된다. <br>

주로 FakeRedisRepository 같은 `Fake-` 라는이름의 클래스를 만들고, RedisRepository 라는 인터페이스를 통해 RedisRepository 라는 추상타입으로 분류해둔다.<br>

또는 [testcontainer](https://www.testcontainers.org/) 라는 격리된 docker 환경 내에서 테스트를 구동하는 경우 역시 생각해볼 수 있다.<br>

<br>



### 클래스 기반의 Stub, 가짜 객체 기반의 Stub 표현식
{: .fs-6 .fw-700 }
**클래스 기반의 Stub**

- 여러 곳에서 Stub 으로 작성한 코드들이 중복되면, 이 Stubbing, 즉, 가정을 하나의 코드로 구체화해서 모아두는 것에 대해 고려하게 된다.
- 여러 곳에 제 각각으로 흩어져있는 Mockito 의 stubbing 이 하나의 구체적인 비즈니스 요건으로 공통화가 가능하다면 클래스 기반으로 정의해두면, 재사용이 가능해진다.
- e.g.
  - 실제로 경험해본 예로는 주식 거래 데이터 전송/저장 프로그램 구현 시에 미국주식의 개장/폐장/AFT/PRE 여부파악을 해야했고, 미국주식 말고도 중국,한국,베트남,채권,환율 데이터 를 연동했기에 어느 나라의 어느 시각의 어느 데이터인지 파악이 필요했었다. 써머타임 적용시에 대한 테스트 코드 역시 있어야 했었다.
  - 이때 여기 저기 여러가지 경우의 수에 대한 테스트 코드를 만들어야 했는데, Mockito 기반의 Stubbing 식은 세부 테스트 코드 안에 숨어있어서 뭔가 조건 값 가정을 수정할 때 일일이 찾아야 하는 피로감이 있었고, 이런 이유로 `Stub-` 이 붙은 별도의 테스트 클래스를 테스트 패키지 내에서만 작동하도록 정의해두어 테스트에 사용했었다.

**가짜 객체 기반의 Stub 표현식**

- 비즈니스 요건이 파악되고 공통점이 발견되기 전까지는 Mockito 같은 프레임워크에서 제공하는 가짜 객체 기반의 스터빙을 사용한 테스트를 사용하면 좋다.

<br>



### 일러스트
{: .fs-6 .fw-700 }
아래 그림들은 [Test Doubles - Fakes, Mocks and Stubs](https://blog.pragmatists.com/test-doubles-fakes-mocks-and-stubs-1a7491dfa3da) 에서 얻어왔다. 

<br>



#### stub
{: .fs-5 .fw-700 }
![](./img/STUB-SPY-MOCK-VS-DUMMY-FAKE/STUB.png)

<br>



#### mock
{: .fs-5 .fw-700 }
![](./img/STUB-SPY-MOCK-VS-DUMMY-FAKE/MOCK.png)

<br>



#### Fake
{: .fs-5 .fw-700 }
![](./img/STUB-SPY-MOCK-VS-DUMMY-FAKE/FAKE.png)













