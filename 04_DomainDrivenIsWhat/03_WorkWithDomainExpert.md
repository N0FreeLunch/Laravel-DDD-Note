## 도메인 전문가와 어떻게 협업할 것인가?
- 어려운 개발 용어로 도메인 전문가를 곤란하게 하지 않는 것이 좋다.
- 개발적인 측면 보다는 시스템의 비즈니스적인 측면을 중점으로 설명하자.
- 도메인 모델을 만들기 위한 모든 요소들을 비즈니스적인 설명이 가능하도록 도메인과 도메인 전문가의 용어로 바꾸어 설명하고 이해할 수 있도록 하자.
- 비즈니스 모델의 개념이 불분명한 경우 명확한 로직으로 정의할 수 있도록 하는 것이 중요하며, 어려운 개념이나 용어도 잘 풀어서 설명될 수 있도록 유비쿼터스 언어를 정의하는 것이 필요하다.
- 하나의 용어도 다양하게 쓰일 수 있기 때문에 구체적이고 세부적인 맥락 내에서 도메인 용어 및 언어를 사용하도록 하자.
- 회사 전체에서 합의 될 수 있는 방식으로 도메인을 설계하도록 하자.

## 못생긴 디자인이라도 좋다. 일단 도메인 모델을 만들어 보자.
- 도메인 모델을 처음 만들 때는 조잡해도 괜찮다. 일단 비즈니스가 어떻게 동작하는지 설명하기 위한 표현을 하는 편이 좋다. 그림 및 메모를 통해 비즈니스의 동작을 묘사하는 것도 좋다. 이를 토대로 점차 뼈대, 골격, 스케치 등을 확장하는 방식으로 도메인 모델을 만들면 된다.
- 팀에서 얻은 새로운 통찰력을 바탕으로 CI/CD를 통한 지속적인 개선 및 업데이트를 통해 도메인 모델을 개선하면서 애플리케이션을 만들어 나가야 한다.

## 실패로부터 배우자.
- 애플리케이션의 개발 속도는 한계가 있다. 개발 보다는 비즈니스의 변화가 훨씬 빠르며, 비즈니스가 시행착오를 반복하는 만큼 개발도 시행착오를 반복하게 된다. 비즈니스 영역에서 미리 시행착오를 겪고 개선된 비즈니스 모델을 개발에 적용하는 방법이 개발 비용이 비교적 적게 드는 방법이지만, 애플리케이션을 통해서 비즈니스 모델을 시험해야 하는 경우에는 모델 리팩토링으로 애플리케이션을 개선한 결과가 비즈니스적으로 좋지 못할 수도 있다. 실패했다고 개발에 실패한 것으로 생각하기 보다는 실패를 통해 얻은 통찰로 비즈니스를 개선할 통찰을 얻고 계속적인 변화를 통해 비즈니스 모델을 개선하는 것이 필요하다.
- 비즈니스에 대한 새로운 통찰을 얻었다면, 애플리케이션 뿐만 아니라, 비즈니스 모델을 설명하는 유비쿼터스 언어 또한 업데이트 해야 한다.
- 애플리케이션 중심의 비즈니스를 다듬어 나가기 위해서는 비즈니스 모델을 빠르게 개선하고 비즈니스적인 가치가 있는 모델인지 더 나은 모델이 있는 것인지 확인하는 과정이 필요하다.

## 애자일의 핵심
- 지속적으로 피드백을 주고받는 것이 핵심이다.
- 지속적인 피드백을 받기 위해서는 구현된 도메인 모델을 테스트 할 수 있어야 한다.
- 도메인에 대한 정보와 지식은 개발자, 도메인 전문가, 최고 경영진 모두가 공유해야 하며 모두가 도메인 모델에 대한 피드백을 수행할 수 있어야 한다.

## 정보와 지식의 흐름
- 도메인 모델을 설계하기 위해서는 개발자도 현재의 비즈니스적인 상황에 대한 이해를 잘 하고 있어야 하고, 도메인 전문가도 비즈니스가 이뤄지고 있는 애플리케이션의 상황을 잘 알고 있어야 하며, 경영자도 애플리케이션과 비즈니스적인 상황을 잘 알고 적절한 판단을 할 수 있어야 한다. 서로의 영역의 정보가 독립되어 존재하는 것이 아니라, 서로 서로가 정보를 잘 알기 위해서 정보와 지식이 회사 전체에서 흐르도록 만드는 것이 중요하다. 
