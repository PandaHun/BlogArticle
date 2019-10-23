# SW 개발의 원칙

[웹 개발자가 되기 위한 로드맵] 을 참고로 공부하며 작성하고 있습니다.

https://github.com/devJang/developer-roadmap

[
devJang/developer-roadmap2019년 웹 개발자가 되기 위한 로드맵 :kr:. Contribute to devJang/developer-roadmap development by creating an account on GitHub.github.com](https://github.com/devJang/developer-roadmap)

------



개발의 원칙이라고 불리는 SOLID, KISS, YANGI, DRY

#### 1. SOLID 원칙 (객체지향 5대원칙)

 **-SRP(Single Responsibility Principle, 단일 책임 원칙)**: 
  소프트웨어의 설계 부품(클래스, 함수 등)은 하나의 **"책임"**만을 가져야 한다. (**기능**으로 해석하면 편할 것 같다)

  즉, **하나의 클래스 혹은 메소드는 하나의 기능을 가져야 한다.**  왜냐하면 코드를 변경할 때, 여러 기능을 가진 클래스가 있다면, 어느 부분을 고칠지 힘들어진다.
  (유지보수가 어렵다)

 **-OCP(Open-Closed Principle, 개방-폐쇄 원칙)**: 
  확장성은 열려있지만, 수정은 가능해야한다

  즉, 기존의 코드는 변경하지 않고**(Closed)** 기능을 수정하거나 추가할 수 있어야**(Open)** 한다.

  제일 맞는 구현방법은 **인터페이스**를 활용해 구현하는 것.
  이러한 구현을 Strategy Pattern(전략 패턴)이라고도 한단다.

 **-LSP****(Liskov Substitution Principle, 리스코프 치환 원칙)**: 
  상위클래스와 하위 클래스에 대해, 상위 클래스 대신 하위 클래스가 작동되어도 변함없어야 한다.

  즉, **자식 클래스는 부모 클래스에서 가능한 기능을 해야 한다.**  부모 클래스와 자식 클래스의 행위에 일관성이 있어야 한다는 MIT 교수가 제안한 설계 원칙.

 **-ISP****(Interface Segregation Principle, 인터페이스 분리 원칙)**: 
  한 클래스는 자신이 사용하지 않는 인터페이스는 구현하지 말아야 한다.

  즉, **하나의 일반적인 인터페이스 보다는 여러개의 구체적인 인터페이스가 낫다.**  
  다시말해, 자신이 사용하지 않는 기능에는 영향을 받으면 안된다.

 **-DIP****(Dependency Inversion Principle, 의존 역전 원칙)**: 
  의존관계를 맺을 때, 변화하기 쉬운 것 보단, 변화하기 어려운 것에 의존해야 한다. 
                                  \* JAVA 객체 지향 디자인 패턴(정인상/채홍석 지음, 한빛미디어, 2014) p.121 인용

  즉, **의존관계를 맺을 때, 구체적 클래스가 아닌 인터페이스나 추상 클래스와 관계를 맺는 것**

------

#### 2. KISS - Keep It Simple, Stupid: 단순하게 하라

------

#### 3. YAGNI - You Ain' Gonna Need It: 필요하기 전까지 구현하지 말아라

------

#### 4. DRY - Don't Repeat Yourself: 같은 일을 두 번 하지 말아라

------