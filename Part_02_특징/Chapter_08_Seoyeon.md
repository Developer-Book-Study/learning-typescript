TypeScript class관련 정리
ES6에 존재하는 class 키워드를 이용하여 구현할 수 있다. 아래와 같이 class 키워드 뒤에 class 이름을 적어준다.


# 클래스 키워드

```ts
class Greeter {
    greet(){
    // ...
    }
}
```

string 타입의 단일 필수 매개변수를 갖는 greet 클래스 메서드를 가진 Greeter 클래스 정의 코드.

```ts
class Greeter {
    greet(name: string){
        console.log('${name}, hello!'); 
    }
}

new Greeter().greet('seoyeon'); // Ok
new Greeter().greet(); // Error: Expected 1 arguments, but got 0.
```




# 클래스 속성
typescript에서 클래스의 속성을 읽거나 쓰려면 클래스에 명시적으로 선언해야한다.
만약 사전에 등록되지 않은 프로퍼티에 접근하려고 한다면 아래와 같이 에러를 발생시킨다.


```ts
class Greeter {
    greet(name: string){
        this.name = name;
        // Error : Property 'name' does not exist on type 'Greeter'.
  }
}
```

에러발생을 막으려면 다음과 같이 코드를 작성하면된다.

```ts
class Greeter {
    name: string;
    greet(name: string){
        this.name = name;
    }
}
```




# 클래스 상속
typescript는 ES6 문법에서와 마찬가지로 클래스 상속에 extends 키워드를 사용한다.
상속계층은 Button이 Input을 상속받고있으며, 하위클래스에서 constructor를 정의하고싶다면 반드시 상위클래스의 constructor를 호출해야한다. 
super(name)을 보면 매개변수도 동일하게 전달하는것을 확인할 수 있다.


```ts
class Input {
    name: string;
    constructor(name: string) {
        this.name = name;
    }
    inputName() {
        console.log(`input name is ${ this.name }`);
    }
}

class Button extends Input {
    constructor(name: string) {
        super(name);
        // constructor를 덮어쓰기 위해서는 super() 실행이 필요
    }
}

const button = new Button('click me');
button.inputName(); // input name is click me
```



# 읽기 전용 속성
프로퍼티 앞에 readonly를 붙이면 읽기 전용 프로퍼티로 만들어준다.
readonly륿 붙이면 constant(상수)로 인식되며, 생성자에서 한 번 결정 된 후에는 변경 불가하다.


```ts
class Person {
    readonly name: string;
    constructor(name: string) {
        this.name = name;
    }
}

const person = new Person('heecheolman');

// person.name = 'kimheecheol'; Error!
// TS2540: Cannot assign to 'name' because it is a read-only property.
```


매개변수 프로퍼티를 이용하면 선언과 할당을 동시에 할 수 있다.

```ts
class Person {
    constructor(readonly name: string) {}
    greet() {
        console.log(`my name is ${ this.name }`);
    }
}

const person = new Person('heecheolman');
```




# 인터페이스와 클래스

## 클래스 이행 규칙
인터페이스는 클래스와 비슷한데, 클래스와 달리 정의만 할 뿐 실제로 구현되지는 않는다. 
즉, 어떠한 객체를 생성 했을 때 가져야 할 속성이나 메서드를 정의한다고 보면 된다.


```ts
// 인터페이스 User 정의
interface User {
    name: string;
    age: number;
}

class Person implements User {
    constructor (
        public name: string,
        public age: number,
    ){}
}

const user1 = new Person('kim', 30);
```

### Implements란?
Implements 키워드는 class의 interface에 만족하는지 여부를 체크할 때 사용한다. 
만약 implements한 interface의 타입이 없다면 에러를 반환한다.
implements는 오직 타입 체크를 위해 사용되며, 안의 값을 자동으로 바꾸어주지는 않는다.




# 클래스 확장
typescript는 다른 클래스를 확장하거나 하위 클래스를 만드는 javascript 개념에 타입 검사를 추가한다.
아래의 예제에서 Teacher는 StudentTeacher 하위 클래스의 인스턴스에서 사용할 수 있는 teach메서드를 선언한다.

```ts
class Teacher {
    teach() {
        console.log('hello');
    }
}

class StudentTeacher extends Teacher {
    learn() {
        console.log('hi');
    }
}

const teacher = new StudentTeacher();

teacher.teach(); // OK(기본 클래스에 정의 됨)
teacher.learn(); // OK(하위 클래스에 정의 됨)

teacher.something(); 
// Error: Property 'something' does not exist on type 'StudentTeacher'.
```



# 추상 클래스
typescript에서 때로는 일부 메서드의 구현을 선언하지 않고, 대신 하위 클래스가 해당 메서드를 제공할 것을 예상하고 기본 클래스를 만드는 방법이 더 유용할 수 있다.
이러한 상황에서는 추상화하려는 클래스 이름과 메서드 앞에 typescript의 abstract 키워드를 추가한다.
추상화 메서드 선언은 추상화 기본 클래스에서 메서드의 본문을 제공하는 것을 건너뛰고, 대신 인터페이스와 동일한 방식으로 선언된다. 

```ts
abstract class School {
    readonly name: string;

    constructor(name: string) {
        this.name = name;
    }
.
.
.
```



# 클래스 멤버 접근성
## 접근 제어자 

- public(기본값) : typescript의 기본적인 모든 멤버 접근권함, 모든 곳에서 누구나 접근 가능
- private : 클래스 내부에서먄 접근 가능
- protected : 클래스 내부 또는 하위 클래스에서만 접근 가능
