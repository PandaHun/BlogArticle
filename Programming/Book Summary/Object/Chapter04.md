# 04. 설계 품질과 트레이드 오프

- 객체지향 설계: 올바른 객체에게 올바른 책임을 할당하며, 낮은 결합도와 높은 응집도를 가진 구조를 창조하는 활동
  - 객체지향 설계의 핵심은 **올바른 책임, 책임은 결합도와 응집도에 연관있다.**
  - 설계의 변경에는 비용이 수반되나, 훌륭한 설계는 합리적인 비용 안에서 변경을 할 수 있는 구조
    - 높은 응집도와 낮은 결합도
  - 객체에 행동을 중심으로 설계한다면, 구현이 노출되지 않아 설계의 변경에 유연하다.

## 01. 데이터 중심의 영화 예매 시스템

- 객체지향 설계의 두 가지 방법
  1. 상태를 중심으로 한 분할(데이터 중심)
  2. 책임을 중심으로 한 분할(우리의 목표)
- 데이터에 중심을 둔다면, 구현에 중심을 둔 불안전한 설계
  - 이러한 사항들이 노출되어 캡슐화를 저해하고, 변경에 취약하다.

### 데이터를 준비하자

```java
public class Movie {

    private String title;
    private Duration runningTime;
    private Money fee;
    private List<DiscountCondition> discountConditions;

    private MovieType movieType;
    private Money discountAmount;
    private double discountPercent;
}
```

- 책임 중심 설계와 다른 점은 할인 조건 목록, 정책 등이 `Movie`안에 정의되고 있다.

- 데이터 중심 설계는 객체에게 필요한 데이터에 집중한다.

- 캡슐화를 지키기 위해 

  ```
  접근자(accessor), 수정자(mutator)
  ```

  를 추가한다.

  - 쉽게 `getter, setter`

- 이러한 형태로 설계한 데이터 중심의 시스템

