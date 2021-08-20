## 응집력이란 무엇인가?
- 요소들의 조합을 통해 기능적으로 잘 정의된 단일 작업을 만드는 것이 응집력이다.
- 조금 다른 말로 하면 모듈들의 조합을 통해 한 단위의 비즈니스 로직을 역할을 만드는 것이다.

## 응집력 있는 시스템을 만들기 위해서는 
- 요소들을 잘 나눠야 한다. 기능별 모듈화를 잘 해야 한다.
- 하나의 클래스에 너무 많은 책임을 부여하지 않고 관심사에 따른 작동만 하도록 오버헤드가 적은 클래스를 만들어야 한다.
- 클래스는 자신의 책임 내에서 확장 가능하게 만들어 져야 한다.

## 클래스의 설계 방식
- 관심사의 분리를 통해서 특정 주제에 관한 단일한 책임을 표현 할 수 있게 만들어야 한다.
- 여러 클래스는 하나의 그룹을 형성하기 위한 부품 단위로 구성된다고 보면 된다.
- 유비 쿼터스 언어로 서술된 도메인 로직을 만들어 내기 위해 조합할 모듈들을 만들어 나가는 개념으로 클래스를 만들어야 한다.
- 클래스는 테스트 가능하고 재사용 가능해야 한다.
- 핵심 시스템 구성 요소 사이에서 명확한 경계로 분할되는 방식으로 만들어 져야 한다.
- 여럿이 합쳐 결합 될 수 있는 단위로 만들어져야 한다.


## 응집성이 좋지 않은 코드의 예
### 내부 로직 코드
```
<?php
namespace App\Registration;

use App\User;
Class RegisterUser
{
  protected $name;
  protected $username;
  protected $isAdmin;
  protected $ispremierMember = false;
  
  public function setName($name) {
    $this -> name = $name;
  }
  
  public function setUsername($username) {
    $this -> username = $username;
  }
  
  public function makeAdmin() {
    $this -> isAdmin = true;
  }
  
  public function getUserAttributes() {
    return [
      'name' => ucfirst($this -> name),
      'username' => $this -> username,
      'isAdmin' => $this -> isAdmin == false ? "NO" : "YES",
      'isPremierMember' => $this -> isPremierMember == false ? "NO" : "YES"
    ];
  }
  
  public function registerUser() {
    $user = new User($this -> name, $this -> username, $this->isAdmin, $this->isPremierMember);
    return $user;
  }
  
}
```

### 클라이언트를 가정한 태스트 코드
```
<?php

use App\Registration\RegisterUser;

$param = [`name` => `Jesse`, `username` => `debdubstep`];

$userRegister = new RegisterUser();
$userRegister -> setName($params[`name`]);
$userRegister -> setUsername($params[`username`]);
$user = $userRegister -> registerUser();
```

#### 내부 로직 코드를 보면
- username이 이미 존재하는 이름인지 중복성 검사 로직이 들어갈 수 있다. 중복 검사 로직을 setUsername 메소드에 넣는 것 보다는 로직의 크기가 커지니까 분리하는 편이 좋을 것이다.
- `registerUser()` 메소드는 User 객체에 자체 멤버(`$this -> name, $this -> username, $this->isAdmin, $this->isPremierMember`)를 전달하지만 실제로 디비에 데이터를 저장하는지는 불분명한 코드이다. 저장소에 저장하는 로직이라면 `$userRegisterRepository -> registerUser();` 방식으로 레포지토리 클래스를 만들어 저장하는 로직임을 분명히 보여 주는 것이 좋다.

