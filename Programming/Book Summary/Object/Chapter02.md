# 02. 객체지향 프로그래밍

## 영화 예매 시스템

### 요구사항 살펴보기

- 사용자는 쉽고 빠르게 보고 싶은 영화를 예매한다.
- 특정한 조건을 만족하는 예매자는 요금을 할인 받는다
  - 할인 조건
    - 상영 순번에 따른 할인
    - 상영 시작 시간에 따른 할인
  - 할인 정책
    - 일정 금액을 할인
    - 일정 비율을 할인
- 하나의 영화에 최대 하나의 할인 정책만 적용할 수 있다.
- 할인 조건은 다수를 지정할 수 있다.

## 객체지향 프로그래밍을 향해

### 협력, 객체, 클래스

- 객체지향을 생각할 때 어떤 `class`가 필요한지 고민할 것이다.
  - **하지만 이것은 본질과는 거리가 멀다.**
- 진정한 객체 지향은 객체에 초점을 맞춰야 하고, 이것은 두 가지를 통해 이뤄진다.

1. 어떤 클래스가 아니라, 어떤 객체가 필요한지 생각해라
   - 객체가 어떤 상태와 어떤 행동을 가지는 지 생각하면 클래스의 윤곽이 잡힌다.
2. 객체를 독립적인 존재가 아니라, **협력하기 위한 공동체의 일원**으로 바라봐라

### 도메인의 구조를 따르는 프로그램 구조

- `도메인` : 문제를 해결하기 위해 프로그램을 사용하는 분야
- 객체지향 패러다임이 강력한 이유: 요구사항 분석부터 구현까지 객체라는 추상화 기법으로 전부 해결
- 객체지향에서는 도메인 개념들을 구현하기 위해 클래스를 사용한다.
  - 즉, 도메인의 구조와 클래스의 구조가 유사하다.

### 클래스 구현하기

- 인스턴스 변수의 가시성은 `private`, 메서드의 가시성은 `public`이다.

- 클래스를 구현하거나, 남이 구현한 클래스를 사용할 때 가장 중요한 것은 

  클래스의 경계를 구분 짓는 것

  - 즉 어떤 부분을 감추고 어떤 부분을 공개할지 정하는 것.

- 클래스의 내부 외부를 분리하는 이유

  - 경계의 명확성이 객체의 자율성을 보장한다.
  - 프로그래머에게 구현의 자유를 제공한다.

**자율적인 객체**

- 객체는 **상태(state)**와 **행동(behavior)**을 가진 복합적인 존재
- 객체는 스스로 판단하고 행동하는 **자율적인 존재**

를 상기하고 있어야 한다.

- 캡슐화: 데이터(상태)와 기능(행동)을 내부로 함께 묶는 것.
  - 캡슐화에서 더 나아가 외부에서의 접근을 통제하는 **접근 제어**도 제공
  - 이를 위해 다양한 **접근 제어자**를 제공 ex) private, protected, public
- 캡슐화와 접근 제어를 통해서 객체를 두 부분으로 나눈다
  1. 외부에서 접근 가능한 **public interface**
  2. 내부에서만 접근 가능한 **implementation**

**프로그래머의 자유**

- 프로그래머의 역할을 클래스 작성자와 클라이언트 프로그래머로 구분하는 것이 좋다.
  - 클래스 작성자: 새로운 데이터 타입을 프로그램에 추가
  - 클라이언트 프로그래머: 클래스 작성자가 추가한 데이터 타입을 사용
- 구현 은닉: 클래스 작성자가 클라이언트 프로그래머에게 필요한 부분만을 공개하고 나머지를 숨김.
  - 이를 통해 외부로 부터의 영향을 고려하지 않고 내부 구현을 변경할 수 있다.
- 이를 통해 내부와 외부는 서로 알아야 할 지식의 양이 줄어들고 자유롭게 구현할 폭이 넓어진다.
  - 따라서, 클래스를 개발할 때마다 인터페이스와 구현을 깔끔하게 나누기 위해 노력해야한다.
- **잊지말자 설계의 목적은 변경을 관리하기 위해서**

### 협력하는 객체들의 공동체

- 영화를 예매하는 메서드

