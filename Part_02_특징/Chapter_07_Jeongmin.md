# Chapter. 7 인터페이스

인터페이스는 타입 체크를 위해 사용되며, 변수, 함수, 클래스에 사용할 수 있다.
별칭으로 된 객체 type과 유사하지만 아래의 장점들도 인해 더 선호된다.

- 더 읽기 쉬운 오류 메시지
- 더 빠른 컴파일러 서능
- 클래스와의 더 나은 상호 운용성

## 타입 별칭 vs 인터페이스

```typescript
type Poet = {
  born: number;
  name: string;
};
```

```typescript
interface Poet = {
    born: number;
    name: string;
}
```

위 두 구문은 거의 같지만 인터페이스와 타입 사이에는 아래 몇 가지의 주요 차이점이 있다.

- 인터페이스는 속성 증가를 위해 병합(merge)할 수 있다.
- 인터페이스는 클래스가 선언된 구조의 타입을 확인하는데 사용할 수 있지만 타입 별칭은 사용할 수 없다. (8장에서 자세히)
- 인터페이스에서 타입스크립트 타입 검사기가 더 빨리 작동한다.
- 인터페이스는 이름 없는 객체 리터럴의 별칭이 아닌 이름 있는(명명된) 객체로 간주되므로 어려운 특이케이스에서 나타나는 오류 메시지를 좀 더 쉽게 읽을 수 있다.

### 읽기 전용 속성

경우에 따라 인터페이스에 정의된 객체의 속성을 재할당하지 못하도록 인터페이스 사용자를 차단하고 싶은 경우 속성 이름 앞에 readonly 키워드를 추가해 다른 값으로 설정되지 못하도록 할 수 있다. readonly 키워드가 붙은 속성은 새로운 값으로 재할당하지 못한다.

```typescript
interface Page {
  readonly text: string;
}

function read(page: Page) {
  console.log(page.text);
  page.text += "!"; // Error
}
```

<br>

## 호출 시그니처

호출 시그니처는 값을 함수처럼 호출하는 방식에 대한 타입 시스템의 설명으로써,
인터페이스와 객체 타입은 호출 시그니처로 선언할 수 있다.
호출 시그니처는 함수 타입과 비슷하지만 콜론(:) 대신 화살표(=>)로 표시한다.

```typescript
type FunctinonAlias => (input: string) => number;

interface CallSignature {
    (input: string): number;
}

const typeFunctinonAlias: FunctinonAlias = (input) => input.length
const typeCallSignature: CallSignature = (input) => input.length

```

<br>

## 인덱스 시그니처

인덱스 시그니처는 객체가 임의의 키를 받고, 해당 키 아래의 특정 타입을 반환할 수 있음을 나타낸다.

```typescript
interface WordCounts {
  [i: string]: number;
}

const counts: WordCounts = {};

counts.apple = 0;
counts.banana = 1;
counts.cherry = false; // Error
```

인덱스 시그니처는 객체에 값을 할당할 때 편리하지만 타입 안정성을 완벽하게 보장하지는 않는다.

```typescript
interface DatesByName {
  [i: string]: Date;
}

const publishDates: DatesByName = {
  Frankenstein: new Date("1 January 1818"),
};

publishDates.Frankenstein;
console.log(publishDates.Frankenstein.toString());

publishDates.Beloved;
console.log(publishDates.Beloved.toString());
// 타입 시스템에서는 오류가 나지 않지만 실제 런타임에서는 오류가 발생
```

<br>

## 중첩 인터페이스

```typescript
interface Novel {
    author: {
        name: string
    };
    setting: Setting;
}

interface Setting {
    place: string;
    year: number;
}

let myNover: Novel;

myNover = {
    author: {
        name: 'Jane Austen'
    }
    setting: {
        place: 'England',
        year: 1812
    }
}
```

<br>

## 인터페이스 확장

타입스크립트는 인터페이스가 다른 인터페이스의 모든 멤버를 복사해서 선언할 수 있는 확장된 인터페이스를 허용한다. 예를 들면 다른 인터페이스의 모든 멤버를 포함하고, 거기에 몇 개의 멤버가 추가된 인터페이스의 경우이다.

확장할 인터페이스의 이름 뒤에 extends 키워드를 추가해서 다른 인터페이스를 확장한 인터페이스라는 걸 표시한다.

```typescript
interface Writing {
    title: string:
}

interface Novella extends Writing {
    pages: Number;
}

let myNovella: Novella = {
    pages: 195,
    title: "Ethan Frome".
}
```

## 다중 인터페이스 확장

extends 키워드 뒤에 쉼표로 인터페이스 이름을 구분해 사용하면 된다.

```typescript
interface GivesNumber {
  giveNumber(): number;
}

interface GivesString {
  giveString(): string;
}

interface GivesBothAndEither extends GivesNumber, GivesString {
  giveEither(): number | string;
}

function useGivesBoth(instance: GivesBothAndEither) {
  instance.giveEither();
  instance.giveNumber();
  instance.giveString();
}
```

<br>

## 인터페이스 병합

두 개의 인터페이스가 동일한 이름으로 동일한 스코프에 선언된 경우,
선언된 모든 필드를 포함하는 더 큰 인터페이스가 코드에 추가된다.

```typescript
interface Merged {
  fromFirst: string;
}

interface Merged {
  fromSecond: number;
}

// 아래와 같다.
// interface Merged {
//   fromFirst: string;
//   fromSecond: number;
// }
```

인터페이스가 여러 곳에 선언되면 코드를 이해하기가 어려워지므로 가능하면 인터페이스 병합을 사용하지 않는 것이 좋다.

인터페이스 병합은 외부 패키지 또는 Window 같은 내장된 전역 인터페이스를 보강하는데 특히 유용하다.

<br>

### 이름이 충돌되는 멤버

병합된 인터페이스는 타입이 다른 동일한 이름의 속성을 여러 번 선언할 수 없다.
속성이 이미 인터페이스에 선언되어 있다면 나중에 병합된 인터페이스에서도 동일한 타입을 사용해야 한다.

```typescript
interface MergeProperties {
  same: (input: boolean) => string;
  dirrerent: (input: string) => string;
}

interface MergeProperties {
  same: (input: boolean) => string;
  dirrerent: (input: number) => string; // Error
}
```