#### 클라이언트 코드를 보면
- 가상의 $param을 가정하고 이를 RegisterUser 객체를 사용하여 데이터를 넣는 작업을 하고 있다.
- 클라이언트 쪽에서 이름을 객체에 집어 넣고, 유저 이름을 객체에 집어 넣고, 클라이언트가 저장하는 로직을 만들고 있다.
- 클라이언트 쪽에서 작업 하나 하나를 지정해 주는 것은 잘못된 디자인이다. $param을 전달하기만 하면 이름이 알아서 세팅되고, 유저 이름이 알아서 세팅되고, 이를 디비에 저장해 주는 것 까지 자동으로 되어야 한다.
- 클라이언트에게 내부 로직의 세세한 부분 하나하나를 컨트롤하게 하는 것이 나쁜 디자인이다. 클라이언트는 필요한 옵션만 지정하고 실행하면 내부 로직이 알아서 돌아가는 방식을 이용해야한다.
- 클라이언트 쪽에서 저장할 데이터 하나하나씩 지정하기 때문에 필요한 디비에 저장하기 전 필요한 데이터가 모두 모여 있는지 확인하는 코드가 없다. 위 코드대로라면 클라이언트 쪽에서 이를 확인하는 코드도 따로 만들어 호출해 줘야 할 것 같다.

#### 웹에서의 예시 (새 계정을 등록하는 절차의 예)
- 일부 필드가 누락되어 있다면 부족한 데이터가 있다는 메시지를 전달 받고 화면에 표시해준다. 
- 제출을 누르면 앱에서 요청을 처리하고 프로필 페이지로 이동한다.
- 클라이언트 쪽에서 알고 싶어하는 것은 아이디를 생성할 수 있냐 생성할 수 없으면 무엇이 부족해서 생성할 수 없냐 아이디가 제대로 만들어 졌냐의 문제이다.
- 이 과정은 서버의 어떤 클래스의 메서드를 호출하고 해당 클래스의 메소드를 어떻게 사용해야 하는지에는 관심이 없다.

#### 코드 개선 방법
- 클라이언트와 직접적인 상호작용을 하는 코드는 유저 정보를 받고 유저 계정이 생성되는지 안 판단하는 것에 관심이 있다.
- 클라이언트와 직접적인 상호작용을 하는 클래스는 유저 정보를 받으며, 유저 정보가 제대로 입력되었는지를 판단하는 책임을 갖는 클래스이다. (작업 처리의 전제 조건이 충족 되었는지 확인)
- 클라이언트와 상호작용하는 클래스는 하나의 책임을 가지고 있기 때문에 유저의 정보를 저장하는 클래스는 별도의 클래스에 캡슐화 할 필요가 있다. (도메인 계층의 서비스 레이어에 보통 위치한다. 캡슐화란 외부에서 객체 및 클래스를 접근 할 때 객체 및 내부의 세부적인 구현을 감춘다는 의미를 가지고 있다.)
- 클라이언트와 직접 상호작용 하는 클래스는 모든 유저 정보를 받기 전에 유저 정보에 관한 변수를 초기화해야 한다. 이는 변수가 오염되지 않게 하기 위해서이다. (웹에서 새 리퀘스트가 들어오면 리퀘스트에 필요한 객체들이 새롭게 생성되는 것처럼)


### Refactoring code
```
<?php
use App\User;

class RegisterUser
{
  protected $safeAttribute;
  protected $user;
  
  public function __construct(arrayparams) {
    $attributes = User::fillableFromArray($params);
    $this -> safeAttributes = $attributes;
    $this -> user = new USer();
  }
  
  public function makeAdmin() {
    $this -> user -> admin = true;
  }
  
  public function makePremiumMember() {
    $this -> user -> premiumMember = true;
  }
  
  public function getUser() {
    return $this -> user;
  }
  
  public function registerUser() {
    $this -> user -> fill($this -> safeAttributes);
    $this -> user -> save();
  }
}
```

