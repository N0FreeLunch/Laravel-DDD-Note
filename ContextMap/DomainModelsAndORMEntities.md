## 컨텍스트 맵이란?
- 비즈니스에서 각각의 도메인을 어떻게 나눌지, 나눈 컨텍스트 간의 상호작용을 어떻게 할지 등을 정의한 것이다.

## 좋은 컨텍스트를 만들기 위해서는?
- 라라벨과 같은 프레임워크를 사용하면 견고하고 생산적인 개발 흐름에 도달할 수 있다.
- 프로그래밍을 하기 전에 모델링에 충분한 시간을 쏟을 가치가 있다.
- 너무 적은 기술이나 개념적 빈약성도 문제가 되지만 너무 많은 기술이나 개념적 추상화도 이상적이지 않다.
- 동료들의 의견에 열린 마음을 갖고 다른 사람들의 관점을 듣는데 시간을 투자하도록 하자. 기존 방식 보다 새롭고 잠재적으로 더 나은 방법으로 갈 수도 있다.

## 리치 도메인
- 비즈니스적인 측면과 애플리케이션적인 측면 모두를 고려하여 작성되어야 한다.
- 컨텍스트가 구성되었다면 컨텍스트가 담당하는 기능을 구현하기 위해 컨텍스트 내의 세부 사항을 정의할 필요가 있다.
- 컨텍스트의 세부사항을 표현하기 위해서는 컨택스트 내에서 동작하는 역할, 기능, 상태를 컨트롤하고 각각의 역할 간의 상호작용을 정해야 하며 이는 보통 객체를 통해서 구현된다.
- 앞선 단계에서 요구되는 조건이 무엇인지 다음 단계에서 요구되는 조건이 무엇인지 정의하고 각각의 단계를 넘어갈 수 있는 로직을 만들어 각 단계의 구현사항을 명확하게 하는 것이 중요하다.
- 또한 각각의 엔티티가 가지는 제약조건을 코드로 제한하여 코드를 통해 도메인을 어떻게 다루고 컨트롤하고 있는지 알 수 있도록 만드는 것이 중요하다.
- 각각의 엔티티가 가지는 제약조건은 객체 및 메소드 명, 변수의 타입, 유효성 검사의 데이터 제약, 코드가 위치하는 계층, 프로그래밍 언어의 문법적인 특성, 프레임워크에 의해 정형화 된 코드 등 다양한 방식으로 이뤄질 수 있다.

## 엔티티
### 엔티티에 더 많은 컨텍스트를 부여하자
- 엔티티에 도메인 로직을 최대한 가깝게 배치한다.
- 엔티티에 더 많은 컨텍스트를 부여하는 것이 좋다.
- 모델을 뚱뚱하게 하고 컨트롤러를 마르게 하기 위해서이다.

### ‘데이터 컨테이너’에서 ‘리치 엔티티 ORM’으로 하자
- ORM에서 엔티티는 데이터베이스의 필드에 대응되어 매핑된다.
- ORM으로 엔티티화 된 데이터는 DTO 구조를 통해서 프레임워크의 계층 간의 전달 및 프레임워크의 API 전달에 사용될 수 있다.
- 이 경우 ORM은 단순히 데이터를 저장하는 컨테이너로서 DTO와 유사한 형태의 모습을 하게 된다.
- 단순히 데이터 매퍼로 동작하는 ORM은 도메인의 엔티티를 나타낼 수 있는 제약조건이나 전제조건을 드러내지 않는 빈혈성 도메인 모델을 만든다.
- 도메인을 설명하는 용어, 플로우, 제약조건 및 전제조건을 드러낼 수 있는 엔티티를 만드는 것이 중요하므로 ORM이 가능한 이들을 나타낼 수 있도록 하면 좋다.

### 뚱뚱한 모델
- ORM은 기본적으로 테이블 릴레이션에 의해 하나의 엔티티가 다른 엔티티와의 관계를 맺는다.
- ORM은 ORM 객체에 매핑된 데이터를 의도치 않은 방식으로 변경하는 것을 막기 위해 getter 및 setter를 사용하여 데이터의 무결성을 지키도록 한다.
- ORM에 매핑 대상이 되는 데이터베이스 테이블의 컬럼이 많아질 가능성이 있다.
- 이런 코드들이 모이고 쌓이면서 모델이 점점 비대해진다는 문제점을 갖는다.

### DTO와 ORM
- DTO는 정해진 가지수의 데이터를 한 객체에서 다른 객체로 전달하기 위한 용도로 사용되는 단순한 데이터 저장소이다.
- DTO는 getter와 setter를 가지는 경우가 있으며 이 경우 데이터의 무결성을 지키기 위한 용도로 사용된다.
- 데이터베이스의 엔티티가 ORM의 엔티티와 직접 대응되는 관계를 맺는 개념으로 만든다면 ORM 또한 하나의 DTO로서의 기능을 한다고 볼 수 있다.
