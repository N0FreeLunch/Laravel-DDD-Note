## 유비쿼터스 언어를 만들기에 앞서서 할 것
- 개발자뿐만이 아니 모든 사람이 이해할 수 있는 용어로 도메인 모델링 하기
- 일반적인 비즈니스 용어에 대한 카탈로그를 만들어서 해당 도메인 언어로 잘 설명할 수 있는 능력을 길러야 한다.
- 시스템의 모든 측면을 모델링 하는 데 사용하기 위한 기초 자료가 되기 때문에 조직의 모든 구성원이 비즈니스 아이디어를 전달하고 설명하고 활용하기 때문에 정확해야 한다.


## 당면한 문제
- 창고 관리 시스템을 만들 예정이다.
- 일상적인 프로세스를 자동화 하여 더 효율적이게 만들려고 한다.
- 고객이 더 많은 제품을 배송하여 더 많은 수익을 남기고자 한다고 하자.
- 주문과 배송은 전자 상거래 플랫폼에서 관리되고 있는 상황이다. 하지만 시스템에는 추적 
- 기능이나 재고 관리가 없기 때문에 사람들이 직접 이를 기록하면서 작업하고 있다.
- 앱의 보고 기능은 다양한 수식과 합계가 포함된 여러 색상의 Excel 스프레드시트로 구성된다.
- 더 큰 창고로 확장하면서 주문 이행 시 어려움을 겪고 있음
- 위치 관리 솔루션이 없기 때문에 프로세스가 급격히 느려짐
- 단일 대량 주문을 완료하는데 10~45분이 소요



## 창고 관리 문제를 해결하기 위한 방안
- 현재 당면한 문제를 부분적으로 보는 것 보다는 전체적으로 보아 포괄적인 솔루션을 찾으려고 노력해야 한다. 왜냐하면 포괄적인 솔루션을 찾는 것은 전체적인 문제를 좀 더 쉽게 해결하기 위한 방법이기 때문이다. 포괄적인 솔루션을 찾을 수 없는 경우가 되면 부분적인 해결 방안을 모색하는 것이 좋아 보인다.
- 제품의 위치 관리 문제를 해결하기 위해 새로운 위치 계획을 고안해야 한다. 


## 도메인 용어 카탈로그
### 용어 리스트
• Order management (주문 관리)
• Inventory updates/audits (재고 업데이트 감사)
• Order tracking lifecycle/workflow (주문 수명 및 워크플로우 추적)
• Inventory management (재고 관리)
• Item tracking (품목 추적)
• Location in warehouse (창고 위치)
• Quantities (수량)
• Reserved: Amount of a product that’s already spoken for (예약됨 : 이미 언급된 상품의 수)
• Available: Amount of a product that is in stock and ready to be sold (가능 : 재고가 있고 판매 준비가 된 제품의 양)
• Expecting: Amount of a product that is on order (예상 : 주문된 제품의 양)
(backordered)
• Item lifecycle process/workflow (상품의 프로세스 및 워크 플로우의 라이프사이클)
• Fulfillment (이행)
• Pick and pack (픽업 및 포장)
• Shipping system (배송 시스템)
• Order fulfillment process/workflow (픽업 및 포장)

### 용어 리스트에 관하여
- 기본적으로 영어로 정의 된 용어라서 일부 용어의 부자연스러움이 있을 수 있다.

