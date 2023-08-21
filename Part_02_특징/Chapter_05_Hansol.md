# 함수
## 1. 함수 매개변수 타입 선언하기
### 매개변수의 타입 선언
js에서는 아래와 같이 함수를 사용했습니다.
```javascript
function sing(song) {
  console.log('Singing: ${song}!')
}
```
여기서 `song`이라는 매개변수가 어떤 타입인지 모르는 상황이 발생합니다.
```javascript
function sing(song: string) {
  console.log('Singing: ${song}!')
}
```
ts로 함수를 선언할 때 위와 같이 매개변수의 타입을 명시하면 오류없이 정확한 코드를 작성할 수 있고, 함수를 더 명확히 선언할 수 있는 장점이 있습니다.

### 필수 매개변수
js에서는 인수의 수와 상관없이 함수를 호출할 수 있습니다. 하지만 ts는 함수에 선언된 모든 매개변수가 필수라고 가정합니다.

인수의 개수가 맞지 않으면 에러를 표시합니다.
## 2. 선택적, 기본, 나머지 매개변수 선언하기
### 선택적 매개변수
js에서 함수 매개변수가 제공되지 않으면 함수 내부의 인숫값은 `undefined`로 기본값이 설정됩니다. ts에서도 이렇게 인수가 정의되지 않았을 때 인숫값을 `undefined`로 설정할 수 있습니다.
```typescript
function announceSong(song: string, singer?: string) {
  console.log('Song: ${song}');

  if (singer) {
    console.log('Singer: ${singer}');
  }
}

// 'singer' 인수를 정의하지 않은 경우
announceSong('GreenSleeves'); //OK
// 'singer' 인수를 undefined로 설정한 경우
announceSong('GreenSleeves', undefined); //OK
// 'singer' 인수를 string으로 설정한 경우
announceSong('Chandelier', 'Sia'); //OK
```
### 기본 매개변수
js에서 선택적 매개변수를 선언할 때 `=`와 값이 포함된 기본값을 제공할 수 있습니다. ts에서도 이 기능을 제공하지만 js와 다르게 기본값의 타입으로 타입을 유추합니다. 그리고 선택적 `기본값의 타입 | undefined`가 됩니다.
### 나머지 매개변수
...스프레드 함수를 사용하여 매개변수를 지정하는 경우입니다. 이때는 인수가 배열로 저장되므로 타입에 `[]`구문이 추가된다는 점만 다릅니다.
```typescript
function singAllTheSongs(singer: string, ...songs: string[]) {
  for (const song of songs) {
    console.log('${song}, by ${singer}');
  }
}

singAllTheSongs("Alicia Keys"); //OK
singAllTheSongs("Lady Gaga", "Bad Romance", "Just Dance", "Poker Face"); //OK
```
## 3. 함수 반환 타입 선언하기
기본적으로 함수의 return값을 통해 함수는 반환 타입을 추론합니다. 변수와 마찬가지로 함수의 반환 타입을 명시적으로 선언하지 않는 것이 좋지만 명시적으로 선언하는 방식이 유용한 경우가 종종 있습니다.
```typescript
const singSongsRecursive = (songs: string[], count = 0): number => songs.length ? singSongsRecursive(songs.slice(1), count + 1) : count;
```
## 4. void & never
어떤 것도 return하지 않는 함수의 반환 타입은 `void`입니다.
의도적으로 항상 오류를 발생시키거나 무한 루프를 실행하려면 반환 타입은 `never`입니다.
## 5. 함수 오버로드
‘함수 오버로드(Overloads)’는 이름은 같지만 매개변수 타입과 반환 타입이 다른 여러 함수를 가질 수 있는 것을 말합니다.
함수 오버로드를 통해 다양한 구조의 함수를 생성하고 관리할 수 있습니다.

아래 예제에서 add 함수는 2개의 선언부와 1개의 구현부를 가지고 있습니다.
주의할 점은 함수 선언부와 구현부의 매개변수 개수가 같아야 합니다.

```typescript
function add(a: string, b: string): string; // 함수 선언
function add(a: number, b: number): number; // 함수 선언
function add(a: any, b: any): any { // 함수 구현
  return a + b;
}

add('hello ', 'world~');
add(1, 2);
add('hello ', 2); // Error - No overload matches this call.
```