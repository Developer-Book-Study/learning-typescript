# Chapter. 2 타입 시스템

## Typescript Primitive Type

- null
  - e.g., null
- undefined
  - e.g., undefined
- boolean
  - e.g., true, false
- string
  - e.g., "Louise"
- number
  - e.g., 1337
- bigint
  - e.g., 1337n
- symbol
  - e.g., Symbol("Franklin");

<br><br>

## 할당 가능성

타입스크립트 변수의 동일한 타입의 다른 값이 할당될 때는 문제가 없음.
그러나 다른 타입의 값이 할당되면 타입 오류가 발생함.

> - 오류 없는 예시
>
> let firstName = "Carole";\
> firstName = "Joan";

> - 오류 발생 예시
>
> let firstName = "Carole";\
> firstName = true;

## Type Annotation

타입스크립트는 초깃값을 할당하지 않고도 변수의 타입을 선언할 수 있는 구문인 타입 애너테이션을 제공함. \
타입 애너테이션은 변수 이름 뒤에 배치되며 콜론과 타입 이름을 차례대로 기재.

Annotate는 "주석을 달다" 라는 사전적인 뜻을 가지고 있다.\
타입스크립트에서는 변수, 함수, 함수 반환값의 데이터 타입을 지정하기 위해 "타입 어노테이션"을 사용한다. 즉, 타입에 주석을 단다.\
한 번 식별자를 특정 타입으로 annotated 하면 해당 타입만 사용할 수 있다.

> let rocker: string;\
> rocker = "Joan Jett";

이러한 타입 애너테이션은 타입스크립트에만 존재하며 런타임 코드에 영향을 주지 않음.
만약 위 예시를 자바스크립트로 컴파일하면 아래처럼 컴파일됨.

> let rocker;\
> rocker = "Joan Jett";
