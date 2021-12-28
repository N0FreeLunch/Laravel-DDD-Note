## 사양(Specifications)을 사용하는 이유
- 애플리케이션에서 클래스가 가지는 외부적인 인터페이스는 API는 노출하고, 외부적으로 드러낼 필요가 없는 비즈니스 로직을 내부에서 처리하는 캡슐화를 사용하여 세부적인 복잡성을 감출 수 있다.
- 동일한 데이터가 필요할 때 마다 도메인별로 모듈화 된 동일한 사양을 사용할 수 있는 구조로 만들어서 재사용성을 향상시킬 수 있다.
- 애플리케이션의 비즈니스 규칙이 사양 객체 내에 포함되어 있기 때문에 비즈니스 규칙이 어떻게 시행되어야 하는지 알 필요가 없고 흐름만을 알면 된다.
- 비즈니스 규칙 자체가 변경되는 경우에는 한 장소에서만 수정하면 된다.
- 좀 더 복잡한 비즈니스 요구사항을 처리하기 위해 여러 다른 사양들을 조합해서 사용할 수 있다.

---

복잡한 로직을 만들 때 여러 각각의 도메인적인 특징을 가진 사양들을 활용하여 리포지토리 메서드에 전달하여 필요한 비즈니스로직을 만들어내어 복잡성을 다룰 수 있다는 것이 핵심

---


## 사양 패턴을 구현하는 리포지토리
```
<interface>
ClaimRepositoryInterface
query(ClaimSpecificationInterface $specification)

<--------

ClaimRepository
query(ClaimSpecificationInterface $specification)

>>>>>>>>>

<interface>
ClaimSpecificationInterface
specifies(Claim $claim): bool

<--------

LastestClaimSpecification
specifies(Claim $claim): bool
```

`<--------` 위쪽의 구현은 아래쪽 (원래는 실선 화살표)

`>>>>>>>>>` 위쪽이 아래쪽을 사용 (원래는 점선 화살표)

#### 관계 해석
- 클레임 리포지토리의 query() 메소드는 인자로 클레임사양 인터페이스(ClaimSpecificationInterface)로 구현되는 인스턴스를 받는다.
- 인자로 클레임 사양 인터페이스를 받는다는 것은, 클레임 사양 인터페이스를 구현하는 객체를 받을 수 있다는 의미이다.
- 클레임 사양(ClaimSpecification)은 주어지는 클레임 모델이 가진 데이터가 사양에 맞는 데이터를 가졌는지 판별하는 specifies 메소드를 가지고 있다.
- specifies메소드는 판별하는 역할을 하므로 결과 값이 bool 타입이다.
- 사양이 모델을 인자로 받을 때 해당 인스턴스가 사양이 요구하는 데이터를 가지고 있는지 확인하는 것을 술어검사라고 하며, 술어검사를 하기 위한 메소드를 extends()라고 한다.
- 클레임 사양의 술어검사를 위한 메소드는 specifies이다.

```php
namespace Claim\Submission\Domain\Contracts;
use Claim\Submission\Domain\Models\Claim;
use ClaimSpecificationlnterface;

interface ClaimRepositorylnterface {
    public function query(ClaimSpecificationInterface $specification);
}
```

```php
namespace Claim\Submission\Infrastructure\Repositories;
use Claim\Submission\Domain\Contracts\ClaimRepositoryInterface;
use Claim\Submission\Domain\Contracts\ClaimSpecificationlnterface;

class ClaimRepository implements ClaimRepositorylnterface {
    public function query(ClaimSpecificationInterface $specification){
        return Claim::get()->filter(function (Claim $claim) use ($specification) {
                return $specification->specifies($claim);
        }
    }
}
```

#### 코드 해석
- ClaimRepositorylnterface를 구현하는 ClaimRepository는 query라는 메소드를 가지고 있다.
- query 메소드는 ClaimSpecificationInterface를 구현하는 인스턴스인 $specification라는 인터페이스를 받고
- Claim 객체에서 영속성 계층의 데이터 리스트를 받아와서
- 받아온 리스트에서 사양($specification)에서 정한 기간에 해당하는 데이터만 리스트에서 뽑는다.
- 사양에 해당한다는 것은 정한 기간에 해당한다는 것으로 여기서는 specifies 메소드를 통해서 리스트의 하나의 원소가 사양에서 지정한 데이터인 특정 기간에 해당하는지 확인을 한다.
- filter 컬렉션의 specifies 메소드의 반환값 true, false로 데이터를 뽑아낸다.
- 엘로퀀트 모델을 컬렉션으로 받을 때 각각의 row는 엘로퀀트 모델에 해당한다는 것을 확인할 수 있다.