```java
public class Screening {
	public Reservation reserve(Customer customer, int audienceCount) {
		return new Reservation(customer, this, calculateFee(audienceCount), audienceCount);
	}
	
	public Money calculateFee(int audienceCount) {
		return movie.calculateMovieFee(this).times(audienceCount);
	}
}
```

`Screening`의 `reserve` 메서드는 예매하기 위한 정보를 담은 `Reservation`객체를 반환한다.

- 1장의 금액은 `Long` 타입이지만, 현재는 `Money` 타입 객체로 의미를 전달할 수 있다.
  - 이를 통해서 서로 다른 곳에 중복되어 구현되는 것을 막을 수 있고, 의미를 더욱 풍부히 표현한다.
- 영화를 예매하기 위해 `Screening, Money, Reservation`이 서로 메서드를 호출하고 상호작용하는 협력을 수행한다.
- 객체지향 프로그램을 작성할 때는 먼저 협력의 관점에서 어떤 객체가 필요한지 결정하고, 객체들의 공통 상태와 행위를 구현하기 위해 클래스를 작성해라.

## 03. 할인 요금 구하기

### 할인 요금 계산을 위한 협력 시작하기

```java
public class Movie {
	private String title;
	private Duration runningTime;
	private Money fee;
	private DiscountPolicy discountPolicy;

	public Money calculateMovieFee(Screening screening) {
      return fee.minus(discountPolicy.calculateDiscountAmount(screening));
	}
}
```

- `Movie`객체를 보면 영화 요금을 계산하기 위한 `calculateMovieFee`라는 메서드가 존재한다.
  - 할인 정책의 할인 요금을 계산해 반환한다.

Q> 하지만 어떤 정책을 사용할 지 존재하지 않는다.

- 여기에는 **상속, 추상화**가 감춰져 있다.

### 할인 정책과 할인 조건

- 할인 정책은 두 가지지만 대부분의 유사하다.
  - 따라서 두 클래스 사이의 중복을 제거하기 위한 공통 보관 클래스가 필요하다.
  - **추상클래스!**

```java
public abstract class DiscountPolicy {
    private List<DiscountCondition> conditions = new ArrayList<>();

    public Money calculateDiscountAmount(Screening screening) {
        for(DiscountCondition each : conditions) {
            if(each.isSatisfiedBy(screening)) {
                return getDiscountAmount(screening);
            }
        }
        return Money.ZERO;
    }

    abstract protected Money getDiscountAmount(Screening screening);
}
```

- 할인 정책은 다수의 할인 조건을 갖고 있고, 만족하는 경우 **추상 메서드**를 호출한다.
- 전체적인 흐름은 정의 했지만 실제는 추상 클래스를 상속받은 자식 클래스의 메서드가 실행할 것이다.
  - 이러한 디자인 패턴은 **TEMPLATE METHOD 패턴**이라 한다.
- 할인 조건은 인터페이스를 통해 조건 여부를 반환한다.

### 할인 정책 구성하기

- 하나의 영화에 대해 하나의 할인 정책이지만, 다수의 할인 조건이 가능하다.

```java
public class Movie {
	public Movie(... , DiscountPolicy discountPolicy) {
				...
        this.discountPolicy = discountPolicy;
  }
}
public abstract class DiscountPolicy {
	public DefaultDiscountPolicy(DiscountCondition ... conditions) {
		this.conditions = Arrays.asList(conditions);
  }
}
```

- 파라미터 목록을 이용해 정보를 강제한다.
  - 이를 통해 올바른 상태를 가진 객체의 생성을 보장한다.

## 04. 상속과 다형성

### 컴파일 시간 의존성과 실행 시간 의존성

`Movie`의 영화 요금을 계산하기 위해서 필요한 것은 `DiscountPolicy`가 아니라 `AmountDiscountPolicy` 혹은 `PercentDiscountPolicy` 둘 중 하나의 인스턴스가 필요하다.

- 따라서, `Movie`는 두 인스턴스에 의존해야 한다.
- 하지만 코드 수준에서는 어떤 것에도 의존하지 않고, 오로지 추상 클래스인 `DiscountPolicy`에 의존.

