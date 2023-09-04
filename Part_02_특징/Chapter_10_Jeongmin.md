# Chapter. 10 제네릭

타입을 직접적으로 고정된 값으로 명시하지말고 '변수' 를 통해 언제든지 변할 수 있는 타입을 통해 보다 유연하게 코딩을 할 수 있는 장치가 필요한데 이것이 제네릭이다.
간단하게 말하자면 타입을 변수화한 것이다. 타입을 마치 함수의 파라미터처럼 사용하는 것을 의미한다.
매개변수 괄호 바로 앞 홑화살괄호(<,>)로 묶인 타입 매개변수에 별칭을 배치해 함수를 제네릭으로 만든다.

```Typescript
function identity<T>(input: T) {
    return input;
}

const numeric = identity("me"); // 타입: "me"
const stringy = identity(123); // 타입: 123
```

<br>

## 제네릭 인터페이스

다음 Box 선언은 속성에 대한 T타입 매개변수가 있다. 타입 인수로 Box로 선언된 객체를 생성하면 inside의 T속성이 해당 타입 인수와 일치된다.

```Typescript
interface Box<T> {
    inside: T;
}

let stringyBox: Box<string> = {
    inside: "abc"
}

let numberBox: Box<number> = {
    inside: 123,
}

let incorrectBox: Box<number> = {
    inside: false,
}
```

### 유추된 제네릭 인터페이스 타입

```Typescript
interface LinkedNode<Value> {
    next?: LinkedNode<Value>;
    value: Value
}

function getLast<Value>(node: LinkedNode<Value>): Value {
    return node.next ? getLast(node.next) : node.value;
}

// 유추된 Value 타입 인수: Date
let lastDate = getLast({
    value: new Date("09-13-1993"),
})

// 유추된 Value 타입 인수: string
let lastFruit = getLast({
    next: {
        value: "banana",
    },
    value: "apple",
})

// 유추된 Value 타입 인수: number
let lastMismatch = getLast({
    next: {
        value: 123
    },
    value: false
})
```

<br>

## 제네릭 클래스

Secret 클래스는 Key와 Value 타입 매개변수를 선언한 다음 이를 멤버 속성, constructor 매개변수 타입, 메서드의 매개변수, 반환 타입으로 사용한다.

```Typescript
class Secret<Key, Value> {
    key: Key;
    value: Value

    constructor(key: Key, value: Value) {
        this.key = key;
        this.value = value;
    }

    getValue(key: key): Value | undefined {
        return this.key === key
        ? this.value
        : undefined
    }
}

const stroage = new Secret(12345, "luggage"); // 타입: Secret<number, string>
storage.getValue(1987) // 타입: string | undefined
```

### 제네릭 클래스 확장

```Typescript
class Quote<T> {
    lines: T;

    constructor(lines: T) {
        this.lines = lines;
    }
}

class SpokenQuote extends Quote<string[]> {
    speak() {
        console.log(this.lines.join("\n"));
    }
}

new Quote("The only real failure is the failure to try.").lines; // 타입: string
new Quote([4, 8, 15, 16, 23, 42]).lines; // 타입: number[]

new SpokenQuote([
    "Greed is so destructive.",
    "It destroy everything",
]).lines; // 타입 : string[]

```

### 메서드 제네릭

```Typescript
class CreatePairFactory<Key> {
    key: Key;

    constructor(key: Key) {
        this.key = key;
    }

    createPair<Value>(value: value) {
        return { key: this.key, value };
    }
}

// 타입: CreatePairFactory<string>
const factory = new CreatePairFactory("role");

// 타입: { key: string, value: number }
const numberPair = factory.createPair(10);

// 타입: { key: string, value: string }
const stringPair = factory.createPair("Sophie");
```

<br>

## 제네릭 제약 조건(extends)

> 제네릭의 extends는 인터페이스나 클래스의 extends 와 약간 정의가 다르다.
> 클래스의 extends는 상속의 의미로서 '확장' 의 정의를 가지지만, 제네릭의 extends는 '제한' 의 의미를 가진다는 차이점이 있다.
> 따라서 <T extends K> 형태의 제네릭이 있다면, T가 K에 할당 가능해야 한다 라고 정의하면 된다.

````typescript
type numOrStr = number | string;

// 제네릭에 적용될 타입에 number | string 만 허용
function identity<T extends numOrStr>(p1: T): T {
  return p1;
}

identity(1);
identity("a");

identity(true); //! ERROR
identity([]); //! ERROR
identity({}); //! ERROR```
````

<br>

## typeof 연산자

typeof는 객체 데이터를 객체 타입으로 변환해주는 연산자로써,
객체에 쓰인 타입 구조를 그대로 가져와 독립된 타입으로 만들어 사용하고 싶다면, 앞에 typeof 키워드를 명시해주면 해당 객체를 구성하는 타입 구조를 가져와 사용할 수 있다.

```typescript
const obj = {
  red: "apple",
  yellow: "banana",
  green: "cucumber",
};

// 위의 객체를 타입으로 변환하여 사용하고 싶을때
type Fruit = typeof obj;
/*
type Fruit = {
    red: string;
    yellow: string;
    green: string;
}
*/

let obj2: Fruit = {
  red: "pepper",
  yellow: "orange",
  green: "pinnut",
};
```

## keyof 연산자

객체 형태의 타입을, 따로 속성들만 뽑아 모아 유니온 타입으로 만들어주는 연산자이다.

```typescript
type Type = {
  name: string;
  age: number;
  married: boolean;
};

type Union = keyof Type;
// type Union = name | age | married

const a: Union = "name";
const b: Union = "age";
const c: Union = "married";
```

만일 obj 객체의 키값인 red, yellow, green을 상수 타입으로 사용하고 싶을 때는 typeof obj 자체에 keyof 키워드를 붙여주면 된다.

```typescript
const obj = { red: "apple", yellow: "banana", green: "cucumber" } as const; // 상수 타입을 구성하기 위해서는 타입 단언을 해준다.

// 위의 객체에서 red, yellow, green 부분만 꺼내와 타입으로서 사용하고 싶을떄
type Color = keyof typeof obj; // 객체의 key들만 가져와 상수 타입으로

let ob2: Color = "red";
let ob3: Color = "yellow";
let ob4: Color = "green";
```

### 제네릭 활용

만일 함수의 매개변수 key가 반드시 매개변수 obj의 제네릭 타입 T(객체를 받게되는)에 존재하여야 할때, keyof T 를 하면 객체의 key 값을 모아 유니온 타입으로 만들 수 있다.

```typescript
function getProperty<T, K extends keyof T>(obj: T, key: K) {
  return obj[key];
}

let x = { a: 1, b: 2, c: 3, d: 4 };

getProperty(x, "a"); // 성공
getProperty(x, "m"); // 오류: 인수의 타입 'm' 은 'a' | 'b' | 'c' | 'd'에 해당되지 않음.
```
