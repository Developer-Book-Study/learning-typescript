# Chapter. 4 객체

## 객체 타입

중괄호 { ... } 를 사용해서 객체를 생성.

```
const poet = {
    born: 1935,
    name: "Mary Oliver"
}
```

<br>

### 객체 타입 선언

객체의 타입을 명시적으로 선언

```
let poetLater: {
    born: number
    name: string
}

poetLater = {
    born: 1935,
    name: "Mary Oliver"
}

```

<br>

### 별칭 객체 타입

객체 타입에 타입 별칭을 할당해 사용하는 방법이 더 일반적

```
type Poet = {
    born: number;
    name: string;
}

let poetLater: Poet;

poetLater = {
    born: 1935,
    name: "Sara Teasdale"
}

```

> 대부분의 타입스크립트 프로젝트는 객체 타입을 설명할 때 인터페이스(interface) 키워드를 사용하는 것을 선호

<br>

### 선택적 속성

타입의 속성 애너테이션에서 : 앞에 ?를 추가하여 선택적 속성임을 나타낼 수 있음.

```
type Book = {
    author?: string;
    pages: number;
}

const ok: Book = {
    author: "Rita Dove",
    pages: 80
}

const ok2: Book = {
    pages: 40
}
```

<br><br>

## 객체 타입 유니언

### 유추된 객체 타입 유니언

```
const poem = Math.randoe() > 0.5
    ? { name : "The Double Image", pages: 7 }
    : { name : "Her Kind", rhymes: true };

poem.name // string
poem.pages // number | undefined
poem.rhymes // boolean | undefined
```

### 명시된 객체 타입 유니언

```
type PoemWithPages = {
    name: string;
    pages: number;
}

type PoemWithRhymes = {
    name: string;
    rhymes: boolean;
}

type Poem = PoemWithPages | PoemWithRhymes;

const poem: Poem = Math.random() > 0.5
    ? { name: "The Double Image", pages: 7 }
    : { name: "Her Kind", rhymes: true }

poem.name; // Ok
poem.pages; // error
poem.rhymes; // error

```

속성 name에 접근하는 것은 name 속성이 항상 존재하기 때문에 허용되지만\
pages와 rhymes는 항상 존재한다는 보장이 없기 때문에 에러가 발생.

<br>

### 객체 타입 내로잉

```
if ("pages" in poem) {
    poem.pages;
} else {
    poem.rhymes;
}
```

poem 예제에서 poem의 pages가 타입스크립트의 타입 가드 역할을 해 PoemWithPages임을 나타내는지 확인

<br>

## 교차 타입

타입스크립트 유니언 타입은 둘 이상의 다른 타입 중 하나의 타입이 될 수 있음을 나타냄.

```
type Artwork = {
    genre: string;
    name: string;
}

type Writing = {
    pages: number;
    name: string;
}

type WrittenArt = Artwork & Writing

// 아래와 같음
// {
//    genre: string;
//    name: string;
//    pages: number
// }

```

### never

never 키워드로 작성하면 never 타입이 됨.

```
type NotPossible = number & number

let notNumber: NotPossible = 0; // error
let notString: never = "" // error
```

never 키워드와 never타입은 bottom 타입 또는 empty 타입을 의미\
bottom 타입은 값을 가질 수 없고 참조할 수 없는 타입.

-> never 타입과 키워드는 값을 가질 수 없고 참조할 수 없다.