- 어떻게 `Movie`가 코드 작성 시점에는 몰랐던 인스턴스와 실행 시점에 협력이 가능한가?
- 코드의 의존성과 실행 시점의 의존성은 다를 수 있다.
  - 다만, 다르면 다를 수록 코드를 이해하기 어려워진다.
  - 이러한 **의존성의 양면성**이 설계가 **트레이드오프의 산물**이라는 것을 잘 보여준다.

### 차이에 의한 프로그래밍

- 상속을 이용하면 기존 클래스의 속성과 행동을 새로운 클래스에 포함시킬 수 있다.
- 이처럼 부모 클래스와 다른 부분만을 추가해 클래스를 빠르고 쉽게 만드는 방법을 **차이에 의한 프로그래밍**이라 한다.

### 상속과 인터페이스

- 상속이 가치 있는 이유: 부모 클래스가 제공하는 인터페이스를 자식 클래스가 물려받을 수 있기 때문.
  - 이것이 상속을 바라보는 일반적인 인식과의 차이
  - 대부분은 메서드나 인스턴스 변수를 재사용 하는 것을 목적으로 봄
- 인터페이스: 객체가 이해할 수 있는 메시지의 목록
  - 상속을 통해 자식 클래스는 자신의 인터페이스 + 부모 클래스의 인터페이스
  - 자식 클래스는 부모 클래스가 수신할 수 있는 모든 메시지를 수신할 수 있다.
    - 외부 객체는 이를 통해 자식과 부모 클래스를 동일한 타입으로 간주한다.
- **upcasting:** 자식 클래스가 부모 클래스를 대신하는 것

### 다형성

- 메시지 ≠ 메서드
- `Movie`는 동일한 메시지를 전송하지만 실제로 어떤 메서드가 실행될 것인지는 메시지를 수신한 클래스가 무엇인가에 따라 달라진다.
  - 이를 **다형성**이라 한다.
  - 동일 한 메시지를 수신 했을 때, 객체의 타입에 따라 다르게 응답할 수 있는 능력
  - 다형적인 객체들은 같은 메시지를 이해할 수 있어야 한다. ⇒ 인터페이스가 동일하다.
- 다형성은 컴파일 시간 의존성과 실행 시간 의존성이 다를 수 있다는 사실에 기반으로 한다.
- 다형성을 구현하는 방법은 다양하지만 메시지에 응답할 메서드를 컴파일 시점이 아닌 실행 시점에 결정한다는 공통점이 있다.
  - 즉, 실행 시점에 **메시지와 실행 메서드를 바인딩 한다.**
  - 이를 **동적 바인딩** 혹은 **지연 바인딩**이라 한다.
  - 반대로 컴파일 시점에 정하는 것을 **초기 바인딩** 혹은 **정적 바인딩**이라 한다.
- 다형성이란 추상적인 개념이며, 이를 구현할 수 있는 방법은 상속만이 아니다.

### 인터페이스와 다형성

- 추상 클래스를 구현함으로써 자식 클래스들이 인터페이스와 내부 구현을 함께 상속받았다.
  - 하지만 구현은 공유할 필요가 없고, 인터페이스만 공유할 때가 있다.
  - 이때는 `interface`라는 프로그래밍 요소를 활용한다.

## 05. 추상화와 유연성

### 추상화의 힘

- `DiscountPolicy` 는 `AmountDiscountPolicy`, `PercentDiscountPolicy`보다 추상적이다.
- `DiscoutCondition`은 `SequenceCondtion`, `PeriodeCondition` 보다 추상적이다.
  - 그 이유는 인터페이스에 초점을 맞추기 때문이다.

