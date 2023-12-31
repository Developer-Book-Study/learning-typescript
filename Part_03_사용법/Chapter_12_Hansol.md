# IDE 기능 사용
## 1. 정의, 참조, 구현을 찾아 코드 탐색하기
기본적으로 변수나 타입 등에 우클릭을 해서 나오는 명령어 목록에 대해 소개합니다.
### 정의 찾기
우클릭 메뉴에서 `Go to Definition`입니다.

하지만 `cmd + 클릭`으로 주로 사용하고 더 편리합니다.

해당 변수나 함수가 정의된 위치를 찾는 것이기 때문에 매우 유용합니다.
### 참조 찾기
우클릭 메뉴에서 `Go to References`입니다.

해당 변수나 함수 또는 타입 등을 참조하고 있는 모든 곳을 표시해줍니다.
### 구현 찾기
우클릭 메뉴에서 `Go to Implementations`입니다. 해당 인터페이스 또는 추상 메서드의 모든 구현을 찾습니다.

여기서 구현이란 `implements` 키워드를 사용하여 생성된 인터페이스 또는 추상 메서드를 의미합니다.
## 2. 이름 완성 및 자동 가져오기로 코드 작성 자동화하기
타입스크립트 API를 이용하면 동일한 파일에 존재하는 이름을 자동 완성할 수 있습니다. 자동 완성 기능으로 오타를 줄일 수 있습니다.
자동 가져오기(자동 import) 기능을 사용하면 개발 생산성을 높이고 오타를 줄일 수 있습니다.

파일 이름을 바꾸거나 다른 폴더로 이동하는 경우 import문을 업데이트 해야 합니다. VS Code에서는 해당 파일 경로를 업데이트하도록 제안합니다.
## 3. 이름 바꾸기와 리팩터링을 포함한 추가 코드 액션
메뉴의 `Rename Symbol` 옵션을 선택하면 새 이름을 입력할 수 있습니다.

이 기능은 해당 함수, 인터페이스 또는 변수의 스펠링만 체크하여 변경하는 것이 아니라 동일한 스펠링이더라도 같은 기능이나 의미로 사용되고 있는 이름만 변경하는 것이기 때문에 매우 유용합니다.

`F2` 단축키를 이용하여 사용합니다.
## 4. 언어 서비스 오류를 확인하고 이해할 때 유용한 전략
오류 메시지가 표시되어 수정이 필요한 곳에서 가능한 수정 기능을 제안합니다.
아래와 같은 경우입니다.
- 클래스 또는 인터페이스에서 누락된 속성 선언하기
- 잘못 입력된 필드 이름 수정하기
- 타입으로 선언된 변수의 누락된 속성 채우기

이 때 [Quick Fix] 기능을 사용하면 오류를 해결하는 데에 생산성을 높일 수 있습니다.

`cmd + .` 단축키로 사용합니다.
## 5. 타입 이해에 유용한 전략
타입이 명확하게 표시되어 있지 않은 변수나 함수의 타입 정보를 확인할 수 있습니다.

마우스 호버를 통해 확인하고 `cmd + click`으로 해당 변수나 함수가 정의된 곳으로 이동할 수도 있습니다.