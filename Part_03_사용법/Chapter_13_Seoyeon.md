# TypeSecipt 구성 옵션

<br/>

# tsc 옵션

자바스크립트에서 타입스크립트로 index.ts파일을 컴파일 하기 위해 tsc index.ts를 사용한다.  
tsc명령은 타입스크립트의 대부분 옵션을 —-플래그로 사용한다. 만약 index.ts파일에서 tsc를 실행할 때 index.ts 파일을 건너뛰려면 —noEmit 플래그를 전달하면된다.

```ts
tsc index.ts —noEdit
```

<br/>

## Pretty 모드

tsc CLI는 색상과 간격의 스타일을 지정해 가독성을 높이는 pretty모드(기본적용모드) 를 지원한다.  
만약 타입스크립트에 더 간결하고 색상이 없는 형식을 사용하도록 지시하고싶다면 아래의 명령어를 실행하면 된다.

```ts
tsc index.ts —pretty false
```

<br/>

## Watch 모드

해당 모드는 타입스크립트를 무기한 실행 상태로 유지하고, 모든 오류의 실시간 목옥을 가져와 터미널을 지속적으로 업데이트 할 수 있게 한다. (-w, -watch)  
해당 모드는 여러 파일에 걸쳐 리팩토링 같은 대규모 변경 작업을 진행할 때 유용하다.

```ts
tsc index.ts -w
```

<br/><br/>

# 타입스크립트 컴파일 설정

tsconfig.json은 타입스크립트를 자바스크립트로 변환 시키는 컴파일 설정을 한꺼번에 정의 해놓는 파일이라고 보면 된다.  
프로젝트를 컴파일 하는데 필요한 루트 파일과 컴파일러 옵션 등을 상세히 설정할 수 있으며, 보통 tsconfig.json 파일은 TypeScript 프로젝트의 루트 디렉토리(Root Directory)에 위치된다.  
그래서 tsconfig.json 파일이 프로젝트에 있다면 vscode는 우리가 타입스크립트로 개발한다는 것을 인식하게 되는 것이다.

<br/>

## tsconfig.json 생성하기

tsc 명령의 tsc —init 명령어를 사용한다.  
타입스크립트 프로젝트에서는 구성 파일을 생성하기 위해 상단 명령어를 사용하는것을 권장한다.

명령어를 사용하여 구성 파일을 생성하면 아래와 같이 완전 주석 처리된 tsconfig.json파일 생성이 된다.

```ts
{
	“compilerOptions” : {
		//…
	}
}
```

<br/>

# tsconfig 전역 속성

tsconfig 전역 속성이란 파일의 최상위에 위치하고 있는 속성들을 말한다.  
하단에 많은 전역 속성들이 있지만 주로 쓰이는 다섯가지 속성으로는 <b>compilerOptions, files, include, exclude, extends</b> 가 있다.

```ts
{
    // TypeScript 컴파일러의 옵션들을 지정
    "compilerOptions": {
        "target": "es5",
        "module": "commonjs",
        "strict": true,
        "sourceMap": true
        // ...
    },


    // 컴파일할 파일들의 개별 목록을 지정
    "files": ["src/main.ts", "src/utils.ts"],


    // 컴파일할 파일들을 지정
    "include": [ "src/**/*.ts" ],


    // 컴파일 대상에서 제외할 파일들을 지정
    "exclude": [ "node_modules", "**/*.test.ts" ],


    // 다른 tsconfig.json 파일을 상속받아서 설정을 재사용 가능하게 해줌
    "extends": "./configs/base.json",


    // 여러 개의 하위 프로젝트로 구성된 프로젝트의 의존 관계를 지정
    "references": [
        { "path": "./subproject1" },
        { "path": "./subproject2" }
    ],


    // 타입 습득과 관련된 옵션들을 지정
    "typeAcquisition": {
        "enable": true,
        "include": ["jquery"],
        "exclude": ["react"]
    },


    // watch 모드와 관련된 옵션들을 지정
    "watchOptions": {
        "watchFile": "useFsEvents",
        "watchDirectory": "useFsEvents",
        "fallbackPolling": "dynamicPriority"
    }
}
```

<br/>

## files

프로젝트에서 컴파일할 파일들의 목록을 명시적으로 지정하는 속성이다.

<br/>

## extends

다른 tsconfig.json 파일의 설정들을 가져와 재사용할 수 있게 해주는 옵션이다.

<br/>

## include

files 속성과 같이 프로젝트에서 컴파일할 파일들을 지정하는 속성이다.

<br/>

## exclude

프로젝트에서 컴파일 대상에서 제외할 파일들을 지정하는 속성이다.
include의 반대 버전이라 보면 된다.

<br/>

## compilerOptions

컴파일 대상 파일들을 어떻게 변환할지 세세히 정하는 옵션이다.

<br/><br/>

# 컴파일러 옵션 설명

## target

어떠한 버전의 JavaScript 코드로 컴파일 할지 지정한다. <br/>
만약 코드가 구식의 환경에서 배포된다면 더 낮은 버전을 지정해야 하고, 신식의 환경에서만 배포된다는 보장이 있다면 더 높은 타겟으로 지정해도 된다. (자바스크립트 버전 지정 값을 넣지 않으면 default로 es5로 컴파일 된다.)

