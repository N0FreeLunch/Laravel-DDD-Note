## 라라벨의 장점
- 라우팅, 이벤트 관리, 데이터베이스 엑세스 등 많은 세부 사항을 추상화하여 제공한다.
- DDD의 핵심은 시스템 전반에서 패턴과 빌딩 블록은 기본 구성 요소로 사용하여 개발하는 것이다.
- 빌딩 블록이라는 것은 시스템을 구축하기 위해 뭔가 쌓아갈 수 있는 재사용가능한 조각을 의미한다.
- 라라벨은 웹 시스템에 필요한 이미 만들어진 패턴들이 존재한다. 웹 시스템을 구축하기 위한 인프라적인 도구들을 만들 필요가 없이 라라벨이 이를 제공한다. 라라벨의 도구를 사용해서 도메인 로직에만 집중할 수 있다.
- 하지만 이를 위해서는 라라벨이라는 도구를 잘 알아야 한다. 라라벨이 제공하는 추상화를 잘 이용할 수 있어야 한다라는 단점은 있다.

## 리포지토리 패턴을 배우는 이유
- 도메인 오브젝트의 데이터 저장 및 획득 방법을 관리하기 위해서 책임을 분리하는 방법을 알 수 있다.
- 클라이언트가 동일한 메소드 컬렉션으로 작업할 수 있도록 하는 구조를 제공하는 방법을 알 수 있다.
- 유용한 패턴으로 입증된 추상화를 배울 수 있다. (적어도 웹 개발의 리포지토리 유형이 있다. 이를 사용해서 코드를 짜는 법을 연습할 것.)

## 라라벨에서 리포지토리 구현의 종류
### 컬렉션 기반 리포지토리
- 객체라는 것은 그 객체가 지닌 여러 메소드를 사용해서 해당 객체에게서 원하는 양상을 만들어 내고 객체로부터 원하는 데이터를 획득하는 방식으로 사용된다.
- 리포지토리에서 컬렉션이라는 것은 라라벨의 함수형 라이브러리인 컬렉션을 의미하는 것은 아니다.
- 컬렉션이라는 디자인 패턴이 존재하고 컬렉션이란 디자인 패턴은 어떤 방식으로 메소드를 정의해서 데이터를 저장하고 획득하는 방식을 의미한다.

### 쿼리 기반의 리포지토리
- 복잡한 커스텀 쿼리를 처리하기 위한 리포지토리

## 라라벨 컬렉션
- 라라벨 컬렉션은 스테로이드 배열의 역할을 한다.
- 프로그래밍 언어에서 스테로이드라는 것은 아주 강력하게 기능이 발전된 형태를 의미한다. 그러니까 스테로이드 배열이라는 것은 php의 그냥 배열이 아닌 굉장이 강력한 기능들을 탑재한 배열의 역할을 한다는 것이다. 마치 배열이 운동선수 도핑을 한 것처럼 강력한 기능을 가지게 되는 것을 의미한다.
- 스테로이드란? : https://english.stackexchange.com/questions/487060/meaning-of-an-object-on-steroid/487062


## 엘로퀀트 컬렉션의 특징
- 엘로퀀트 컬렉션은 대부분의 SELECT 쿼리를 대체할 수 있다.
- 함수형 표현을 통해 리스트를 다루는데 알기 쉽고 편리한 방법을 사용한다. 따라서 세세한 쿼리를 사용하지 않고서도 엘로퀀트 컬렉션을 통해 필요한 값들을 선택적으로 취득 가능하다.
- 엘로퀀트 모델은 데이터 리스트를 다루기 위한 다양한 종류의 컬렉션 메소드를 제공한다.
- 엘로퀀트 모델의 컬렉션은 데이터베이스 쿼리의 결과 리스트에서 특정 레코드를 선별하는 깔끔하고 우아한 방법을 제공한다.
- 엘로퀀트 모델에서 컬렉션 메소드를 사용하면 엘로퀀트 모델 객체가 아닌 엘로퀀트 컬렉션 객체를 반환한다.
- 엘로퀀트 컬렉션을 통해서 깔끔하고 지정된 메소드로 컬렉션 처리가 가능하기 때문에 별도의 리포지토리를 만들 필요가 없다.
- 라라벨 컬렉션의 존재는 라라벨에서 리포지토리 추상화를 옹호하지 않는다.

### 리포지토리의 복잡성 관리 방식
- 데이터를 일관된 형식을 통해서 제공한다.
- 점점 더 구체적이고 세분화된 형태로 사용한다.

