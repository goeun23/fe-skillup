# SOLID 원칙

## Single Responsibility Principle(SRP)

> 단일책임원칙

- 한 객체는 하나의 책임만 가져야 한다.
- 책임 = 변경의 이유
- 객체가 너무 많아지므로 지키지 않는 경우도 많음

## Open Closed Principle(OCP)

> 개방폐쇄원칙

- 확장에 대해서는 열려있고, 변경에 대해서는 닫혀있어야 한다.
- 새로운 기능을 추가할 때 기존 코드가 수정되면 안된다.

## Liskov Substitution Principle(LSP)

> 리스코프 치환원칙

- 자식 클래스는 부모클래스의 역할을 대체할 수 있어야 한다.
- 부모 클래스의 자리에 자식 클래스를 넣고 타입 에러가 나나 확인해 보면 됨

## Interface Segregation Principle(ISP)

> 인터페이스 분리 원칙

- 클래스는 자신이 사용하지 않는 인터페이스는 구현하지 말아야한다.
- 인터페이스의 단일 책임 원칙
- 인터페이스를 쪼개서 여러 개로 만들고, 필요한 만큼 implements

## Dependency Inversion Principle(DIP)

> 의존성 역전 원칙

- 추상성이 높은 클래스와 의존 관계를 맺는다.
- 상속 대신 합성을 하자.
- interfce, abstract class를 매개변수로 받자.