#### 사양의 단점
- 사양 객체는 각각의 row에 해당하는 모델 객체가 적절한지 확인하기 위한 것으로 수 많은 원소를 포함하는 리스트의 경우, 사양을 해당 원소 갯수만큼 판별에 사용해야 한다. 모델로 가져오는 리스트의 원소의 갯수가 많아질수록 사양 객체를 사용하는 것의 효율이 떨어지게 된다.
- 이를 해결하기 위해 사양 객체를 사용해서 리스트의 필터링을 하기 전에 모델로 부터 가져오는 리스트의 수를 쿼리문을 사용해서 줄이는 방법을 사용해서 성능 문제를 해결해야 한다.

```php
$claimsSubmittedLastYear = Claim::whereBetween(
    "submitted_at", [
        Carbon::parse("today-l year")->toDateTimeString(),
        Carbon::parse("today")->toDateTimeString()
    ]
)->get();
```
- 엘로퀀트 모델에서 whereBetween 메소드를 사용하여 submitted_at 컬럼의 값이 whereBetween의 두 번째 인자인 배열[]의 첫 번째 값과 두 번째 값 사이에 해당하는 데이터를 뽑는다. whereBetween은 엘로퀀트 쿼리 빌더로 구성되어 있으며, 영속성 계층에서 데이터를 가져올 때 필터링이 되어 가져와 지기 때문에 성능상의 문제를 해결하는 방법이다.
- 위 코드에서whereBetween 절은 재사용가능한 형태이지 않지만, 클레임 모델의 쿼리를 만드는 부분을 리포지토리에서 정의하지 않고, 리포지토리는 조건을 형성하는데 필요한 값만을 전달하기 때문에 리포지토리와 모델간의 캡슐화의 경계를 무너뜨리지는 않는 방식으로 만들어져 있다.
- 빠른 구현에 목표를 두고 있다면, 다양한 컬렉션과 모델 파사드를 사용하는 방식으로 코드를 짤 수 있다. 사양을 사용한다는 것은 관심사의 분리와 명시적인 개념 정의를 할 수 있는 반면 위와 같이 모델을 확장하는 방식은 사양과 같이 따로 분리될 수 있는 독립적인 로직을 사용하지 않기 때문에 재사용성이 떨어지고 반복된 형태의 코드를 만들기 때문에 기술 부채의 문제가 생긴다.

### 리포지토리에 사양 정의하기
- 데이터 필터링, 결과의 수 제한 등을 통해 쿼리 실행 시간이나 결과 쿼리의 양을 줄이는 방식이 필요하다. 리포지토리에 메소드를 추가하는 방식으로 `submitWithinRange($startDate, $endDate)` 재사용성을 높일 수 있다.
```php
namespace Claim\Submission\Domain\Contracts;use Claim\Submission\Domain\Models\Claim;
interface ClaimSpecificationlnterface{
    public function specifies(Claim $claim);
}
```
- 사양은 재사용할 수 있어야 한다. 컬렉션에 적용하는 사양뿐만 아니라, 쿼리빌더 방식으로 적용하는 사양도 재사용 가능한 단위로 만들어 주어야 한다.
- 사양은 단일한 조건을 판별하는 방식으로 정의를 한다.

#### 단일한 조건으로 사양을 분리했을 경우
- 날짜 범위와 상태에 대한 두 가지 개별 사양이 만들어진다.
1. 날짜 범위를 필터링 할 때 쓰는 조건
2. 상태를 필터링 할 때 쓰는 조건

```
<?php
use Claim\Submission\Infrastructure\Repositories\ClaimRepository;
SclaimRepository = new ClaimRepository();

$latestClaims = $claimRepository->query(
    new LatestClaimSpecification(
        new \DateTimeImmutable('-30 days')
    );
```

