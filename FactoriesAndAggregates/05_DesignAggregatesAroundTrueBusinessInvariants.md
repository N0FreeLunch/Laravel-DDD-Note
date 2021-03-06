## 불변성을 중심으로 애그리게이트 디자인
- 애그리게이트는 일반적으로 도메인 모델 객체의 불변성을 유지하기 위해 사용한다.
- 불변성을 통해 비즈니스 문제를 해결할 수 있는 경우 유용하다.
- 비즈니스 모델에서 불변량을 다루는 부분을 따로 모델링한다.
- 각 도메인 또는 기능 간의 순서가 중요한 경우, 유효성 있는 도메인 모델로 만들기 위한 어떤 로직이 필요한 경우 데이터베이스와 응용 프로그램 내에서 이런 순서에 따라 유효한 로직에 따라 전개될 수 있도록 불변량의 기준을 세우는 것이 중요하다.

### 예
- 구매 주문 집계에는 회계 부서에서 승인할 수 있도록 구매주문(Purchase Order)에 하나 이상의 주문 라인(line item)가 있어야 한다는 비즈니스 규칙이 있을 수 있다.
- 주문 라인의 금액의 합계가 일정 금액 이하이여야 한다는 허용 가능한 최대 금액이 있을 경우 등의 제한 조건이 있을 수 있다.
- 이런 개념들도 도메인 모델 내에 명시적으로 만들어 주는 것이 좋은데 불변량이란 개념을 사용하여 코드를 표현할 수 있다.

#### 주문 라인이란?
- 상품을 주문한 묶음 및 주문한 상품 단위를 처리하기 위한 정보들의 집합을 하나로 총칭한 것.

## 작은 애그리게이트 설계
- 큰 컨텍스트를 다룰 때는 애그리게이트 내의 객체가 증가하면서 복잡성이 증가하게 된다. 따라서 작은 애그리게이트가 큰 애그리게이트 보다 선호된다.
- 애그리게이트는 트랜젝션 컨텍스트 내에 위치할 수 있기 때문에 트랜젝션 실행과 관련된 쿼리가 많을수록 프로세스의 복잡성이 증가한다.
- 데이터 내의 불일치를 방지하기 위해서 데이터베이스 잠금이 설정되어 있는 경우 동시에 여러 사용자가 데이터베이스 잠금과 관련된 작업을 수행하면 시스템의 불일치 또는 예기치 않은 동작을 일으킬 수 있다.

## 비즈니스의 불병량이란 것이 무엇일까?
- 많은 주문 단위를 보유하는 시스템의 모델인 주문 개념이 있는 전자 상거래 응용 프로그램을 가정해 보자.
- 주문의 체크아웃 프로세스를 거치면 주문 상품들의 묶음과 체크아웃 시 입력한 정보들을 하나로 묶은 주문 라인 인스턴스를 만들어야 한다. 이후 주문 단위의 부가세 및 운영에 필요한 비용을 추가하는 작업을 한다. 주문 라인의 업데이트, 삭제, 생성 등 주문 라인의 변경에 따라 부가세는 계속적으로 변하게 된다. 
- 영속성 개체에 이 변화를 계속 저장하는 것 보다는 부가세 추가와 같은 불변 로직은 Order 객체와 분리하여 독립적인 모델링을 하는 것이 좋다.
