# Chapter. 15 타입 운영

## 매핑된 타입

타입스크립트는 다른 타입의 속성을 기반으로 새로운 타입을 생성하는 구문을 제공한다.
즉, 하나의 타입에서 다른 타입으로 매핑한다.
타입스크립트의 매핑된 타입은 다른 타입을 가져와서 해당 타입의 각 속성에 대해 일부 작업을 수행하는 타입이다.

```typescript
type Animals = "alligator" | "baboon" | "cat";

type AnimalCounts = {
  [K in Animals]: number;
};

// 다음과 같음
// {
//     alligator: number,
//     baboon: number,
//     cat: number
// }
```

keyof 연산자를 사용해 키를 가져오는 방식으로도 작동.

```ts
interface AnimalVariants {
  alligator: boolean;
  baboon: number;
  cat: string;
}

type AnimalCounts = {
  [K in keyof AnimalVariants]: number;
};
// 다음과 같음
// {
//     alligator: number,
//     baboon: number,
//     cat: number
// }
```

<br>

## 제한자 변경

매핑된 타입은 원래 타입의 멤버에 대해 접근 제어 제한자인 READONLY와 ?도 변경 가능하다.

```ts
interface Envirmentalist {
    area: string;
    name: string;
}

type ReadonlyEnvironmentalist {
    readonly [K in keyof Environmentalist]: Environmentalist[K]
}

// 다음과 같음
// {
//     readonly area: string;
//     readonly name: string;
// }

type OptionalReadonlyEnvironmentalist = {
    [K in keyof ReadonlyEnvironmentalist]?: ReadonlyEnvironmentalist[K]
};
// 다음과 같음
// {
//     readonly area?: string;
//     readonly name?: string;
// }
```

새로운 타입의 제한자 앞에 -를 추가해 제한자를 제거할 수 있다.

```ts
interface Conservationist {
  name: string;
  catchphrase?: string;
  readonly born: number;
  readonly died?: number;
}

type WritableConservationist = {
  -readonly [K in keyof Conservationist]: Conservationist[K];
};

// 다음과 같음
// {
//   name: string;
//   catchphrase?: string?
//   born: number;
//   died?: number
// }

type RequiredWritableConservationist = {
  [K in keyof WritableConservationist]-?: WritableConservationist[K];
};

// 다음과 같음
// {
//   name: string;
//   catchphrase: string;
//   born: number;
//   died: number;
// }
```

<br>

## 조건부 타입

조건부 타입의 개념은 기존 타입을 바탕으로 두 가지 가능한 타입 중 하나로 확인되는 타입이다.
조건부 타입 구문은 삼항 연산자 조건문처럼 보인다.

다음 CheckStringAgainstNumber 조건부 타입은 string이 number가 되는지 여부를 검사한다.
string타입을 number 타입에 할당할 수 있는지 여부를 확인하고 할당할 수 없다면 타입은 false가 된다.

```ts
type CheckStringAgainstNumber = string extends number ? true : false;
```

### 제네릭 조건부 타입

```ts
type CheckAgainstNumber<T> = T extends number ? true : false;

// 타입: false
type CheckString = CheckAgainstNumber<"parakeet">;

// 타입: true
type CheckString = CheckAgainstNumber<1891>;

// 타입: true
type CheckString = CheckAgainstNumber<number>;
```

<br>

## never

### never와 교차, 유니언 타입

```ts
type NeverIntersection = never & string; // 타입 : never
type NeverUnion = never | string; // 타입 : string
```

> 교차 타입(&)에 있는 never는 교차 타입을 never로 만든다.\
> 유니언 타입(|)에 있는 never는 무시한다.

<br>

## 템플릿 리터럴 타입

템플릿 리터럴 타입은 문자열 타입이 패턴에 맞는지를 나타내는 타입스크립트 구문이다.

```ts
type Greeting = `Hello ${string}`;

let matches: Greeting = "Hello, world!";

let outOfOrder: Greeting = "World! Hello!";
// Error: Type '"World! Hello!"' is not assignable to type `Hello ${string}`.

let missingAltogethe: Greeting = "hi"; // Error
```

### 고유 문자열 조작 타입

- Uppercase : 문자열 리터럴 타입을 대문자로 변환
- Lowercase : 문자열 리터럴 타입을 소문자로 변환
- Capitalize : 문자열 리터럴 타입의 첫 번째 문자를 대문자로 변환
- Uncapitalize : 문자열 리터럴 타입의 첫 번째 문자를 소문자로 변환

```ts
type FormalGreeting = Capitalize<"hello">; // 타입: "Hello"
```

### 템플릿 리터럴 키

```ts
type DataKey = "location" | "name" | "year";

type ExistenceChecks = {
  [K in `check${Capitalize<DataKey>}`]: () => boolean;
};
// 다음과 같음
// {
//   checkLocation: () => boolean;
//   checkName: () => boolean;
//   checkYear: () => boolean;
// }
```
