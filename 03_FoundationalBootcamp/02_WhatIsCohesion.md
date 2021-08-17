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
      'isAdmin' => $this->isAdmin == false ? "NO" : "YES",
      'isPremierMember' => $this -> isPremierMember == false ? "NO" : "YES"
    ];
  }
  public function registerUser() {
    $user = new User($this -> name, $this -> username, $this->isAdmin, $this->isPremierMember);
    return $user;
  }
}
```