다만 타입스크립트 소스 코드에 Promise 코드가 있을 경우,
이는 ES5에서 지원해주지 않기 때문에 Typescript를 컴파일 하면 오류가 발생하니 이부분은 유의해야 한다.
대부분의 브라우저는 모든 ES6를 지원하기 때문에, 보통 ES6로 놓고 사용되는 편이다.

```ts
"compilerOptions": {
	"target": "ES6" // 어떤 버전의 자바스크립트로 컴파일 될 것인지 설정
    // 'es3', 'es5', 'es2015', 'es2016', 'es2017','es2018', 'esnext' 가능
}
```

<br/>

## lib

lib 옵션은 컴파일에 필요한 JavaScript 내장 라이브러리를 지정할 수 있다.  
이 프로퍼티가 지정되어 있지 않다면 target 프로퍼티에 지정된 버전에 따라 필요한 타입 정의들에 대한 정보가 자동으로 지정된다.

lib 프로퍼티를 지정하지 않을 때 자동으로 설정되는 값은 다음과 같다.

- target이 <b>es3</b>이면 디폴트로 <b>lib.d.ts</b>를 사용
- target이 <b>es5</b>이면, 디폴트로 <b>dom, es5, scripthost</b>를 사용
- target이 <b>es6</b>이면, 디폴트로 <b>dom, es6, dom.iterable, scripthost</b>를 사용

<br/>

## jsx

.tsx 확장자의 컴파일 결과 JSX 코드를 어떻게 컴파일할지 결정한다.

```ts
"compilerOptions": {
	"jsx": "preserve" //'preserve', 'react-native', 'react'
}
```

<br/><br/>

# 모듈 옵션

## rootDir

루트 디렉토리 기준을 변경한다. js 아웃풋 경로에 영향을 줄 수 있다.

<br/>

## module / moduleResolution

프로그램에서 사용할 모듈 시스템을 결정한다.

<br/>

## baseURL / paths

import 구문의 모듈 해석 시에 기준이 되는 경로를 지정한다.
개발을 하다 보면 노드 패키지 이외에 직접 만든 소스 파일을 import 시켜야 하는 때가 발생하는데, 그럴 때 파일의 상단에 다음과 같이 전역경로 와 상대경로 기준으로 각각 import 하게 된다.

```ts
import styled from "styled-components";
// 노드 패키지일 경우 최상단 경로에 있는 node_modules 폴더를 자동 인식

import { TextField } from "../../../../components/textfield";
// 직접 만든 사용사 소스 파일이 있을 경우 상대경로로 가져와야 한다.
```

상대경로로 import 하는 이런 방식이 좀 예쁘진 않아도 일단 작동하는데는 문제가 없다.
하지만 추후 계속 프로젝트를 작성하거나 리팩토링 할 때 문제가 발생할 수 있는데, 상대 경로란 현재 위치하고 있는 파일 경로에 따라 얼마든지 달라질수가 있기 때문이다.

만일 다른 경로에 파일을 만들어서 동일한 모듈을 import 하려고 하면 위치 기준점이 달라지기 때문에 작업하는데 번거로울 수 있다.
그래서 baseUrl 속성 과 paths 속성을 설정하면 절대경로로 import 할 수 있게 되며 아래와 같이 절대경로로 깔끔하게 작성할 수 있다.

```ts
import styled from "styled-components";

import { TextField } from "@components/textfield";
```

<br/><br/>

# JavaScript Support 옵션

## allowJs

TypeScript 프로젝트에서 JavaScript 파일도 사용할 수 있도록 하는 설정이다.

```ts
"compilerOptions": {
	"allowJs": true,
    // true이면 JS 파일들도 타입스크립트 프로젝트에서 import가능
}
```

<br/>

## checkJs

ts 파일 뿐만 아니라, js 파일에 있는 에러도 보고하라는 옵션이다.
보통 allowJS 속성과 함께 사용된다.

```ts
"compilerOptions": {
    "allowJs": true, // js 파일들 ts에서 import해서 쓸 수 있는지
    "checkJs": true, // 일반 js 파일에서도 에러체크 여부
}
```

<br/><br/>

# Type Checking 옵션

## strict

타입스크립트의 각종 타입 체킹 동작을 전부 활성화한다.
사실상 이 옵션을 쓰지않는것은 곧 타입스크립트를 쓰지 않는 다는 것과 같아서 기본으로 true로 되어있다.

```ts
"compilerOptions": {
    "strict": true // 모든 엄격한 유형 검사 옵션 활성화
}
```

이 프로퍼티를 true로 지정하면 strict mode family 프로퍼티들을 전부 true로 지정하는 것과 동일하다.
만일 사소한 부분에도 너무 지나치게 빨간줄이 생기는게 불편하다면 선택적으로 몇 개의 strict mode family 프로퍼티를 false로 지정하는 식으로 설정할 수 있다.

<br/>

## noImplicitAny

명시적이지 않은 any 타입이 지정될 경우 표현식과 선언에 사용하면 에러 발생 시킨다.
예를들어 타입스크립트가 추론을 실패해서 any로 지정되면 빨간줄이 뜨게 되는데, 이때 직접 any라고 지정해야 빨간줄이 사라진다.
이때 이 옵션을 false로 지정하면 아래와 같이 any 타입을 명시적으로 지정 안해도 오류가 사라지게 된다.

<br/>

## suppressImplicitAnyIndexErrors

nolmplicitAny를 사용할 때 인덱스 객체에 인덱스 시그니처가 없는 경우 오류가 발생하는 것을 예외 처리 해준다.