#### 리펙토링 코드 해석
- 생성자 주입을 통해 $params 데이터를 받는다. 생성자는 객체 생성과 동시에 반드시 호출되는 것이기 때문에 객체를 사용하기 위해서 반드시 먼저 받아야 하는 값을 받아야 한다. 클라이언트에서 RegisterUser 객체를 사용하기 위해서는 반드시 기본 정보 name과 username을 받아야 하고 이 정보를 $params에 담고 있다.
- 클래스 속성을 참조하는 코드는 참조 하는 코드를 잘 못 사용할 경우, 속성이 오염 될 가능성이 있다. 그래서 보통 단 한번만 호출하는 생성자를 통해서 처음 지정해야 하는 값을 $param으로 전달 해 주었다. 또한 생성자에는 보통 초기화 로직을 적어 준다. 생성자를 호출하면 객체는 초기 상태로 바뀐다. 생성자를 통해 속성을 할당하면 초기화와 동시에 속성을 할당하는 방식으로 사용하기 때문에 클래스, 객체의 내부 속성을 깔끔하게 관리할 수 있다.
- `User::fillableFromArray($params)`는 User 모델의 엘로퀀트의 메소드 fillableFromArray를 호출하는 것이며, 메소드 명은 배열에서 '필요한 값만 추려낸다'(fillable)는 의미를 가지고 있다. fillable기능을 담고 있기 때문에 빈 배열이 들어들어 왔을 때의 처리를 생략할 수 있다.
- `setName`, `setUsername`이란 메소드명에서 `makeAdmin()`, `makePremiumMember()`으로 변경되었다. 생성자에서 admin 속성과 premiumMember 속성을 받지 않은 것은 클라이언트에 추가 옵션으로 제공하기 위해서이다. 또한 `$this -> user ->` 방식으로 엘로퀀트 모델의 컬럼을 지정하는 방식으로 사용되고 있기 때문에 호출과 동시에 모델에 저장을 준비하는 (save 메소드를 호출하지 않았기 때문에 데이터를 저장할 준비만 하는 것) 기능까지 갖추었다. 기존에는 모델에 저장하는 방식이 아니라 RegisterUser 객체 내부에 저장하는 방식을 사용했다.
- 클라이언트의 관점에서 볼 때 리펙토링 이전보다 훨씬 디테일 하게 컨트롤해야하는 메서드의 수가 적다는 것을 확인할 수 있다.

#### 데이터의 저장
- 도메인 모델의 관점에서 애플리케이션 계층의 데이터를 영속성 계층에 전달하는 디테일한 사항은 없어도 되는 부분이다.
- 디테일한 부분은 별도의 클래스를 만들어서 캡슐화 해 주는 편이 좋다. 이런 계층 간의 데이터 전달에 관한 사항을 Factory 접미어를 붙여 UserFactory 같은 방식으로 만든다.

## 낮은 응집성과 높은 응집성
### 낮은 응집성이란?
- 개념적으로 정의된 어떤 기능을 만들기 위해 메서드의 조합으로 로직을 처리할 수 있어야 하는데, 이와 반대로 서로 연결되는 관계를 가지지 않는 메소드, 메소드가 서로 연결되지 않는 것 (예를 들어 컨트롤러의 메서드와 같은 방식)
- 클래스의 프로시저 간에 공유 리소스를 사용하지 않는다. (프로시저 : 로직 처리만 하고 결과 값을 반환하지 않는 것, 클래스에서는 반환이 없는 메소드라고 생각하면 된다.)
- 메소드를 사용하면 메소드의 정의에 따라 특정 멤버 변수의 값이 변경되는 것이 아니라, 하나의 메소드는 하나의 멤버 변수에만 관여하는 방식을 가지고 있다.

#### 응집성이 낮은 클래스의 예

| some class       |
|------------------|
| +param1 : string |
| +param2 : int    |
| +param3 : array  |
|                  |
|+ method1() {\/* uses param1; \*\/} |
|+ method2() {\/* uses param2; \*\/} |  
|+ method3() {\/* uses param3; \*\/} |  

(+는 public을 의미한다.)
- 특정 멤버의 조회 변경과 같은 단일 작업을 위해 메소드가 만들어져 있기 때문에 재사용이 어렵지만 확장하기 쉬운 구조이다.

### 높은 응집성이란?
- 도메인 계층에서 개념적인 요소들 간에 경계를 만들어 나가기 위해 객체가 상호 작용하는 방식을 고려한 디자인을 해야 한다.
- 개념적으로 정의된 단일 목표 또는 작업에 기여하기 위한 개별 요소들로 만들어 져야 한다.


