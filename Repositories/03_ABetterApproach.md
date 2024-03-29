### 쿼리빌더 메소드 사용하기
```php
$usersAddr = User::with('address') //join on address relation
    ->where('is_active', true) //returns OueryBuilder object
    ->where('age', '>', $startingAge)
    ->where('gender', $gender)
    ->where(function ($query) use ($request) {
        $query->whereHas('posts', function ($query) use ($request) {
            $query->where('is_published', $published);
        });
    })
    ->get() //fetches result and returns a Collection->address; //grabs the 'address' relation--included via //the call to with() in the first line
```
- 쿼리 빌더의 메소드를 연속적으로 이어서 사용하는 것은 편리하고, 빠르고, 표현적인 접근법이다.
- 하지만 이런 좋은 점 이면에는 큰 책임이 따른다.

### 의미론적인 코드 조각
- 작은 단위의 기능 조각이 되는 코드를 짜서 재사용 가능성을 높이고 추가 기능을 구축할 수 있는 더 나은 아키텍처를 만드는 것이 낫다.
- 모델을 체이닝 방식으로 확장하는 방식의 코드는 가독성을 떨어뜨릴 수 있다.
- 기능의 조각을 만들어 나가는 개발 방식은 다른 개발자들에게 읽히기 쉬운 코드를 만드는 장점이 있다.

#### 의미론적인 스코프이름
- 스코프를 만들 때는 scopeIsActive(), scopeIsMale(), scopeIsFemale(), scopeHasPublished() 와 같이 메소드의 이름이 의미론적이어야 한다. 
- 쿼리의 세부 사항을 정하는 것이 아니라, 메소드 명만 보고서도 해당 스코프를 사용할 수 있도록 만들어 주는 것이 중요하다.
- 새로운 리포지토리 메소드 안에 새로운 동작을 캡슐화하며, 동작이 제공하는 목적과 의도를 완전히 포착할 수 있는 유비쿼터스 언어에 따라 메소드 이름을 지정한다.

```php
public function getPublishedMales(\DateTimeImmutable $isAtLeastAge): OueryBuilder
{
    $users = User::with('address')
        ->isActive()
        ->isMale()
        ->isAtLeastAge($age)
        ->hasPublished();
}
```

#### 의미론적 이름의 스코프를 작성하는 이점
- 코드가 분리되어 적절한 쿼리를 작성할 수 있다.
- 이름이 도메인 기반인 유비쿼터스 언어의 정의를 통해 쿼리 언어를 만들 수 있다.
- 구현의 세부 사항을 파고 들지 않고도 코드가 실제 수행하는 작업을 한 눈에 명확하게 이해할 수 있다.
- 모델별로 의미 있는 제약 조건을 설정하였다.

### 코드 조작
#### 코드조각을 만드는 방법
- 일단은 모델을 체이닝 방식으로 확장하는 방식으로 돌아가는 코드를 만든다.
- 제약 조건을 추가할 객체, 유비쿼터스 언어의 적절한 용어를 따라 쿼리조각을 만든다.
- 다양한 서비스, 프로토콜 및 여러 클래스에서 사용되는 더 복잡한 쿼리의 경우 기능을 더 작은 조각으로 나눈다.

#### 코드 조각의 장점
- 재사용 가능하며 다양한 상황에서 스코프의 조합으로 쿼리를 구성할 수 있다.
- 거대한 단위의 테스트를 하는 대신 작은 단위로 결과를 확인할 수 있는 장점이 있다.

#### 코드 조각을 만들 때 주의 할 점
- 도메인 자체에서 발견되는 비즈니스 규칙 및 개념에 부합되는 코드 조각을 만들기 위해 노력할 필요가 있다.
- 도메인 컨텍스트의 경계를 잘못 판단하거나 너무 세분화 하지 않았는지 확인할 필요가 있다.

### 쿼리빌더 사양의 장점
- 영속성 계층의 데이터를 가져올 때, 컬렉션 보다는 쿼리 빌더를 통해 데이터를 필터링하는 것이 성능이 좋다. 사양은 백앤드에서 영속성 계층의 데이터를 백앤드의 로직을 통해 필터링을 할 수 있지만 성능상의 문제를 피할 수 있는 수단을 제공한다.
- 전통적인 리포지토리를 사용하는 것과 달리 라라벨에서는 컬렉션 및 로컬 및 글로벌 스코프를 모델에 정의하는 방식으로 관계형 데이터베이스로 할 수 있는 거의 모든 작업을 수행하는 빠르고 현실적이고 직접적인 접근 방식을 이용할 수 있다.

### 리포지토리 패턴이 의미 없는가?
- 리포지토리 모델은 영속성 계층과 도메인 계층을 분리하기 위한 수단으로서 유용하다. 스코프와 컬렉션을 이용하는 방식은 관계형 데이터베이스를 사용할 때만 이용 가능한 수단이기 때문에 데이터베이스 의존성이 존재한다.
- 리포지토리 모델은 관계형 디비, 레디스, 엘라스틱서치, 인메모리 디비 등 다양한 저장소를 연결하는 방식에 공통적으로 사용할 수 있다.
