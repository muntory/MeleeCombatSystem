# MeleeCombatSystem

## 1. 소개
학습을 위해 소울라이크 근접 전투 시스템 구현을 목적으로 UE5로 제작한 포트폴리오입니다. 

 ![meleeCombat](https://github.com/muntory/MeleeCombatSystem/assets/38274669/0a5ca7fb-0ee7-4970-a423-8d69efa420f0)

---

## 2. 구현 특징

### 1) 캐릭터 장비 상속구조

![장비 클래스 다이어그램](https://github.com/muntory/MeleeCombatSystem/assets/38274669/b08db77d-490a-4199-9aea-4f0f11207f3c)


확장성을 고려하여 새로운 무기의 업데이트를 쉽게 할 수 있도록 장비를 클래스 상속구조로 구현하였습니다. 새로운 무기를 추가하기 위해 무기에 맞는 애니메이션 몽타주들과 무기 스탯을 블루프린트 에디터에서 추가하면 쉽게 새로운 무기를 업데이트 할 수 있습니다.

![image](https://github.com/muntory/MeleeCombatSystem/assets/38274669/7253d43a-d59b-487a-ad13-ada4fdc624f8)
![image](https://github.com/muntory/MeleeCombatSystem/assets/38274669/b29bcc9e-b4b4-4b29-b54f-3871a7de05e4)


---

### 2) 여러 종류의 애님 노티파이와 애님 노티파이 스테이트

![image](https://github.com/muntory/MeleeCombatSystem/assets/38274669/d6a89bcf-0838-43b6-a810-80e4efc3eac3)

![image](https://github.com/muntory/MeleeCombatSystem/assets/38274669/0e3d66a4-2e31-4752-a439-86fc68e05b2d)


애니메이션 중에 상태가 달라져야 하는 경우가 많습니다. 예를 들어 무기를 꺼내고 집어 넣는 애니메이션 중에 무기가 부착되어야 할 소켓의 위치가 달라진다거나 콤보 공격을 계속 이어나갈 것인지 말 것인지, 또는 공격 애니메이션 중에 무기와 공격대상이 충돌했는지 탐지하는 작업등이 필요합니다. 이를 위한 다양한 AnimNotify와 AnimNotifyState를 구현하였습니다. 

---

### 3) 게임시스템을 위한 액터 컴포넌트
![image](https://github.com/muntory/MeleeCombatSystem/assets/38274669/e8c989a4-a91a-4c52-98eb-886b87aaa8a4)

게임의 전투시스템을 구현하는 데 수많은 요소가 필요합니다. 캐릭터가 무기를 장착하고 무기 종류에 맞게 상대를 공격하는 것, 캐릭터의 체력이나 스태미나와 관련된 스탯, 캐릭터가 어떤 동작 중인지 캐릭터가 공격 가능한 상태인지 죽었는지 판단하는 등 여러가지 구현요소가 많은데 만약 이 모든 것을 게임 캐릭터 클래스에 구현한다면 너무 비대하게 커져서 관리하기는 물론 추후에 다른 종류의 새로운 게임 캐릭터를 추가하기 불편해집니다.
게임 시스템에 관련된 요소들을 개념에 따라 액터 컴포넌트로 분리하여 캐릭터에 추가할 수 있도록 하였습니다.
만약 새로운 게임 캐릭터를 추가하려는 계획이 있다면 필요에 따라 액터컴포넌트를 추가함으로써 쉽게 다양한 캐릭터를 만들 수 있습니다. 그리고 개념적으로 캐릭터에 대한 시스템을 분리하였기 때문에 유지보수면에서도 도움이 됩니다.

---

### 4) 게임플레이 태그
![image](https://github.com/muntory/MeleeCombatSystem/assets/38274669/e9d70a99-563d-4009-8064-7f4eb6851192)

게임플레이 태그는 오브젝트의 상태를 분류하고 설명하는 데 유용하게 사용되는 구조체로 오브젝트가 특정 게임플레이 태그를 소유하고 있는지 없는지에 따라 현재 상태를 나타낼 수 있습니다. 예를 들어 Character.State.Attacking 이라는 태그를 가지고 있다면 이 캐릭터의 현재 상태는 공격중이라는 것을 알 수 있습니다. 게임플레이 태그로 상태를 관리하면 이전과 마찬가지로 새로운 시스템을 추가하기 쉽습니다. 만약 전투 시스템을 더욱 심화시켜 보스 몬스터에 스턴 상태이상을 부여하고 스턴 상태에서 동작을 멈추게 하고자 하면 Monster.State.Stun 이라는 게임플레이 태그를 추가하고 몬스터의 상태를 나타내는 게임플레이 태그가 Monster.State.Stun 이라면 현재하는 동작을 멈추고 Stun에 관련된 로직을 수행하면 됩니다. 게임플레이 태그를 통하여 상태를 관리하면 기존에 bool이나 enum을 통하여 if 또는 switch문으로 관리하는 방법보다 확장이 용이합니다.
