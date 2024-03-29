## 개발자가 쏟는 시간
- 개발자의 몸값은 비싸다. 개발자의 시간은 서비스를 만들어 내기 위한 도메인 모델을 구현하는데 집중할 수 있어야 한다.
- 언어의 상세 스펙이나 웹에서 사용하고 있는 기술들의 세부 스펙 등에 대한 상세한 이해가 있으면 좋겠지만 이들을 많은 시간을 투자하지 않고도 이해하고 사용할 수 있어야 하며 이미 만들어져 있고 잘 관리되는 기술 스텍들을 적절히 사용하여 서비스를 위한 도메인 모델을 만드는데 집중할 수 있어야 한다.

## 소스코드가 복잡할 때 생기는 문제
- 소스코드의 작동 방법을 이해하기 어렵거나 도메인의 작동 방식에 대한 지식 부족등이 생긴다면 하나의 변경 사항을 처리하는데 몇 주가 걸리는 경우가 생길 수도 있다.
- 소스 코드가 복잡성하다면 사이트를 유지 보수 하는 시간을 예측하기 어려운 문제가 생긴다.
- 관리하기 힘든 애플리케이션은 변화하는 흐름에 대응하지 못하고 경쟁력을 잃을 수 있다.

## 애플리케이션이 나아가야 할 방향
- 오랜 기간 기술의 변화에 뒤처지지 않고 변화를 받아들이고 수용할 수 있는 구조여야 한다.
- 변경사항이 생겼을 때 쉽게 업데이트 할 수 있도록 이해하기 쉬워야 한다.
- 시간이 지남에 따라 계속적인 업데이트가 이뤄질 수 있어야 한다.

## 라라벨의 장점
- 프레임워크의 가이드를 따라서 애플리케이션을 다루게 되면 프레임워크가 제공하는 버전업과 새로운 기능들을 그에 맞춰 이용할 수 있어 변화에 대응하기 쉽다.
- 웹 애플리케이션을 빌딩하는데 필요한 도구들을 충분히 제공한다.
- 활성화된 커뮤니티를 통해서 해결하기 어렵거나 좀 더 상세한 지식이 필요한 경우 접근할 수 있다.
- 잘 설명된 문서를 제공하여 프레임워크를 다루는 누구나 일관된 방향으로 애플리케이션을 다룰 수 있다.

## 애플리케이션은 저렴하고 데이터는 비싸다
- 시스템의 가장 중요한 것은 데이터이다. 데이터를 잘 다룰 수 있어야 애플리케이션이 변경사항에 대처하기 쉽다.
- 애플리케이션이 데이터를 잘 다룰 수 있는 구조를 갖는다는 것은 데이터베이스의 설계가 잘 되어 있어 필요한 데이터를 변경 사항에 맞게 가공하기 쉬운 형태를 갖는 것을 뜻한다. 단순히 과제나 문제를 해결하기 위해 데이터베이스의 스키마를 만드는 것이 아니라 공을 들여서 앞으로의 변경사항에도 대응할 수 있도록 도메인과 시스템에 대한 이해를 바탕으로 스키마를 설계하는데 공을 들이는 것이 중요하다.
- 또한 데이터가 남아 있어야 애플리케이션을 새로 작성하든 리펙토링을 하든 어려운 문제가 생겼을 때 어떻게든 해결할 수 있다. 더 나은 서비스를 위해 애플리케이션을 교체, 재구축 또는 리팩토링을 할 수 있기 때문에 데이터를 보존하는 구조에는 충분한 투자하는 편이 좋다.
- 데이터베이스 구조에 충분한 시간을 투자해서 올바른 설계를 하는 것은 중요하다. 회사에서 개발자의 교체는 흔한 일이고 언제가 시스템에 대해 잘 모르는 사람이 수정하는 날이 오게 될 경우 비즈니스의 존속 자체가 위태로워지게 된다.
