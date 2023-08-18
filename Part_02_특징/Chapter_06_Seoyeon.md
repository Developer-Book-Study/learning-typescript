# 배열                          
타입스크립트는 자바스크립트 처럼 값들을 배열로 다룰 수 있게 해준다.
만약 타입스크립트에서 초기에 빈 배열로 설정 된 변수에서 타입을 지정하지 않으면, 타입스크립트는 배열을 any[]로 취급하고 모든 콘텐츠를 받을 수 있다.
하지만 타입스크립트는 값의 타입을 알 때 가장 잘 작동하기때믄에 값 지정을 해주는것이 좋다.
많이 사용되는 배열 타입은 아래의 방법으로 사용할 수 있다.

<br/>

1.나타내는 타입 뒤에 []를 작성
```js
const numArr : number[] = [1,2,3];
const strArr : string[] = ['가','나','다'];
```

2.제네릭 배열 타입 사용
```js
const numArr : Array<number> = [1,2,3];
const strArr : Array<string> = ['가','나','다'];
```

3.유니언 배열 타입 사용
```js
const numArr : number | undefined[] = [1,2,undefined];
const numArr : (number | undefined)[] = [1,2,undefined];
```

<br/>


# 튜플
서로 다른 타입을 함께 가질 수 있는 배열이다.
요소들의 타입들이 모두 같을 필요는 없으며, 튜플 타입을 사용하면 요소의 타입과 개수가 고정 된 배열을 표현할 수 있다.

```js
const person : [string, number];

person = ['kim', 25];
person = [25, 'kim']; //error
```

<br/>


# 스프레드 문법

```js
// 타입 : string[]
const users = ['joan','tom','harriet'];

// 타입 : number[]
const userAges = [25,40,34];

// 타입 : string | number[]
const people = [...users, ...userAges];
```
