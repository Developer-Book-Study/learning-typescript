타입스크립트 사용법

# 선언 파일(declaration file)

TypeScript는 구현과 별도로 타입 형태를 선언할 수 있다. 
타입 선언은 파일 이름이 .d.ts 확장자로 끝나는 선언 파일에 작성되며, 선언 파일은 일반적으로 프로젝트 내에서 작성된다.
TypeScript 선언 파일 .d.ts는 TypeScript 코드의 타입 추론을 돕는 파일이다. 쉽게 말해 .d.ts파일은 type을 정의(declare)하기 위해 존재하는 파일이다.

<br/>


## 사용이유
.d.ts파일이 존재하지 않는다면 기존 JavaScript기반 라이브러리들은 TypeScript환경에서 사용할 수 있는 타입이 정해져있지 않아 타입 체킹에 어려움이 있다. 
.d.ts파일은 기존 JavaScript로 만들어진 모듈들을 TypeScript 환경에서도 사용할 수 있도록 따로 타입만 정리해서 넣어두어 타입 체킹의 문제점을 해결할 수 있다.

<br/>

## 사용 예시
선언 파일은 다른 TypeScript 파일과 마찬가지로 import해서 사용할 수 있다.
아래의 types.d.ts 파일은 index.ts 파일에서 사용하는 User 인터페이스를 내보낸다.

```ts
// types.d.ts
export interface User {
	name: string;
	age: number;
}
```

<br/>


```ts
// index.ts
import { User } from ‘./types’;
	
export const person: User = {
	name: ‘John’;
	age: 20;
}
```

<br/>
<br/>


# .d.ts 파일만들기

<br/>

## 이미 존재하는 .d.ts 불러오기

```ts
npm install module-name

// 타입 선언만 포함하는 모듈
npm install @types/module-name
```

존재하고 있는 .d.ts 파일이 없다면 직접 만들어서 사용해야한다.


<br/>


# .d.ts 파일의 내부

<br/>


## ambient context란?
.d.ts 파일에는 타입의 선언만 되어있어야 하며, 이렇게 값이 아닌 타입만 선언할 수 있는 코드 영역을 앰비언트 컨텍스트(amdient context)라고 한다.

<br/>


## declare란?
선언 파일의 또 다른 중요한 기능은 모듈의 상태를 설명하는 기능이다. 
모듈의 문자열 이름 앞에 declare 키워드를 사용하면 모듈의 내용을 타입 시스템에 알릴 수 있다.
다른말로, 컴파일러에게 해당 변수나 함수의 존재 여부를 알릴 수 있다는 뜻이다.
또햔 해당 내용들은 JavaScript코드로 컴파일 되지 않고, TypeScript 컴파일러에게 타입 정보를 알릴 수 있다.

<br/>

아래의 ‘my-example-lib’ 모듈은 modules.d.ts 선언 스크립트 파일에 존재하도록 선언한 다음 index.ts 파일에서 사용된다.
```ts
// modules.d.ts
declare module ‘my-example-lib’ {
	export const value: string;
}
```

<br/>

```ts
// index.ts
import { value } from ‘my-example-lib’;

console.log(value); //ok
```

<br/>

## namespace란?
namespace란 일종의 패키지 개념으로, 클래스나 인터페이스 또는 함수와 같은 내용들을 한 파일에서 그룹화하여 관리할 수 있게 해주는 요소이다.
TypeScript 1.5 이전버전에는 Internal modules라고 불렸다.

<br/>

# Definitely Typed
Definitely Typed는 짧게 줄여서 DT라고 불린다.
모든 유명한 npm 라이브러리를 가지고 있는 거대한 저장소이며, 이곳에서 TypeScript로 작업할 때 필요한 대부분의 라이브러리나 패키지 type definition(유형 정의)를 얻을 수 있다.
TypeScript의 타입 정의를 제공하는 repository서비스이기 때문에,  사용하는 라이브러리의 인터페이스가 이 repository에 등록되어 있다면 쉽게 내려받아 그 정의를 사용할 수 있다.






