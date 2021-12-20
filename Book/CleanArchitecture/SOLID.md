# :book: 설계 원칙

> 클린 아키텍처 Ch03 요약



SOLID 원칙의 목적

- 변경에 유연
- 이해하기 쉬움
- 많은 소프트웨어 시스템에 사용될 수 있는 컴포넌트의 기반



- SRP: Single Responsibility Principle
  - Conway 의 법칙에 따른 따름 정리: 소프트웨어 시스템이 가질 수 있는 최적의 구조는 시스템을 만든 조직의 사회적 구조에 커다란 영향을 받는다. 따라서 각 소프트웨어 모듈은 변경의 이유가 하나, 단 하나여야만 한다.
- OCP: Open Closed Principle
  - 1980년대 Bertrand Meyer에 의해 유명해진 원칙, 기존 코드를 수정하기보다는 반드시 새로운 코드를 추가하는 방식으로 시세틈의 행위를 변경할 수 있도록 설계해야만 소프트웨어 시스템을 쉽게 변경 할 수 있다는 것이 이 원칙의 요지
- LSP: Liskov Substitution Principle
  - 1988년 Barbara Liskov 가 정의한, 하위 타입에 관한 유명한 원칙, 상호 대체 가능한 구성요소를 이용해 소프트웨어 시스템을 만들 수 있으려면, 이들 구성요소는 반드시 서로 치환 가능해야 한다는 계약을 반드시 지켜야 한다.
- ISP: Interface Segregation Principle
  - 이 원칙에 따르면 소프트웨어 설계자는 사용하지 않은 것에 의존하지 않아야 한다
- DIP: Dependency Inversion Principle
  - 고수준 정책을 구현하는 코드는 저수준 세부사항을 구현하는 코드에 절대로 의존해서는 안 된다. 대신 세부사항이 정책에 의존해야 한다.



#### :link: 참고자료​

- [클린 소프트웨어](http://www.butunclebob.com/ArticleS.UncleBob.PrinciplesOfOod)
- [SOLID](https://en.wikipedia.org/wiki/SOLID)

<br>

### :bookmark: SRP: 단일 책임 원칙

> 하나의 모듈은 하나의, 오직 하나의 액터에 대해서만 책임져아 한다.
>
> 액터: 변경을 요청하는 한 명 이상의 사람들



원칙을 이해하는 좋은 방법 중 하나는 원칙을 위반하는 징후를 살펴보는 것

#### 징후 1: 우발적 중복

#### 징후 2: 병합

#### 해결책

