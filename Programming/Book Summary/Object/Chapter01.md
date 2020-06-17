# 01. 객체, 설계

"이론이 먼저일까, 실무가 먼저일까?"

: SW에서는 이론보다 **실무**가 먼저다. 또한 **실무가** 중요하다.

소프트웨어의 설계를 설명할 때에는 이론이 아니라 **코드로** 보여라

# 티켓 판매 어플리케이션.

### 시나리오

극장은 먼저 관람객의 가방에 초대장이 있는지 확인한다.

만약 초대장이 들어 있다면, 판매원에게서 받은 티켓을 관람객의 가방에 넣는다.

아니라면 티켓을 판매한다.

관람객의 가방에서 티켓의 금액을 차감하고, 매표소에 금액을 증가시킨다.

그리고 티켓을 관람객의 가방에 넣어준다.

## 문제점

소프트웨어 모듈의 세 가지 목적

- 실행 중에 제대로 동작하는 것
- 변경을 위해 존재하는 것.
- 코드를 읽는 사람과 의사소통 하는 것

티켓 판매 어플은 제대로 동작은 하나, 변경이 어렵고 이해하기 어렵다

### 예상을 빗나가는 코드

시나리오를 보면 관람객과 판매원이 극장의 제어를 받는 **수동적인 존재**

- 이해가 가능한 코드란 동작이 우리의 예상에서 크게 벗어나지 않는 코드.

하지만, 수동적인 존재가 우리의 예상을 벗어나고, `enter` Method가 너무 많은 동작을 한다.

```java
public class Theater {
	private TicketSeller ticketSeller;

	public void endter(Audience audience) {
		if (audience.getBag().hasInvitation()) {
			Ticket ticket = ticketSeller.getTicketOffice().getTicket();
			audience.getBag().setTicket(ticket);
		} else {
			Ticket ticket = ticketSeller.getTicketOffice().getTicket();
			audience.getBag().minusAmount(ticket.getFee());
			ticketSeller.getTicketOffice().plusAmount(ticket.getFee());
			audience.getBag().setTicket(ticket);
		}
	}
}
```

하지만 가장 큰 문제는 Audience와 TicketSeller를 변경하면 Theater도 변경해야 된다.

### 변경에 취약한 코드

현재의 가정

- 관람객이 항상 가방을 갖고 다닌다
- 매표소에서만 티켓을 판매한다.
- 현금으로 결제한다

만약 가방을 갖고 있지 않다면?

- 관람객의 가방 객체를 수정해야 하고, 극장에서 접근하는 방식도 바꿔야 한다.

즉 너무 **의존적**이다.

객체 지향의 설계는 서로 의존적이며 협력하는 객체의 공동체를 구축하는 것

즉, 최소한의 의존성만 유지하는 것.

객체 사이의 의존성이 강하다 ⇒ **결합도**가 높다

따라서 설계의 목적은 **객체 사이의 결합도를 낮추는 것**

## 설계 계선하기

현재 시나리오에서는 Theater가 Audience와 TicketSeller에게 관여한다는 것. 즉, 결합도가 높다.

사실 Theater는 Audiencedhk TicketSeller를 자세하게 몰라도 된다.

다시 말해, Audience와 TicketSeller를 자율적인 존재로 만들면 된다.

### 자율성을 높이자

- Theater의 enter 메소드에서 ticketOffice에 접근하는 모든 코드를 TicketSeller 내부로 숨기는 것

  객체 내부의 세부적인 사항을 감추는 캡슐화.

```java
public class Theater {
	private TicketSeller ticketSeller;

	public void enter(Audience audience) {
		ticketSeller.sellTo(audience);
	}
}

public class TicketSeller {
	private TicketOffice ticketOffice;

	public void sellTo(Audience audience) {
		if (audience.getBag().hasInvitation()) {
			Ticket ticket = ticketOffice.getTicket();
			audience.getBag().setTicket(ticket);
		} else {
			Ticket ticket = ticketOffice.getTicket();
			audience.getBag().minusAmount(ticket.getFee());
			ticketOffice.plusAmount(ticket.getFee());
			audience.getBag().setTicket(ticket);
		}
	}
}
```

- TicketSeller에서 Audience를 직접 제어하므로, Audiene도 캡슐화.

```java
public class Audince {
	private Bag bag;
	
	public Long buy(Ticket ticket) {
		if (bag.hasInvitation()) {
			bag.setTicket(ticket);
			return 0L;
		} else {
			bag.setTicket(ticket);
			bag.minusAmount(ticket.getFee);
			return ticket.getFee();
		}
	}
}

public class TicketSeller {
	private TicketOffice ticketOffice;

	public void sellTo(Audience audience) {
		ticketOffice.plusAmount(audience.buy(ticketOffice.getTicket()));
	}
}
```

### 무엇이 개선됐는가

수정된 Audience와 TicketSeller는 자신이 직접 관리해, 객체의 행동이 우리의 예상과 맞다.

더 나아가서 Audience와 TicketSeller 내부가 변한다 하더라도 내부만 변경하면 되어 변경이 용이해진다.

### 어떻게 한것인가

각각의 객체가 자신의 문제를 스스로 해결 하도록 코드를 수정한 것 뿐이다.

### 캡슐화와 응집도

객체 내부의 상태를 캡슐화하고 객체 간에 오직 메세지를 통해서만 상호작용하도록 만드는 것.

**응집도:** 밀접하게 연관된 작업만을 수행하고 연관성 없는 작업은 다른 객체에게 위임하는 객체를 가리켜 응집도가 높다고 한다.

객체의 응집도를 높이려면 객체 스스로 자신의 데이터를 책임진다.

즉 자율적이여야 한다.

### 책임의 이동

초기의 어플리케이션은 책임이 Theater에 집중되고

개선한 어플리케이션은 책임이 적절히 분배된다.

즉, Theater에 몰린 책임이 각각의 객체에 **책임이 이동**했다.

객체지향에서는 각 객체에 책임이 적절하게 분배돼 스스로 책임진다.

### 더 개선할 수 있다.

- 설계의 방법은 한 가지만 있는 것이 아니다.
- 결국 트레이드 오프의 산물이다.

적절한 균형을 찾아 만들어야 한다.

### 그래, 거짓말이다!

예제에서의 극장과 가방 등은 실세계에서 수동적이나 코드에서는 자율적이다.

실세계 객체를 소프트웨어 객체로 **의인화**했기 때문.

훌륭한 객체지향 설계란, 소프트웨어를 구성하는 모든 객체들이 자율적으로 행동하는 설계를 가리킨다.

## 객체지향 설계

### 설계가 왜 필요한가

- 설계란 코드를 배치하는 것이다.

좋은 설계란 기능을 구현하는 동시에 쉽게 변경 가능한 코드를 짜는 것.