![https://user-images.githubusercontent.com/13096845/85088018-00c49b00-b21a-11ea-82e7-6af10087f648.png](https://user-images.githubusercontent.com/13096845/85088018-00c49b00-b21a-11ea-82e7-6af10087f648.png)



- 그림을 통해 추상화의 장점을 알 수 있다
  1. 추상화의 계층만 본다면, 요구사항의 정책을 높은 수준에서 서술한다.
  2. 설계가 유연하다

그림은 한 문장으로 서술하면, "영화 요금은 최대 하나의 정책을 갖고, 다수의 조건을 이용한다"

세부적인 내용이 아니라 상위 정책을 간단하게 표현 할 수 있다.

이것은 기본적인 어플리케이션의 협력 흐름을 나타내고, 새로운 자식 클래스들은 추상화를 이용해 상위 흐름을 그대로 따른다.

⇒ 이것은 디자인패턴, 프레임워크에서 매우 중요하게 작용된다.

### 유연한 설계

- 할인 정책이 없는 경우를 생각해보자.
  - 즉, 할인 요금을 계산할 필요 없이 영화에 설정된 기본 가격을 사용하면 된다.

```java
public class Movie {
	public Money calculateMovieFee(Screening screening) {
		if (discountPolicy == null) {
			return fee;
		}
	}
		return fee.minus(discoutPolicy.calculateDiscountAmount(screening));
	}
}
```

- 하지만, 이 경우는 기존의 협력 방식이 무너지게 된다.
  - 이 경우 할인을 결정하는 책임이 `DiscountPolicy`가 아니라 `Movie`에 있다.
  - 협력의 설계 측면에서 대부분의 경우 좋지 않다.
- 이를 위해  `NoneDiscountPolicy`를 구현하면 된다.

```java
public class NoneDiscountPolicy extends DiscountPolicy {
	@Override
	protected Money getDiscountAmount(Screening screening) {
		return Money.ZERO;
	}
}
```

- 새로운 클래스를 추가함으로써 애플리케이션의 기능을 확대 했다.
- 추상화를 이용했기 때문에 유연하고 확장 가능한 설계가 완성됐다.
- 유연성이 필요하다면 추상화를 이용하자.

### 추상 클래스와 인터페이스 트레이드 오프

- 앞의 `NoneDiscountPolicy를 보게 되면, 어떤 값을 반환 해도 상관이 없다.
  - 부모 클래스인 `DiscountPolicy`와 `NoneDiscountPolicy`를 개념적으로 결합시킨다.
- 해결하기 위해 `DiscountPolicy`를 인터페이스로 바꾸고, `NoneDiscountPolicy`가 `DiscountPolicy`의 `getDiscountAmount()`가 아니라 `calculateDiscountAmount()` 오퍼레이션을 오버라이딩 하게 변경한다.
- 이러한 설계가 추상 클래스와의 설계 중 어느 것이 더 좋은 것은 아니다.
- 모든 것은 트레이드 오프의 산물이다.

### 코드 재사용

- 상속: 코드를 재사용하기 위해 널리 사용 되는 방법
  - 널리 알려졌다고 가장 좋은 방법은 아니다.
- 상속 보다는 **합성**

### 상속

- 객체지향에서 코드를 재사용하기 위해 널리 사용되는 기법
- 하지만 두 관점에서 안 좋은 영향을 미친다
  1. 캡슐화를 위반.
  2. 설계를 유연하지 못하게 한다.
- 캡슐화를 위반한다.

상속을 위해서는 부모 클래스의 구조를 잘 알고 있어야 한다.

결과적으로, 부모 클래스의 구현이 자식 클래스에 노출되기 때문에 캡슐화가 약화된다.

즉 결합도가 높아져 변경하기가 어려워진다.

- 설계가 유연하지 못하다.

상속은 부모 클래스와 자식 클래스 사이의 관계를 컴파일 시점에 결정한다.

즉, 실행 시점에 객체의 종류를 변경하지 못한다.

### 합성

- 인터페이스에 정의된 메시지를 통해서만 코드를 재사용 하는 방법.
  - `Movie`는 `DiscoutPolicy`가 외부에 `calculateDiscountAmount` 메서드를 제공한다는 사실만 안다.
- 합성은 상속의 두가지 문제를 모두 해결한다.
  - 인터페이스에 정의된 메시지만을 사용해 구현을 캡슐화
  - 의존하는 인스턴스를 교체하는 것이 쉬워 설계가 유연하다.
- 그렇다고 상속을 쓰지 말고 합성만 쓰라는 것이 아니다
  - 적절하게 함께 사용한다.

### 결론

- 객체지향은 객체를 지향하는 것.
- 즉 그 중심은 객체다.
  - 하지만 단일 객체가 아니라, 애플리케이션에서 그 기능을 위해 협력하는 객체들 사이의 상호작용.