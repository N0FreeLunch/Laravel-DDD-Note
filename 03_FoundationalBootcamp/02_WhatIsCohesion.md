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
- 클라이언트와 상호작용하는 클래스는 하나의 책임을 가지고 있기 때문에 유저의 정보를 저장하는 클래스는 별도의 클래스에 캡슐화 할 필요가 있다. (도메인 계층의 서비스 레이어에 보통 위치한다.)
- 클라이언트와 직접 상호작용 하는 클래스는 모든 유저 정보를 받기 전에 유저 정보에 관한 변수를 초기화해야 한다. 이는 변수가 오염되지 않게 하기 위해서이다. (웹에서 새 리퀘스트가 들어오면 리퀘스트에 필요한 객체들이 새롭게 생성되는 것처럼)


### Refactoring code
```
<?php
use App\User;

class RegisterUSer
{
  protected $safeAttribute;
  protected $user;
  public function __construct(array $params) {
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
