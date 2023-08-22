# 타입 제한자
## top 타입
### any 타입
any 타입은 타입스크립트의 타입 검사 기능 자체를 활용하지 않도록 설정하는 타입입니다.
any 타입 변수에 어떤 메서드를 호출해도 오류를 표시하지 않습니다.
### unknown 타입
unknown 타입은 알 수 없는 타입입니다.
any 타입과 유사하지만 타입스크립트는 unknown 타입의 값을 훨씬 더 제한적으로 취급합니다.
```ts
let a: any = 123;
let u: unknown = 123;

let v1: boolean = a; // any 타입은 어디든 할당할 수 있습니다.

// unknown 타입은 모든 any 타입을 제외한 다른 타입에 할당할 수 없습니다.
let v2: number = u; // Error

let v3: any = u; // OK!

let v4: number = u as number; // 타입을 단언하면 할당할 수 있습니다.
```
unknown 타입은 다양한 타입을 반환할 수 있는 API에서 유용할 수 있습니다.
```ts
type Result = {
  success: true,
  value: unknown
} | {
  success: false,
  error: Error
}

export default function getItems(user: IUser): Result {
  // Some logic...
  if (id.isValid) {
    return {
      success: true,
      value: ['Apple', 'Banana']
    };
  } else {
    return {
      success: false,
      error: new Error('Invalid user.')
    }
  }
}
```
typeof 연산자로 타입을 좁혀서 unknown 타입을 제어할 수도 있습니다.
```ts
function greetComedianSafety(name: unknown) {
    if (typeof name === "string") {
        console.log(`Annowncing ${name.toUpperCase()}`);
    } else {
        console.log("Well, I'm off");
    }
}

greetComedianSafety("Betty White"); // "Annowncing BETTY WHITE"
greetComedianSafety({}); // "Well, I'm off"
```
### any vs unknown
어떤 값이 될 수 있음을 나타내려면 unknown 타입이 훨씬 안전합니다.
- 타입스크립트는 unknown 타입 값의 속성에 직접 접근할 수 없습니다.
- unknown 타입은 top 타입이 아닌 타입에는 할당할 수 없습니다.

위 두 가지 제한으로 인해 unknown이 any보다 안전한 타입입니다.
결론은 가능하면 any 대신 unknown을 사용하길 추천합니다.

## 타입 서술어
타입스크립트에는 인수가 특정 타입인지 여부를 나타내기 위해 boolean 값을 반환하는 함수를 위한 특별한 구문이 있습니다. 이를 **타입 서술어**라고 부르며 '사용자 정의 타입 가드'라고도 부릅니다.
```ts
function isNumberOrString(value: unknown): value is number | string {
  return ['number', 'string'].includes(typeof value);
}

function logValueIfExists(value: number | string | null | undefined) {
  if (isNumberOrString(value)) {
    // value: number | string
    value.toString(); // Ok
  } else {
    // value: null | undefined
    console.log("value does not exist:", value);
  }
}
```
## 타입 연산자
### keyof
인덱싱 가능 타입에서 keyof를 사용하면 속성 이름을 타입으로 사용할 수 있습니다.
인덱싱 가능 타입의 속성 이름들이 유니온 타입으로 적용됩니다.
```ts
interface ICountries {
  KR: '대한민국',
  US: '미국',
  CP: '중국'
}
let country: keyof ICountries; // 'KR' | 'US' | 'CP'
country = 'KR'; // ok
country = 'RU'; // Error - TS2322: Type '"RU"' is not assignable to type '"KR" | "US" | "CP"'.
```
### typeof
단일 변수의 type을 추출할 때도 typeof를 사용하지만 자바스크립트의 typeof와 다릅니다.
제공되는 값의 type을 반환합니다.
```ts
const original = {
    medium: "movie",
    title: "Mean Girls",
};

let adaptation: typeof original;

if (Math.random() > 0.5) {
    adaptation = {...original, medium: "play"};
} else {
    adaptation = {...original, medium: 2};
}
```
여기서 adaptation은 original의 타입을 그대로 사용합니다.
## 타입 어서션
```ts
const rawData = ["grace", "frankie"]

// 타입: any
JSON.parse(rawData);

// 타입: any
JSON.parse(rawData) as string[];

// 타입: any
JSON.parse(rawData) as [string, string];

// 타입: any
JSON.parse(rawData) as ["grace", "frankie"];
```
## const 어서션
as const를 사용해서 읽기 전용 튜플로 타입을 정의합니다.
리터럴 타입으로 정의합니다.
객체의 속성은 읽기 전용으로 간주됩니다.
```ts
// 타입: (number | string)[]
[0, ''];

// 타입 : [0, '']
[0, ''] as const;
```