![https://user-images.githubusercontent.com/13096845/87161015-dfc60600-c2fe-11ea-817d-085de0a72d43.png](https://user-images.githubusercontent.com/13096845/87161015-dfc60600-c2fe-11ea-817d-085de0a72d43.png)

## 02. 설계 트레이드오프

- 설계를 판단하는 세가지 기준
  1. 캡슐화
  2. 응집도
  3. 결합도

### 캡슐화

- 객체의 내부 구현을 숨기기 위해, 상태와 행동을 한 객체 안에 모은다.

- 이를 통해 객체의 변화가 시스템에 주는 파급효과를 조절한다.

- 변경의 가능성이 높은 불안한 

  구현

  과 안정적인 부분인 

  인터페이스

  - 알아야 할 필요 없는 부분을 감춤으로써 단순화하는 추상화 전략인 **캡슐화**
  - 불안한 구현을 안전한 인터페이스에 숨기는 것

### 응집도와 결합도

- 모듈의 내부 요소들의 연관 정도를 나타내는 

  응집도

  - 하나의 목표에 얼마나 긴밀하게 협력하는가?

- 다른 모듈에 대해 얼마나 많은 것을 아는지 나타내는 

  결합도

  - 서로가 어느 수준의 지식을 공유하는가?

- 좋은 설계란? 응집도가 높고 결합도가 낮은 설계 (너무 난해하다!)

  - 각 요소가 하나의 목표를 위해 작동하고, 느슨하게 결합돼야 한다(이것도 난해하다)
  - 좋은 설계는 오늘의 기능을 수행하면서, 내일 변경을 수용할 수 있어야 한다.

- 변경의 관점에서의 응집도는 

  변경이 발생 할 때 내부 모듈에서 발생하는 변경의 정도

  - 하나의 변경을 위해 한 모듈만 변경된다면 높은 응집도(얼마나 바꾸기 편한가..)
  - 응집도가 높을 수록 변경 대상과 범위가 명확해 진다.

- 변경의 관점에서의 결합도는 

  한 모듈이 변경되기 위해 필요한 다른 모듈의 변경 정도

  - 결합도가 높으면 높을 수록 한 모듈을 바꾸려 할 때 바꿔야 하는 다른 모듈이 많다(

    끔찍

    )

    - 내부 구현을 변경 했을 때 외부에 영향을 준다면 결합도가 높다.
    - 인터페이스만 변경 했을 때 영향을 준다면 결합도가 낮다.

  - 즉 인터페이스에 의존해서 설계를 한다면. 결합도가 낮다.

- 다만 성숙한 라이브러리, 프레임 워크는 결합도에 고민할 필요는 없다.

- 하지만, 개인의 코드는 불안정

- 캡슐화를 지킨다면 응당 응집도는 높아지고, 결합도는 낮아진다.

## 03. 데이터 중심의 영화 예매 시스템의 문제점

- 기능적 측면은 동일하지만, 설계는 아니다.
- 데이터 중심의 설계는 내부 구현을 노출한다.
  - 즉, 캡슐화를 위반 한다.
  - 따라서 낮은 응집도와 높은 결합도를 가진다.

### 캡슐화 위반

```java
class Movie {
    private Money fee;

		public Money getFee() {
				return fee;
		}
		
		public void setFee(Money fee) {
				this.fee = fee;
		}
}
```

- 캡슐화를 지키는 것 처럼 보인다. 

  ```
  private
  ```

   접근 지정자 때문에

  - 하지만, 내부 구현이 노출되어 있다.
  - 노골적으로 `Money` 타입의 `fee`라는 객체가 있음을 나타냈다.

- 데이터 중심의 설계를 하다보면, 접근자와 수정자를 찾게 된다.

  - 과도하게 의존하는 설계 방식을 **추측에 의한 설계 전략(design-by-guessing strategy)**
  - This sort of design-by-guessing strategy is inefficient at best because you end up wasting time writing methods that aren’t used *by.Allen Holub*
  - 추측에 의한 설계는 잘해도 비효율적이다. 사용하지 않는 방법을 작성하느라 시간을 낭비하기 때문에 *by.Allen Holub*

### 높은 결합도

- 객체 내부 구현이 인터페이스에 노출 되면서 구현에 강하게 결합됐다.

- 단순히 내부 구현을 변경해도, 인터페이스에 의존하는 객체 혹은 클라이언트도 변경해야 한다.

- 만약 

  ```
  Money
  ```

   타입에서 다른 타입으로 객체를 바꾼다면?

  - 반환도 바꾸고, 받는 쪽도 바꿔야 한다.
  - 사실상 캡슐화가 이뤄지지 않았다.
  - 접근자를 둠으로써 가시성을 `private`에서 `public`으로 바꿔 캡슐화를 약화시켰다.

- 여러 데이터 객체들을 사용하는 제어 로직이 특정 객체에 집중된다.

  - 시스템에서의 `ReservationAgency`
  - 어떤 객체가 바뀌더라도 `ReservationAgency`는 바뀐다.

- **전체 시스템을 하나의 의존 덩어리로 만들어, 변경이 발생할 때 전체 시스템이 요동친다.**

### 낮은 응집도

- 서로 다른 이유로 변경되는 코드가 한 모듈 안에 있는 경우
- 변경과 응집도 사이의 관계
  - 할인 정책이 추가될 경우
  - 할인 정책 별로 요금 계산 방법이 변경될 경우
  - 할인 조건이 추가될 경우
  - 할인 조건 별로 할인 여부를 판단하는 방법이 변경될 경우
  - 예매 요금 계산 방법이 변경될 경우
  - 위의 모든 경우에 `ReservationAgency`는 변경된다.
- 변경의 이유가 다른 코드가 한 모듈 안에 있어 변경과 상관 없는 코드들이 영향을 받는다.
- 하나의 요구 사항을 변경하기 위해 다른 모듈을 수정해야 한다.
  - 책임의 일부가 엉뚱한데 위치하기 때문에

## 04. 자율적인 객체를 향해

### 캡슐화를 지켜라

- **객체 지향 설계의 제 1원리: 캡슐화**
- 객체 외부에서는 인터페이스에 정의된 메서드를 통해서만 상태에 접근해야 한다.
  - 이때의 메서드는 접근자와 수정자를 의미하는 게 아니다.
  - 객체가 책임져야 하는 무언가를 수행하는 메서드
  - 가시성을 `private`으로 설정해서 캡슐화 한 것이 아님.

### 스스로 자신의 데이터를 책임지는 객체

- 객체가 어떤 데이터를 포함해야 하는가?
- 객체가 데이터에 대해 수행해야 할 연산은 무엇인가?
- 위 두 가지를 염두하면, 객체의 내부 상태를 저장하는 방식과 연산 집합을 얻을 수 있다.
- 이러한 방식으로 개선한 설계

![https://user-images.githubusercontent.com/13096845/87166039-f0c64580-c305-11ea-9bd3-c7aa88864aae.png](https://user-images.githubusercontent.com/13096845/87166039-f0c64580-c305-11ea-9bd3-c7aa88864aae.png)

## 05. 하지만 여전히 부족하다.

- 개선했지만 문제는 여전하다.

### 캡슐화 위반

- 여전히 상태를 인터페이스에 노출하는 `DiscountCondition`
- 만약 수정해야 한다면?
  - 해당 메서드를 사용하는 클라이언트도 함께 수정해야 하는 **파급효과**가 발생한다.
  - 파급 효과는 캡슐화가 부족했다는 증거

### 높은 결합도

- 캡슐화 위반으로 내부 구현이 외부로 노출됐기 때문에 결합도는 높을 수 밖에 없다.
  - 즉, 한 모듈의 구현이 변경될 때 다른 모듈의 변경의 영향이 전파될 확률이 높다.

### 낮은 응집도

- 캡슐화 위반으로 높은 결합도가 생겼고, 할인 조건 변경이 `Movie, Screening`까지 영향을 미친다.
- **모든 문제는 캡슐화 위반에서 시작된다.**

## 06. 데이터 중심의 설계의 문제점

- 데이터 중심의 설계는 변경에 취약하다.
  1. 본질적으로 너무 빠른 시기에 데이터에 관해 결정하도록 강요한다.
  2. 협력이라는 문맥이 없이 객체를 독립된 섬으로 만들고, 연산을 결정한다.