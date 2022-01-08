### 코드조각을 만드는 방법
- 일단은 모델을 체이닝 방식으로 확장하는 방식으로 돌아가는 코드를 만든다.
- 제약 조건을 추가할 객체, 유비쿼터스 언어의 적절한 용어를 따라 쿼리조각을 만든다.
- 다양한 서비스, 프로토콜 및 여러 클래스에서 사용되는 더 복잡한 쿼리의 경우 기능을 더 작은 조각으로 나눈다.


### 코드 조각의 장점
- 재사용 가능하며 다양한 상황에서 스코프의 조합으로 쿼리를 구성할 수 있다.
- 거대한 단위의 태스트를 하는 대신 작은 단위로 확인할 결과를 확인할 수 있는 장점이 있다.


### 코드 조각을 만들 때 주의 할 점
- 도메인 자체에서 발견되는 비즈니스 규칙 및 개념에 부합되는 코드 조각을 만들기 위해 노력할 필요가 있다.
- 도메인 컨텍스트의 경계를 잘못 판단하거나 너무 세분화 하지 않았는지 확인할 필요가 있다.


### 쿼리빌더 사양의 장점
영속성 계층의 데이터를 가져올 때, 컬렉션 보다는 쿼리 빌더를 통해 데이터를 필터링하는 것이 성능이 좋다. 사양은 백앤드에서 영속성 계층의 데이터를 백앤드의 로직을 통해 필터링을 할 수 있지만 성능 상의 문제를 피할 수 있는 수단을 제공한다.


```php
<?php
//ClaimRepositoryInterface.php
   public function getProgressNotes($claimId): Collection;
   public function formatDateOfService($format='Y-m-d h:i:s'):\DateTimeImmutable;
   public function getEstimatedClaimAmount($claimId): float;
```
- 사양을 사용하면 위와 같이 리포지토리를 만들 필요가 없다.




```php
<?php
    $claim = Claim::find(123)；
    $progressNotes = $claim->progressNotes->toArray();
    $dateOfService = \DateTime::format($claim->dateOfService, 'm-d-Y');    $estimatedClaimAmount = $claim->estimated_claim_amount;

```
