# 구문 확장(Syntax Extensions)
## 1. 클래스 매개변수 속성(class parameter property)
### 알아둬야 할 점
- 클래스를 많이 사용하는 프로젝트나 클래스 이점을 갖는 프레임워크가 아니라면 클래스 매개변수 속성을 사용하지 않는 것이 좋습니다.
### 장점
매개변수 속성을 사용하면 속성을 명시적으로 할당하기 위한 코드를 줄일 수 있습니다.
- 매개변수 속성을 사용하지 않았을 때
```ts
class NamedEngineer {
  // 일반 property
  fullName: string;
  area: string;

  constructor(
    // 일반 parameter
    name: string,
    area: string,
  ) {
    this.area = area;
    this.fullName = `${name}, ${area} engineer`;
  }
}
```
- 매개변수 속성을 사용했을 때
```ts
class NamedEngineer {
  // 일반 property
  fullName: string;

  constructor(
    // 일반 parameter
    name: string,
    // parameter property
    public area: string,
  ) {
    this.fullName = `${name}, ${area} engineer`;
  }
}
```
## 2. 인수 클래스와 필드에 데코레이터 사용하기
- 데코레이터 기능은 ECMA스크립트에서 아직 승인하지 않은 기능이므로 타입스크립트에서는 `experimentalDecorators`옵션을 통해 활성화할 수 있습니다.
- Angular, NestJS와 같은 프레임워크에서는 해당 프레임워크 사용법에 맞는 데코레이터 사용법을 알려줍니다.
- 자신만의 데코레이터를 작성하는 것은 부적절합니다.
## 3. 열거형(Enum)
자바스크립트에서는 Enum 구문을 포함하지 않으므로 Enum을 배치해야 하는 곳에 일반적인 객체를 사용합니다.

Enum은 숫자 혹은 문자열 값 집합에 이름(Member)을 부여할 수 있는 타입으로, 값의 종류가 일정한 범위로 정해져 있는 경우 유용합니다.

기본적으로 0부터 시작하며 값은 1씩 증가합니다.
```ts
enum Week {
  Sun,
  Mon,
  Tue,
  Wed,
  Thu,
  Fri,
  Sat
}

console.log(Week.Sun) // 0
console.log(Week.Mon) // 1
console.log(Week.Tue) // 2
// ...
```
수동으로 값을 변경할 수 있으며, 값을 변경한 부분부터 다시 1씩 증가합니다.
```ts
enum Week {
  Sun,
  Mon,
  Tue = 24,
  Wed,
  Thu,
  Fri,
  Sat
}

console.log(Week.Sun) // 0
console.log(Week.Mon) // 1
console.log(Week.Tue) // 24
console.log(Week.Wed) // 25
// ...
```
Enum 타입의 재미있는 부분은 역방향 매핑(Reverse Mapping)을 지원한다는 것입니다.

이것은 열거된 멤버(Sun, Mon 같은)로 값에, 값으로 멤버에 접근할 수 있다는 것을 의미합니다.
```ts
enum Week {
  // ...
}
console.log(Week);
console.log(Week.Sun); // 0
console.log(Week['Sun']); // 0
console.log(Week[0]); // 'Sun'
```
추가로, Enum은 숫자 값 열거뿐만아니라 다음과 같이 문자열 값으로 초기화할 수 있습니다.

이 방법은 역방향 매핑(Reverse Mapping)을 지원하지 않으며 개별적으로 초기화해야 하는 단점이 있습니다.
```ts
enum Color {
  Red = 'red',
  Green = 'green',
  Blue = 'blue'
}
console.log(Color.Red); // red
console.log(Color['Green']); // green
```
## 4. 파일 또는 타입 정의에 그룹화 생성을 위해 네임스페이스 사용하기
### 용어 정의
namespace는 module과 비교됩니다. 타입스크립트 1.5에서 중요한 명명법 변경이 있었습니다. “내부 모듈(Internal modules)”은 “namespace”가 되었습니다. “외부 모듈(External modules)”은 간단하게 “모듈(modules)”이 되어 ECMAScript 2015의 용어와 맞췄습니다.

하지만 결론부터 말하자면 ECMA스크립트에서는 namespace보다 module을 선호합니다. 아마 대부분의 타입스크립트 개발자들은 module을 사용하고 있을 것입니다.

타입스크립트 개발팀에서도 완벽하게는 아니지만 namespace를 module로 마이그레이션하는 작업을 진행했으며, namespace에서 다양한 이슈가 발생했기 때문에 해당 작업을 진행했습니다.

### 참고 : 외부 모듈과 내부 모듈
- 외부 모듈

외부 모듈이란 파일 내 export 문을 갖는 파일을 의미하며, 외부 모듈은 import 문을 사용하여 모듈을 불러옵니다.

export나 import 문이 존재하는 타입스크립트 파일은 외부 모듈로 인식되며, 각 파일마다 자체적인 전역 스코프를 생성하기 때문에 전역 스코프에 영향을 주지 않습니다.

- 내부 모듈

내부 모듈은 타입스크립트의 Namespace를 말합니다. Namespace는 외부 모듈과 달리 파일마다 자체적인 스코프를 생성하지 않고, 하나의 전역 스코프를 사용합니다.

이때 하나의 전역 스코프 내에서 스코프 역할을 하는 그룹을 Namespace라고 합니다. 즉, 하나의 파일 내 Namespace들로 스코프를 구분하도록 합니다.