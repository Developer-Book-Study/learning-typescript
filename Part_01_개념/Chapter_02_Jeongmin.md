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
> let firstName = "Carole";
>
> firstName = "Joan";

> - 오류 발생 예시
>
> let firstName = "Carole";
>
> firstName = true;

##