### 리포지토리보다는 엘로퀀드 사용
- 리포지토리 객체를 만들기 보다는 엘로퀀트 모델을 사용하라.
- 리포지토리에 데이터를 검색하는 방법을 추가하는 코드가 답답해지는 과정을 피하고 엘로퀀트 컬렉션을 이용하여 우아하게 결과 집합을 반환하는 방식을 사용하자.

#### 리포지토리를 사용한 코드 표현의 예
```php
$claims = $claimRepository
          ->findBy('patient_id'， $patientId)
          ->addWhere('submitted_at', 'BETWEEN",[
              Carbon::parse("today –l year")->toDateTimeString(), Carbon::parse("today")->toDateTimeString()
          ])->getResults();

foreach ($claims as $claim) {
    $cptCodeCombos[] = $cptComboRepository->find($claim->getId())->getResults()); 
}
```

#### 엘로퀀트와 컬렉션을 사용한 코드 표현의 예
```php
Claim::where("patient_id",$patientId)
       ->whereBetween("submitted_at", [
          Carbon::parse("today – l year")->toDateTimeString(),
          Carbon::parse("today")->toDateTimeString()
])->get()
->cptCodeCombo
->cptCodes;
```

#### 리포지토리의 단점
- 리포지토리 클래스를 일관되게 유지할 수 있는 기본 메소드 이외에 복잡한 요구사항을 만족하기 위한 예외적인 메소드들이 추가되게 되면, 리포지토리의 재사용성이 감소하고, 아주 긴 이름의 메소드명을 붙여야 하는 등의 코드 복잡성이 추가 된다.
- 긴 이름의 메소드 예시 : GetCountOfProvidersWithRegisteredPatientsWithinLastYear(), CptCodesForPracticeWithinDateRange($sdate, Sedate)

#### 엘로퀀트 컬렉션의 장점
- 리포지토리에 정의되는 예외적 메소드의 추가를 하지 않는 방식으로 사용하지 않고 엘로퀀트 모델과 컬렉션의 사용법으로 표현이 가능하기 때문에 일관성을 유지할 수 있고, 라라벨의 엘로퀀트와 컬렉션을 알고 있는 사람은 해당 기능이 어떤 기능인지 쉽게 알 수 있다.
- 클래스를 답답하게 만드는 긴 이름의 메소드 사용을 피할 수 있다.
- 원시 쿼리와 내부 작동을 추상화하여 사용성을 높이지만, 추상화된 표현의 예외적인 사용법을 줄이고 엘로퀀트와 컬렉션의 인터페이스를 이용하기 때문에 사용하기 편하다.

#### 리포지토리를 쓰는 이유
- 영속성 계층을 숨기는 것.
- 영속성 계층이 일관된 리포지토리 인터페이스를 제공하게 하기 위한 것.


#### 리포지토리의 인터페이스 만들기
ClaimRepositoryInterface
```
nextldentity(): int
claimOfld(): Claim
save(Claim $claim)
saveAII(array $claims)
remove(Claim Sclaim)
removeAII(array Sclaims)
```

#### 인터페이스를 구현하는 리포지토리 클래스 만들기
SqlClaimRepository
```
nextldentity(): int
claimOfld(): Claim
save(Claim $claim)
saveAII(array $claims)
remove(Claim $claim)
removeAII(array $claims)
```

RedlsClaimReposltory
```
nextldentity(): int
claimOfld(): 
Claimsave(Claim $claim)
saveAII{array $claims)
remove(Claim $cteim)
removeAII(array $claims)
```

DoctrlneClalmReposltory
```
nextldenlity(): int
claimOfld(): Claim
save(Claim $claim)
saveAII(array $claims)
remove(Claim $claim)
removeAII(array $claims)
```

- 일관된 인터페이스를 갖는 리포지토리를 interface를 정의하여 implements 하는 방식으로 만들어 준다.


#### 리포지토리의 기능
- 영속성 계층의 컨텍스트에서 각 기능에 맞는 인터페이스를 만들어 사용
- 하지만 웹 개발에서 이런 리포지토리를 만들어 쓰는 것은 과도함
- 라라벨 엘로퀀트는 리포지토리 객체와 비슷한 인터페이스를 제공하는 객체를 만들어 줌
- 리포지토리를 만들어 쓰는 과도함을 엘로퀀트로 대체하여 리포지토리 코드 작성에 대한 무게감을 줄일 수 있다.


#### 관계 모델의 리포지토리
데이터베이스 레코드에 대해 사용가능한 ID를 생성하거나, 외래키가 위반되지 않았는지 확인하는 것 => 도메인을 표현하는 객체의 라이프 사이클에서 생성 시 이러한 관계들을 한 번만 확인하는 방식으로 구성되어 있다. 
- 레디스와 같은 키-벨류 저장방식은 이런 관계 모델이 가지고 있는 특성을 가질 필요가 없다.
