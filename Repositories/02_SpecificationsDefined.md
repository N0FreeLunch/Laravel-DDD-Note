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

<====

ClaimRepository
query(ClaimSpecificationInterface $specification)

::::::::>

<interface>
ClaimSpecificationInterface
specifies(Claim $claim): bool

<===

LastestClaimSpecification
specifies(Claim $claim): bool
```
-
