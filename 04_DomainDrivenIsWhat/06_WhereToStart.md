## 문제공간
- 회사가 해결하고자 하는 모든 것
- 창고 관리 소프트웨어의 3가지 : 주문 관리, 재고 관리, 이행(Fulfillment)으로 나눠 볼 수 있으며, 주문 도메인의 하위 도메인에 이행 도메인이 존재하며, 재고관리 도메인은 따로 존재한다.

## 솔루션 공간
- 솔루션 공간은 도메인을 나누는 방식이다. 도메인을 나누는 방식은 도메인을 관점별로 분류를 하는 방식이다. 관점별로 분류화 하게 되면 그룹화를 하는데 큰 카테고리로 그룹화를 하고 각 큰 카테고리를 작은 카테고리로 그룹화를 해 나가면서 도메인 모델을 만들어 나간다. 카테고리 분류는 솔루션 공간을 형성하는 것이고 카테고리를 나눈 후 큰 카테고리의 세부 카테고리를 만들어 나갈 수 있기 때문에 솔루션 공간을 형성하는 작업이고, 솔루션 공간을 형성하는 것은 더 세분화된 그룹화를 하기 위한 시작점이 된다. 따라서 도메인 모델을 만들기 위한 방법, 곧 솔루션 공간을 형성하는 것은 그룹화 하는 것이다.

### 하위 도메인을 갖는 예
1. ‘주문 관리’ 도메인은 ‘주문’ 도메인을 가지고 있다.
2. ‘인벤토리 관리’ 도메인은 ‘인벤토리’ 도메인을 가지고 있다.

### 그림을 얻자
- 도메인 모델을 만들기 위해 그룹화를 하고 그룹 안에 그룹을 만들어 나가는 방식을 통해 모델에 대한 명확한 그림을 얻을 수 있다.
- 도메인 중심적으로 문제를 해결하기 위해 다이어그램을 만드는 것은 중요하다. 그룹화를 통해 재사용 가능성을 생각할 수 있고 개선 및 리팩토링을 할 수 있는 기초 토대를 만들어 나갈 수 있다.
