### 리포지토리와 엘로퀀트 코드의 
```php
<?php
//ClaimRepositoryInterface.php
   public function getProgressNotes($claimId): Collection;
   public function formatDateOfService($format='Y-m-d h:i:s'):\DateTimeImmutable;
   public function getEstimatedClaimAmount($claimId): float;
```

```php
<?php
    $claim = Claim::find(123)；
    $progressNotes = $claim->progressNotes->toArray();
    $dateOfService = \DateTime::format($claim->dateOfService, 'm-d-Y');
    $estimatedClaimAmount = $claim->estimated_claim_amount;
```

| Repository | Eloquent Specification |
|------------|------------------------|
| `$repository->all()` | `Model::all() or Model::get()` |
| `$repository->create($repository->getNextId(), $data=[])` | `Model::create($data=[])` or `$model->fill()` or `$model = new Model($data=[])` |
| `$repository->update($id, $data=[])` | `Model::update($data=[])` or `$model->association = $x;` or `$model->save();` |
| `$repository->delete($id)` or `$repository->delete` | `Model::delete($id)` or `Model::destroy($ids=[])` | 
`$repository->findAllBy($ids=[])` or `$rows=$repository->where('id', 'IN',[1,2,3]); if (!empty($rows)) {$row=$rows[0];}` | `Model::find($id=[]);` or `$row=$model->whereIn('id',[1,2,3])->get()->findOrFail()` |


### 일반적인 리포지토리 인터페이스
```php
<?php
interface Repositorylnterface
{
    public	function	all()；	
    public	function	create(array	$data);
    public	function	update(array	$data, $id);
    public	function	delete($id);	
    public	function	show($id);	
}
```

#### 모델의 리포지터리화
- 관련 기능을 그룹화 하듯 모델 자체 내에 배치하는 방식으로 관련 기능끼리는 가깝게 배치하는 방식의 코드가 좋다.

### 엘로퀀트의 단점을 해결하는 방법
- 엘로퀀트의 단점은 문제를 분리하거나, 논리의 유사한 부분을 명시적이고 명확한 방식으로 캡슐화 하지 않는 점이다. 이러한 방식은 도메인 모델의 개념을 혼란 스럽게 만들고 클래스의 목적을 덜 명확하게 만들 수 있기 있다.
- 이를 완화하는 방법은 주어진 객체가 사양을 사용하여 기능별로 나누는 것이다. 유비쿼터스 언어로 이름 지어진 술어 논리를 만들어 명시적으로 정의된 방식으로 사용할 수 있도록 만드는 것이 필요하다.


## 리포지토리 클래스의 필요성
- 엘로퀀트로 리포지토리 패턴을 사용하면 리포지토리 클래스를 통한 리포지토리 패턴을 사용할 필요가 없어지는가?
- 엘로퀀트를 사용하여 리포지토리 패턴을 구현하는 것과 리포지토리 클래스의 구현이 존재한다.
- 엘로퀀트를 사용한 리포지토리 패턴의 구현은 RDBMS에만 적용 될 수 있는 반면 리포지토리 클래스를 통한 리포지토리 패턴의 구현은 RDBMS 뿐만 다른 저장소에 대해서도 일관된 리포지토리 패턴을 적용할 수 있다는 장점이 있다.
